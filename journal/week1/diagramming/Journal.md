## Media Server Architecture

```sh
                   +----------------------------------------+
                   |             Plex Server                |
                   |   Host IP: 192.168.1.120:32400         |
                   |   Tailscale IP: 100.100.0.1:32400      |
                   +------------------+---------------------+
                                      |
                                      v
                    +----------------------------------------+
                    |           Tailscale VPN Mesh           |
                    |   Access Point: 100.100.0.1            |
                    |   Shares Plex access across network    |
                    +------------------+---------------------+
                                      |
          +---------------------------+-------------------------------+
          |                                                           |
          v                                                           v
+-----------------------------+                       +--------------------------------+
|        NVIDIA Shield        |                       |          Prowlarr              |
| Tailscale IP: 100.100.0.2   |                       | Docker IP: 172.18.0.2:9696     |
| Connects to Plex:           |                       | Exposed via Gluetun            |
|        100.100.0.1:32400    |                       +---------------+----------------+
+-----------------------------+                                       |
                                                                     v
                    +--------------------------+------------------------------+
                    |        Media Management Tools (via Gluetun)             |
                    |---------------------------------------------------------|
                    |   Radarr                  |    Sonarr                   |
                    | Docker IP: 172.18.0.3:7878 | Docker IP: 172.18.0.4:8989 |
                    | via Gluetun VPN @ 172.18.0.1                            |
                    +---------------------------------------------------------+
```



## General Networking Overview 

- **(Plex Server)**:
It holds all your movies and shows.
It runs Plex, the app that organizes your media and makes it look pretty.
It has a local IP like 192.168.1.120 and a Tailscale IP like 100.100.0.1, so you can reach it securely from anywhere.

- **NVIDIA Shield:**
Your TV uses this device.
It connects to your Plex server via Tailscale VPN, which means it’s like you’re still on the same home network—even when you’re not.
It finds your Plex server at 100.100.0.1:32400.

- **Prowlarr + Radarr + Sonarr**: These are the automation brains.
Prowlarr finds what to download.
Radarr is for movies, Sonarr is for TV. They talk to public torrent trackers (the sources for media), but you run them through a VPN container called Gluetun so your IP is protected.

## Questions to Answer

### Scope of Business Use-case - Pro's and Con's of Different Design decisions
Enabling a secure, automated, and remotely accessible media streaming environment that integrates multiple components (Plex, Sonarr, Radarr, Prowlarr) with privacy protections and zero port-forwarding, using Tailscale and Docker-based containers.

- **Pros**: Easy to manage and scale, no need for port forwarding, protects your ip creates hardened security 
- **Cons**: Might have added latency compared to direct LAN access, adds extra routing layer which may impact local performance, requires client install, if not using MagicDNS Tailscale IP's can change. 

This architecture is home-grade in practice but it contains a lot of enterprise executing.

- Tailscale removes the need to expose ports, creates security and privacy. Straightward and works out of the box.
- Gluetun VPN creates isolation and gives fine control tuning for privacy and encryption, especially when dealing with private and NDA data.
- Docker containers can be portable, reproducable and as OS attacks are less common
- Plex Media is useful as a backup tool for photos, videos, music. This type of self hosting tool can be exchanged to tools like Nextcloud where it can be used in a different way.

## Recovery Plan

| Component                       | Backup Strategy                                 | Recovery Method                            |
| ------------------------------- | ----------------------------------------------- | ------------------------------------------ |
| **Plex Media**                  | External drive or NAS snapshot weekly           | Restore media files to same mount path     |
| **Docker Configs**              | Git versioned `docker-compose.yml`              | `git clone` + `docker-compose up -d`       |
| **App Configs**                 | Volume backups or bind mounts (e.g., `/config`) | Restore `/config` from backup ZIP          |
| **Tailscale**                   | Re-authenticate device or reassign keys         | Use Tailscale admin panel                  |
| **Gluetun VPN**                 | Backup `.env` and container config              | Rebuild container with same credentials    |
| **Automations (Sonarr/Radarr)** | Back up indexer settings and quality profiles   | Restore settings or import backup XML/JSON |

## Networking at Scale Detween Multiple Devices

| Scaling Aspect      | What Changes                                                |
| ------------------- | ----------------------------------------------------------- |
| **User Management** | Central auth or invite system (e.g. Tailscale ACL per user) |
| **Media Load**      | Cache-heavy SSDs, split libraries (per user or per genre)   |
| **Bandwidth**       | 1Gbps+ symmetrical fiber + QoS to prioritize streams        |
| **Storage**         | NAS or SAN with 50TB+ and redundant backups                 |
| **Monitoring**      | Prometheus/Grafana or Uptime Kuma for stream health         |
| **Access Control**  | ACL rules, SSO (OIDC), and rate limiting                    |
| **Legal**           | Licenses or user agreements, especially for torrents        |

- Usage of use split networks, maybe assigning a VLAN per household and run one central Plex over a Tailscale subnet router that allows secure sharing to verified clients.

## Handling Overlapping Networks

- Use NAT/Bridge Isolation:
Use Docker custom bridge (172.x.x.x) for internal containers.
Use iptables or Docker's --ip-range to force uniqueness.

- Use Subnet Routing + Tailscale Exit Nodes:
Assign a subnet router to advertise 192.168.10.0/24 to avoid local conflicts.
Clients route to Plex server via unique Tailscale IP (100.100.0.1), not 192.x.

- CIDR Allocation Strategy: Block-assign internal IPs: 192.168.1.0/24 for LAN, 172.18.0.0/16 for Docker, 10.10.0.0/16 for guests.

## Security Hardening Measures

| Layer                   | Hardening Strategy                                        |
| ----------------------- | --------------------------------------------------------- |
| **Tailscale ACLs**      | Limit which users/devices can access Plex                 |
| **Plex Authentication** | Require login for remote streaming, disable guest mode    |
| **Gluetun VPN**         | Enforced VPN lock (no IP leaks), strict firewall rules    |
| **Firewall**            | Block inter-container or container-to-host access         |
| **Docker**              | Use non-root containers, no `--privileged` mode           |
| **Secrets**             | Use `.env` files or secrets manager (not hardcoded creds) |
| **Monitoring**          | Add UptimeKuma, logs, or Prometheus-based alerts          |
| **Backups**             | Automate container volume backups                         |
