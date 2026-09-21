<div align="center">

# Steve's Homelab

### A two-server Linux environment built for self-hosting, networking, security, storage, and disaster recovery

[![Linux](https://img.shields.io/badge/Linux-Server_Administration-FCC624?style=for-the-badge&logo=linux&logoColor=black)](https://www.kernel.org/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://github.com/docker/compose)
[![WireGuard](https://img.shields.io/badge/WireGuard-VPN-88171A?style=for-the-badge&logo=wireguard&logoColor=white)](https://github.com/WireGuard/wireguard-tools)
[![Backblaze](https://img.shields.io/badge/Backblaze-B2-E21E29?style=for-the-badge&logo=backblaze&logoColor=white)](https://www.backblaze.com/cloud-storage)

</div>

## About This Project

I designed and maintain this homelab to gain practical experience with Linux administration, networking, Docker, storage, security, monitoring, and backups. It began as a small self-hosting project and developed into a two-server environment with separate application and network responsibilities.

- **Caroline** is the main application, database, storage, and backup server.
- **GLaDOS** handles DNS, VPN access, reverse proxy traffic, and network monitoring.

The server names are themed after Valve's *Portal* games, with Caroline referencing Caroline from Aperture Science and GLaDOS named after the game's central AI.

Most services run in Docker containers managed with Docker Compose. Persistent data is stored separately from the containers so applications can be upgraded, recreated, or restored without losing their configuration or databases.

My priorities are reliability, limited public exposure, secure remote access, redundant storage, automated backups, and straightforward recovery.

Selected applications are published through Nginx Proxy Manager using HTTPS. Most management interfaces remain private and are accessed locally or through WireGuard.

## Hardware

| System | Hardware | Main responsibility |
| --- | --- | --- |
| **Caroline** | Lenovo M73 Tiny, Intel Core i5, 12 GB RAM, 250 GB SSD | Applications, databases, storage, and backups |
| **GLaDOS** | Lenovo M72 Tiny, Intel Core i3, 8 GB RAM | DNS, VPN, reverse proxy, and monitoring |
| **Storage** | TerraMaster multi-bay DAS | Houses both Linux RAID arrays |
| **Primary array** | 6 TB WD Red Pro drives in RAID 1 | Main data, photos, and media |
| **Secondary array** | 2 TB drives in RAID 1 | Application data and local backups |
| **Recovery storage** | Separate 2 TB drive | Full-system recovery images |
| **Network** | ASUS RT-AX58U running Asuswrt-Merlin | Routing, firewall, DHCP, DNS enforcement, and port forwarding |
| **Power protection** | APC Back-UPS with USB monitoring | Power-loss protection and UPS status |

I also use dedicated KVM devices for console access instead of depending on normal remote shell access.

## Network and Security

GLaDOS separates network services from the main application server. It handles DNS filtering, encrypted remote access, reverse proxy traffic, and service monitoring.

### DNS

[![Pi-hole](https://img.shields.io/badge/Pi--hole-DNS_Filtering-96060C?style=flat-square&logo=pihole&logoColor=white)](https://github.com/pi-hole/pi-hole)
[![Unbound](https://img.shields.io/badge/Unbound-DNS_Resolver-005571?style=flat-square)](https://github.com/NLnetLabs/unbound)

Pi-hole provides network-wide DNS filtering, while Unbound provides upstream DNS resolution. The router directs clients to the internal DNS service, and I maintain different filtering policies for specific device groups.

I have troubleshot client caching, blocked services, incorrect upstream configuration, local DNS records, and devices attempting to bypass the configured DNS server.

### Reverse Proxy, TLS, and Remote Access

[![Nginx Proxy Manager](https://img.shields.io/badge/Nginx_Proxy_Manager-Reverse_Proxy-F15833?style=flat-square&logo=nginxproxymanager&logoColor=white)](https://github.com/NginxProxyManager/nginx-proxy-manager)
[![wg-easy](https://img.shields.io/badge/wg--easy-WireGuard_VPN-88171A?style=flat-square&logo=wireguard&logoColor=white)](https://github.com/wg-easy/wg-easy)
[![Authentik](https://img.shields.io/badge/Authentik-Identity_Provider-FD4B2D?style=flat-square&logo=authentik&logoColor=white)](https://github.com/goauthentik/authentik)
[![Fail2ban](https://img.shields.io/badge/Fail2ban-Intrusion_Prevention-222222?style=flat-square)](https://github.com/fail2ban/fail2ban)

Nginx Proxy Manager routes HTTPS traffic to selected internal applications. I manage a custom domain, DNS records, proxy hosts, and TLS certificates. This lets several applications use standard HTTPS addresses without opening a separate public port for each service.

WireGuard provides encrypted remote access to services that should remain private. I manage VPN peers, allowed routes, client configuration, internal DNS access, and router forwarding.

Additional protections include centralized authentication through Authentik, multi-factor authentication for sensitive accounts, Fail2ban monitoring, limited port forwarding, and private access for most administrative interfaces.

## Docker and Services

[![Docker Compose](https://img.shields.io/badge/Docker_Compose-Container_Platform-2496ED?style=flat-square&logo=docker&logoColor=white)](https://github.com/docker/compose)

My Docker administration work includes:

- Creating and updating Compose files
- Configuring container networks, ports, volumes, and environment variables
- Managing persistent application data
- Connecting services to MariaDB and PostgreSQL databases
- Reviewing health checks and application logs
- Updating or recreating individual containers
- Diagnosing permissions, paths, dependencies, and configuration errors
- Verifying services after upgrades, restarts, and storage maintenance

### Private Cloud and Collaboration

[![Nextcloud](https://img.shields.io/badge/Nextcloud-Private_Cloud-0082C9?style=flat-square&logo=nextcloud&logoColor=white)](https://github.com/nextcloud/server)
[![Immich](https://img.shields.io/badge/Immich-Photo_Management-4250AF?style=flat-square&logo=immich&logoColor=white)](https://github.com/immich-app/immich)
[![Collabora](https://img.shields.io/badge/Collabora-Online_Office-5C2983?style=flat-square&logo=libreoffice&logoColor=white)](https://github.com/CollaboraOnline/online)

- **Nextcloud** provides private file synchronization, contacts, and calendars.
- **Immich** manages my private photo and video library using PostgreSQL and a separate machine-learning container.
- **Collabora Online** integrates with Nextcloud for browser-based document editing.

### Identity and Password Management

[![Authentik](https://img.shields.io/badge/Authentik-Single_Sign--On-FD4B2D?style=flat-square&logo=authentik&logoColor=white)](https://github.com/goauthentik/authentik)
[![Vaultwarden](https://img.shields.io/badge/Vaultwarden-Password_Manager-175DDC?style=flat-square&logo=bitwarden&logoColor=white)](https://github.com/dani-garcia/vaultwarden)

- **Authentik** provides centralized authentication and single sign-on for supported applications.
- **Vaultwarden** provides self-hosted password management with its own protected backup process.

### Media and Libraries

[![Jellyfin](https://img.shields.io/badge/Jellyfin-Media_Server-00A4DC?style=flat-square&logo=jellyfin&logoColor=white)](https://github.com/jellyfin/jellyfin)
[![Jellyseerr](https://img.shields.io/badge/Jellyseerr-Request_Management-7B5BF2?style=flat-square)](https://github.com/Fallenbagel/jellyseerr)
[![Sonarr](https://img.shields.io/badge/Sonarr-TV_Management-35C5F4?style=flat-square)](https://github.com/Sonarr/Sonarr)
[![Radarr](https://img.shields.io/badge/Radarr-Movie_Management-FCBA03?style=flat-square)](https://github.com/Radarr/Radarr)
[![Lidarr](https://img.shields.io/badge/Lidarr-Music_Management-00A65A?style=flat-square)](https://github.com/Lidarr/Lidarr)
[![Prowlarr](https://img.shields.io/badge/Prowlarr-Indexer_Management-DA5B0B?style=flat-square)](https://github.com/Prowlarr/Prowlarr)
[![Navidrome](https://img.shields.io/badge/Navidrome-Music_Server-152B3C?style=flat-square&logo=navidrome&logoColor=white)](https://github.com/navidrome/navidrome)
[![Kavita](https://img.shields.io/badge/Kavita-Ebook_Library-4F46E5?style=flat-square)](https://github.com/Kareadita/Kavita)
[![Audiobookshelf](https://img.shields.io/badge/Audiobookshelf-Audiobook_Server-82612C?style=flat-square&logo=audiobookshelf&logoColor=white)](https://github.com/advplyr/audiobookshelf)

These services have given me experience with application APIs, container networking, storage mappings, user permissions, hardlinks, metadata, media transcoding, and authentication integrations.

### Monitoring and Administration

[![Uptime Kuma](https://img.shields.io/badge/Uptime_Kuma-Service_Monitoring-5CDD8B?style=flat-square&logo=uptimekuma&logoColor=white)](https://github.com/louislam/uptime-kuma)
[![Homarr](https://img.shields.io/badge/Homarr-Dashboard-FA5252?style=flat-square&logo=homarr&logoColor=white)](https://github.com/homarr-labs/homarr)
[![Dozzle](https://img.shields.io/badge/Dozzle-Container_Logs-282A36?style=flat-square&logo=docker&logoColor=white)](https://github.com/amir20/dozzle)
[![NetAlertX](https://img.shields.io/badge/NetAlertX-Network_Monitoring-17A2B8?style=flat-square)](https://github.com/jokob-sk/NetAlertX)

I use these tools to monitor service availability, view container logs, organize service access, and detect devices joining the network.

## Storage and Backups

Caroline uses two Linux software RAID 1 arrays. RAID protects against a single drive failure in each array, but I treat it as availabilityâ€”not as a backup.

My storage work includes monitoring array state, running consistency checks, reviewing SMART information and kernel logs, investigating disk or enclosure issues, and maintaining stable mount points for Docker volumes.

I recently migrated the arrays to a TerraMaster DAS, confirmed that Linux assembled both arrays correctly, and verified application health afterward.

### Backup Strategy

[![Restic](https://img.shields.io/badge/Restic-Local_Backups-1A4D80?style=flat-square)](https://github.com/restic/restic)
[![rclone](https://img.shields.io/badge/rclone-Cloud_Transfer-3F79AD?style=flat-square&logo=rclone&logoColor=white)](https://github.com/rclone/rclone)
[![ReaR](https://img.shields.io/badge/Relax--and--Recover-System_Recovery-8B0000?style=flat-square)](https://github.com/rear/rear)

The backup system includes:

- Scheduled MariaDB and PostgreSQL exports
- Local Restic snapshots
- Docker configuration and application-data archives
- Encrypted off-site backups in Backblaze B2
- Automated retention policies
- Separate protection for password and authentication data
- Bootable full-system recovery images created with Relax-and-Recover
- Written recovery procedures

Large, replaceable media files are generally excluded from off-site storage. I prioritize databases, configurations, photos, account data, request history, and everything required to rebuild the environment.

## Troubleshooting Experience

Maintaining this environment has required troubleshooting across the full path between a user and an application. Examples include:

- Tracing outages through DNS, routing, TLS, reverse proxy, Docker, database, and application layers
- Diagnosing unhealthy containers through logs and health checks
- Resolving incorrect paths, permissions, environment variables, and port mappings
- Investigating HTTP 500, 502, and 503 errors
- Recovering services after storage migrations and host restarts
- Diagnosing NAT loopback and router configuration problems
- Reviewing SMART data and kernel logs for disk and enclosure issues
- Troubleshooting certificate issuance, renewal, and hostname problems
- Diagnosing authentication flows between Authentik, proxies, and applications

The main lesson has been to test each layer instead of guessing. A service that appears offline may actually be affected by DNS, routing, a certificate, the reverse proxy, a missing storage mount, a database, or the application itself.

## Skills Demonstrated

- Linux server administration
- Docker and Docker Compose
- TCP/IP, DNS, DHCP, NAT, and port forwarding
- Nginx reverse proxying and TLS certificates
- WireGuard VPN administration
- Linux software RAID and SMART monitoring
- MariaDB, PostgreSQL, and database exports
- Restic, rclone, Backblaze B2, and backup retention
- Bash, cron, and systemd timers
- Identity management and single sign-on
- Log analysis and structured troubleshooting
- Technical and disaster-recovery documentation

## Future Improvements

- Move storage into a purpose-built NAS chassis
- Improve network separation for IoT and media devices
- Perform additional full recovery tests
- Add more centralized logging and security monitoring
- Continue improving architecture and recovery documentation

---

This repository is a sanitized overview. Passwords, API keys, internal addresses, backup secrets, complete configurations, and other sensitive details are intentionally excluded.
