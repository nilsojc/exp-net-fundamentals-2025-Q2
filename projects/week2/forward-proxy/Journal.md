## Setting Up Squid and Creating new Windows Instance

Like with Reverse proxy lab, I set out to configure squid, and the two clients that are going to be involved in the process are an ubuntu and windows machine deployed as an ec2 instance in AWS, with same SG, VPC and subnet.  

On top of that, I created an extra Windows instance which will be the client connecting to our Windows proxy client. 


## Setting up Proxy

After creating the Windows Client instance and connecting it through RDP, I then connected that same client to the Proxy Instance from RDP in the client. 

I then went to `Network & Internet > Proxy` in the Windows settings to activate manual configuration on the proxy instance. 

I checked if the Ubuntu Proxy instance was attempting to communicate with the Windows Proxy machine by the following command:

```sh
sudo tail -f /var/log/squid/access.log
```

Had to also configure port `3128` on the current security group so that squid could communicate with the windows proxy instance. 

Added the following on the `squid.conf` to make the Ubuntu Proxy forward the connection to the windows proxy:

```sh
acl Safe_ports port 8080        # Forward Proxy
acl Safe_ports port 4223        # More ports
http_access allow localnet
http_port 172.31.88.225:3128
```

Had to do some troubleshooting but managed to make forward proxy to work perfectly fine, forwarding the connection of the ubuntu machine onto the windows proxy box.

