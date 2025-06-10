## Setting Up Cisco Modeling Labs 

In order to get the labs up and running, I had to figure out how to install the Cisco modeling labs. I was running into some issues when trying to initialize the Virtual Machine on workstation due to HyperV issues as I use WSL, but got to get it working once I disabled it. The website can be finicky and setup can be slighly tedious, but was able to set it up.

## Added the Nodes in the Lab

For this first example, to get an overall idea of the inner workings of this virtual appliance, I connected different devices together to get it running:
- An external connector
- A Switch
- A Router
- A simulated ubuntu machine


## Lab Start and Commands 

I accesed the Switch command line through the CML seb service, to check the status of each device.

```sh
enable
show int status
```
