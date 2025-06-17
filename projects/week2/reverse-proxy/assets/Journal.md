## Installing Squid 

I went ahead and installed Squid (a popular caching and forwarding HTTP proxy) so that we can manage reverse proxy. 

```sh
sudo apt update && sudo apt upgrade -y
sudo apt install squid -y
sudo systemctl status squid
```

It is then important to configure squid:

```sh
sudo nano /etc/squid/squid.conf
```

After any changes are made, we will restart it:

```sh
sudo systemctl restart squid
```

## Setting Up Proxy 

I started defining the config from `squid.conf` for setting up reverse proxy

```sh
# no artificial limit on the number of concurrent spare attempts

# Basic reverse proxy configuration for Squid

# Define the HTTP port for incoming reverse proxy requests
http_port 8080 accel vhost

# Define the backend server (your windows IIS server)
cache_peer 172.31.95.210 parent 80 0 no—query originserver name=iis—server

# Define an ACL for requests to onr reverse proxy
acl our_sites dstdomain proxy—demo

# Use the backend server for these requests
cache_peer_access iis—server allow our_sites
cache_peer_access iis—server deny all

# Ensure regular forward proxy requests don't go to this backend
http_access deny our_sites
```

IP `172.31.95.210` will be the IP used for the windows server that will serve as a backend. 


I went to the windows server that will be configured and added a Web Server (IIS) feature in our Windows Server 2025 instance with AWS.

Within the client folder `C:\Windows\System32\drivers\etc` I edited `hosts` with the `public.ip - proxy-demo`  and an error was received from squid. 

After troubleshooting and learning ACL rules and other ports, doing a `sudo nano /etc/squid/squid.conf` and uncommenting `http_access deny all` is the key to allow the 



