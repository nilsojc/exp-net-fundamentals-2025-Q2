# exp-net-fundamentals-2025-Q2

This repository documents the Networking Fundamentals bootcamp organized by Andrew Brown and Tim McConnaughy, where we focus on firewall rules, response between different networks, comprehensive networking of linux and windows operating systems, tracing of packets, proxies and connections in real-world scenarios.

## Repository Structure

```
├── README.md                  # Repo overview of Networking Fundamentals Bootcamp
├── projects/                  # All implementation labs and projects of the bootcamp
│    ├── week1                 # Linux, Windows Firewall Rules and Networking
│        ├──env_automation
         ├──ip-address-management
         ├──linux-firewall-rules
         ├──linux-networking
         ├──packet-tracer
         ├──windows-firewall-rules
         ├──windows-networking 
│    ├── week2                 # Cisco Modeling Lab Demos, VPN, Proxies and Vshark
│        ├──cml-setup
│        ├──forward-proxy
│        ├──ha-proxy
│        ├──nat-basics
│        ├──reverse-proxy
│        ├──vpn
│        ├──vshark-traffic
```

## Technical Video Demostration

# Week 1 

```sh
<Video Demo>
```
# Week 2

## Technical Diagram

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

