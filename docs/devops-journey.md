# Homelab DevOps Journey – NutriFinder Deployment

## Overview

This document captures the technical and personal journey of deploying the NutriFinder API from scratch in a self-hosted homelab environment. The goal was to create a secure, resilient, and reproducible setup using Docker, GitHub Actions, and DNS tunneling—all hosted locally.

---

## Motivation

The motivation for this project was to take my exam project—NutriFinder—and follow it through all the way to a production-ready, publicly hosted API. I wanted to learn what it takes to host an API from my own homelab and challenge myself to publish something live on the internet.

NutriFinder was a good candidate: small, self-contained, and ideal for DevOps experimentation. It gave me a focused opportunity to learn about:

* Hosting an API on local infrastructure.
* Registering and configuring a domain (`mtbonde.dev`).
* Setting up DNS and reverse proxy with Cloudflare.
* Using GitHub Actions as an alternative to Azure DevOps.
* Building CI/CD pipelines with DockerHub and Watchtower.
* Managing services in Linux and automating startup.

The process was both fun and educational. It introduced me to multiple topics—such as CI/CD on Github, domain management, tunneling, and automation—while also giving me hands-on experience working with Linux and maintaining a self-hosted server.

---

## Milestones and Achievements

### Domain & DNS

* Purchased `mtbonde.dev` via Namecheap.
* Set up Cloudflare for DNS management.
* Created subdomain `api.mtbonde.dev` for API exposure.
* Enabled HTTPS and proxying via Cloudflare.

### GitHub Integration

* Moved NutriFinder source code to GitHub.
* Created multi-stage Dockerfiles for both client and server.
* Set up GitHub Actions CI pipeline:

    * Build and test on push to `main` or `dev`.
    * Build and push Docker image to DockerHub.

### Homelab Setup

* Installed Docker and Docker Compose on local server.
* Installed Watchtower for automatic container updates.
* Created `docker-compose.yml` defining MongoDB and NutriFinder.Server.
* Added necessary labels and network configuration.
* Verified container startup and autostart behavior using `docker ps` and reboot testing.

### Cloudflare Tunnel

* Installed `cloudflared` manually on Ubuntu.
* Logged in and created named tunnel `nutrifinder-tunnel`.
* Wrote `~/.cloudflared/config.yml` with `api.mtbonde.dev` as hostname mapping to `localhost:5000`.
* Set DNS routing via `cloudflared tunnel route dns`.
* Created manual systemd service to persist tunnel after reboot.
* Validated service status and live access from external network.

### Public API Access and Verification

* Attempted port forwarding (did not work reliably).
* Successfully switched to Cloudflare Tunnel for secure exposure.
* Validated HTTPS access to `https://api.mtbonde.dev` externally.
* Had a friend run containerized client image: `mtbonde/nutrifinder-client:6`.
* Successfully queried API endpoint and verified full system integration.

---

## Try It Yourself

If you want to test the NutriFinder API client against the live, self-hosted server at `https://api.mtbonde.dev`, you can do so using Docker:

```bash
docker run -it --rm mtbonde/nutrifinder-client:6
```

This will pull the prebuilt Docker image of the CLI client and start it in interactive mode. You can then enter a food item (e.g., `banana`) and receive nutritional data returned from the API.

---

## Challenges and Learnings

* Initial GitHub Actions failure due to incorrect directory paths in WAF setup.
* Required proper handling of user permissions (`usermod -aG docker`) and relogin for Docker.
* Watchtower needed specific Docker label configuration to target NutriFinder API.
* Cloudflare Tunnel setup required debugging and manual systemd creation due to `.deb` installation.

Key lessons:

* Declarative Compose files support rapid redeployment.
* Proper labels and logging make Watchtower predictable.
* DNS-based tunneling is a reliable alternative to port forwarding.
* CI/CD works best when supported by clear versioning strategy.
* Linux system management and service automation are essential for reliable uptime.

---

## Possible Next Steps

* Implement semantic versioning and auto-tagging in GitHub Actions.
* Add Redis caching layer to test advanced deployments.
* Expand documentation in `/docs`, including a tunnel setup guide.
* Write public summary for README and `mtbonde.dev` portfolio.
* Start issue tracking and milestone planning for future services.

---

## References

* NutriFinder repo: [https://github.com/MTBonde/PB-SK-Xamen-NutriFinder](https://github.com/MTBonde/PB-SK-Xamen-NutriFinder)
* DockerHub (image hosting): [https://hub.docker.com/r/mtbonde/nutrifinder-server](https://hub.docker.com/r/mtbonde/nutrifinder-server)
* GitHub Actions: [https://docs.github.com/en/actions](https://docs.github.com/en/actions)
* Cloudflare Tunnel: [https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/)
* Watchtower (Docker auto-update): [https://containrrr.dev/watchtower/](https://containrrr.dev/watchtower/)
* MongoDB official image: [https://hub.docker.com/\_/mongo](https://hub.docker.com/_/mongo)

---

## Author

Mpho Thor Bonde (`mtbonde.dev`)
Portfolio-driven homelab, DevOps, and backend developer documenting the journey from scratch to self-hosted CI/CD infrastructure.
