# My First Homelab

My first homelab — a hands-on journey into Linux, Docker, DevOps, and self-hosting.
As a software development student learning DevOps in school, this project gives me a place to apply concepts, build confidence, and learn by doing.

---

## Goals & Roadmap

I'm building this homelab from scratch to understand real-world infrastructure, deployment, and service management.

### Learning Objectives

* Understand basic Linux server administration
* Practice with DevOps tools (Docker, CI/CD, SSH)
* Learn to self-host APIs and services with resilience
* Build confidence through practical, hands-on experimentation

### Long-Term Plans

* PostgreSQL and/or MongoDB
* NAS and backup system
* Jellyfin media center
* Monitoring tools (Uptime Kuma, Prometheus, Grafana)
* Kubernetes / K3s
* Ansible or Terraform
* Service dashboard
* Failover and restore strategies

---

## Current Setup Summary

* Ubuntu 24.04.2 LTS installed with secure SSH access enabled
* Docker installed and operational
* Watchtower running outside of Docker Compose for automatic container updates
* cloudflared running as a systemd service for persistent tunnel uptime
* NutriFinder API deployed and publicly accessible

---

## Milestones

### ✅ Baseline Setup *(Completed 2025-05-24)*

* Ubuntu Server installed on old laptop
* SSH access configured
* Docker installed and verified

### ✅ Public API Deployment *(Completed 2025-06-05)*

* NutriFinder API deployed via Cloudflare Tunnel
* Live at [api.mtbonde.dev](https://api.mtbonde.dev)
* HTTPS provided by Cloudflare Tunnel with minimal configuration
* CI/CD pipeline built with GitHub Actions → DockerHub → Watchtower auto-deployment
* Used for exam presentation; will serve real users in future

---

## Security Plan (WIP)

* SSH hardened (key-only login, root disabled)
* Firewall setup under review
* fail2ban under consideration
* Threat model and hardening rationale to be documented

---

## Remote Access & Networking (Planned)

* Tailscale or WireGuard setup
* Additional tunnels or reverse proxies if needed

---

## Deployment Documentation

These documents detail how the NutriFinder API was deployed from scratch in a homelab environment.

* [Cloudflare Tunnel Setup Guide](docs/cloudflare-tunnel.md)
* [DevOps Journey: From Exam Project to Production](docs/devops-journey.md)

---

## Lessons Learned

See [docs/lessons-learned.md](docs/lessons-learned.md) for a growing log of insights, mistakes, fixes, and reflections.

---

## System Snapshot

- **OS**: Ubuntu 24.04.2 LTS on repurposed old laptop
- **Networking**: SSH + Static IP + Cloudflare Tunnel
- **Containers**: Docker + Compose (MongoDB, NutriFinder API)
- **Deployment**: GitHub Actions → DockerHub → Watchtower
- **Automation**: Watchtower + systemd Cloudflare Tunnel

---

## Inspiration

* [YouTube video](https://www.youtube.com/watch?v=8s0DWeHuEaw) that started it all
* Mischa Vandenburg’s [homelab repo](https://github.com/mischavandenburg/homelab)

---
