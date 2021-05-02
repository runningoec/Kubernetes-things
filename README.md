# Kubernetes-things


First, we need to configure the USG to speak BGP internally - its fairly simple, but does require use of the CLI, as BGP configuration is not yet in the Unifi Controller GUI:

Enter configuration mode with 

configure

Configure the router for BGP using its own IP address as the router id. We’ll also use the ‘private’ AS (autonomous system) number of 64512. Think of this like the RFC1918 address space (like 192.168.0.0/24). In my case, my internal network is 192.168.1.1/20:

set protocols bgp 64512 parameters router-id 192.168.1.1

Next, configure each worker on the network as a possible peer. Note that if you add or remove workers from your k8s cluster, you should update this. Again, we’ll be using the private AS:

set protocols bgp 64512 neighbor 192.168.1.219 remote-as 64512
set protocols bgp 64512 neighbor 192.168.1.220 remote-as 64512
set protocols bgp 64512 neighbor 192.168.1.221 remote-as 64512


Finally, commit and save the config:

commit
save

You can check to see if BGP as come up with 

show ip bgp
