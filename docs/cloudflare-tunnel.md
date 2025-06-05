# Cloudflare Tunnel for NutriFinder API

To securely expose my self-hosted NutriFinder API to the public internet, I used **Cloudflare Tunnel**. This method allows access via HTTPS to a private Docker container running on my homelab machine without opening any ports or requiring a static IP.

This was done as part of an extended examination deployment for the NutriFinder project. While our report and demo were finalized earlier, I chose to continue working on the hosting setup voluntarily — to take the project to the next level, deepen my DevOps skills, and complete the learning journey. The domain `api.mtbonde.dev` was originally created for the exam week, but will later be repurposed for hosting a production-ready version of the API. This documentation also reflects the full set of DevOps preparations I undertook as part of that exam week, including DNS, GitHub Actions, DockerHub, and runtime verification.

---

## Why use a tunnel?

Most residential ISPs in Denmark block inbound connections or place users behind **Carrier-Grade NAT (CGNAT)**. This makes traditional port forwarding either unreliable or impossible.

Cloudflare Tunnel bypasses this by:

* Initiating an outbound encrypted connection to Cloudflare
* Allowing secure routing from a domain (e.g. `api.mtbonde.dev`) to a local service (e.g. `localhost:5000`)
* Enabling HTTPS with zero configuration

---

## Step-by-Step Setup

### 1. Install `cloudflared` on homelab

```bash
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
cloudflared --version
```

### 2. Login to Cloudflare

```bash
cloudflared tunnel login
```

* Authorize access in browser
* Select zone `mtbonde.dev`
* `cert.pem` saved to `~/.cloudflared/`

### 3. Create the tunnel

```bash
cloudflared tunnel create nutrifinder-tunnel
```

### 4. Configure the tunnel

```bash
nano ~/.cloudflared/config.yml
```

Contents:

```yaml
tunnel: nutrifinder-tunnel
credentials-file: /home/USERNAME/.cloudflared/<tunnel-id>.json

ingress:
  - hostname: api.mtbonde.dev
    service: http://localhost:5000
  - service: http_status:404
```

> Replace `<tunnel-id>.json` with actual filename from tunnel creation
> Replace `USERNAME` with your system username

### 5. Route DNS

```bash
cloudflared tunnel route dns nutrifinder-tunnel api.mtbonde.dev
```

> This adds a CNAME entry in Cloudflare DNS automatically

### 6. Run the tunnel

```bash
cloudflared tunnel run nutrifinder-tunnel
```

Test: [https://api.mtbonde.dev](https://api.mtbonde.dev)

### 7. Run as systemd service

Create systemd unit:

```bash
sudo nano /etc/systemd/system/cloudflared.service
```

Content:

```ini
[Unit]
Description=Cloudflare Tunnel
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/cloudflared tunnel run nutrifinder-tunnel
Restart=on-failure
User=USERNAME

[Install]
WantedBy=multi-user.target
```

Reload and enable:

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable cloudflared
sudo systemctl start cloudflared
```

Check status:

```bash
systemctl status cloudflared
```

---

## Result

```bash
curl https://api.mtbonde.dev/api/nutrition?foodItemName=banana
```

for browser:

```bash
https://api.mtbonde.dev/api/nutrition?foodItemName=banana
```

Returns:

```json
{
  "foodItemName": "banana",
  "carb": 0,
  "fiber": 1.63,
  "protein": 1.14,
  "fat": 0.20,
  "kcal": 93.47
}
```

The API is now available globally with HTTPS, without any router changes or firewall configuration.

---

## What's next

Next step would be a HTML-based client.

---

## Reflections

This tunnel setup taught me more than I expected:

* It showed how to deal with real-world constraints like CGNAT
* I gained confidence in system-level service management and automation
* It felt satisfying to complete the lifecycle of a project — from school report to live, hosted API

This was the first time I made something accessible online in a secure and reproducible way.
