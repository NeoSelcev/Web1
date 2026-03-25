---
applyTo: '**'
---
# web1 - AI Agent Context & Instructions

## 🎯 Project Overview
This repository contains the deployment and monitoring infrastructure for the **web1 Projects Platform**, intended to host various web applications and projects. It uses the same baseline infrastructure architecture as the HomeAssistant and n8n projects.

## 🏗️ Architecture & Stack
- **Core Service:** Web services deployed via Docker containers.
- **External Access:** Accessible via Cloudflare Tunnel at `web1.1ddjrkbeu4e472wsits1.dpdns.org`. Like sibling projects, it employs a Dual Authentication strategy: Cloudflare Access (OAuth) + an `nginx-proxy` enforcing HTTP Basic Auth.
- **Monitoring & Diagnostics:** Features an extensive suite of systemd-managed bash scripts for health monitoring (`web1-watchdog`), log rotation, system diagnostics, and intelligent alerting via a topic-based Telegram sender.

## ⚠️ Important Constraints & Rules
1. **Template Legacy:** The README and standard scripts were largely cloned from the HomeAssistant project. Ensure that any configuration files or Docker networking commands accurately target the `web1` containers rather than inadvertently referencing Home Assistant configurations (e.g. port 8123).
2. **System Tuning:** The host machine relies on specific optimizations (ZRAM swap, tmpfs for `/tmp`, tailored `sysctl.conf`). Keep any added web applications lightweight to respect these performance constraints.
3. **Structured Logging:** All monitoring services must utilize the centralized `logging-service.sh` logic. Events must be identifiable by severity levels (`log_info`, `log_error`) to be properly ingested by the failure notifier.

## 🤖 AI Agent Guidelines
1. **Cloudflare Security:** Do not expose new ports directly on the host or router. Route all new web apps through the `cloudflared` tunnel and update `cloudflared/config.yml` ingress rules accordingly.
2. **Secrets:** Look to `Home-Network-Infrastructure/Credentials/web1_credentials.txt` for passwords and API tokens. Never persist plaintext secrets.
