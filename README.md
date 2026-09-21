# Steve's Homelab
# My Homelab

## Overview

A two-server Linux homelab built for self-hosted applications, network
services, secure remote access, monitoring, and automated backups.

## Infrastructure

### Primary Server

Runs Docker applications for private cloud storage, photo management,
media streaming, password management, authentication, and monitoring.

### Network Server

Handles DNS filtering, DNS resolution, VPN access, reverse proxy traffic,
and network monitoring.

## Networking and Security

- Pi-hole and Unbound
- WireGuard VPN
- Nginx Proxy Manager
- TLS certificates and DNS management
- Authentik centralized authentication
- Fail2ban
- Firewall rules and controlled port forwarding

## Storage and Recovery

- Multiple Linux RAID 1 arrays
- SMART monitoring
- Automated database exports
- Local Restic backups
- Encrypted Backblaze B2 backups
- Full-system recovery images
- Documented recovery procedures

## Technologies

Linux, Docker Compose, RAID, Nginx Proxy Manager, DNS, TLS, WireGuard,
Authentik, Pi-hole, Unbound, Restic, rclone, Backblaze B2, MariaDB,
PostgreSQL, Bash
