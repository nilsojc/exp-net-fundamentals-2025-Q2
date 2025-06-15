# Networking Fundamentals Bootcamp 2025

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

### Week 1

[![Networking Bootcamp Week 1](https://img.youtube.com/vi/kS5CNbLybD4/0.jpg)](https://www.youtube.com/watch?v=kS5CNbLybD4)


### Week 2

```sh
<video here>
```

## Technical Diagram for Capstone Project

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

- [Technical Justification Journal](journal/week1/diagramming/Journal.md)

## Labs and Proof of Work
- [Week 1](projects/week1)
- [Week 2](projects/week2)


## Required Resources and Platforms
- Cloud: AWS, Azure, GCP.
- Development Tools: Github Codespaces, Vscode.
- Cisco Tools: Cisco Modeling Labs, Packet Tracer.
- Networking Software: Vshark
- Operating Systems: Windows 11 Server, Ubuntu, Red Hat.