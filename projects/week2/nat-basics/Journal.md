## Getting NAT overview of current lab topology

Upon learning the intricacies of NAT use case for this bootcamp, I took a look at the current configuration of the external connection and the router for NAT routes to know the different parameters set for NAT.

```sh
sh run | in nat
sh run int E0/0
sh run int E0/1
sh run | s access
sh run | s nat
```

Local router displayed `nat inside` while the external connector displayed `nat outside`.

I then ran a packet capture between the router and the switch. 

When pinging `8.8.8.8` the packet capture on CISCO shows the `ICMP` `LOOP` along with the `source` and `destination` IP's which is the main key when getting into nat connectivity.

## Final Observations 

Within this lab we observed how the packets get received and sent through the ICMP protocol from the external connector and also from the VM Network adapter with `Vshark` which really has it's own nuances but overall a very straight-forward way to learn about ip mapping from one source to the other. 

