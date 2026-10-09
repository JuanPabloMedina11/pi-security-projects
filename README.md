# Raspberry Pi Security Lab

Cybersecurity projects built on a Raspberry Pi 5: a hardened home server that runs services I use every day, with security monitoring and detection built on top of them.

## Goals

- Build and harden a Linux server from scratch
- Self-host useful services and defend them like real assets
- Monitor everything, attack it myself, and write detections for what I find
- Document each step, including what went wrong

## Hardware

- Raspberry Pi 5 (8GB)
- 256GB SanDisk microSD
- Raspberry Pi OS (64-bit)

## Status

**Stage 1 complete.** Hardened base system with key-only SSH, a default-deny firewall, and automatic security updates. Lynis baseline hardening index: 63.

## Roadmap

- [x] **Stage 1:** Foundation and hardening
- [x] **Stage 2:** Docker and Tailscale
- [ ] **Stage 3:** Pi-hole and an automation bot
- [ ] **Stage 4:** Encrypted backups and a password manager
- [ ] **Stage 5:** WireGuard VPN built by hand
- [ ] **Stage 6:** Centralized logging, Suricata, and alerts
- [ ] **Stage 7:** Attack my own services and write detections
- [ ] **Stage 8:** Local AI log summaries and anomaly detection

## Build log

Step-by-step notes for every stage, including problems and fixes: [Build log](build-log.md)
