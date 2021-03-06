# IP-Address renewal

1. Review the installed interfaces to find the related device designation:
	```
	$ ip addr
	```
2. Release IP from selected interface (e.g. eth0, eth1, enp1s0, sit0, wlan0,...):
	```
	# dhclient -r <interface>
	```
3. Request new IP from DHCP server (alert on error):
	```
	# dhclient -1 <interface>
	```
4. Check IP:
	```
	$ ip addr
	```
  
> NOTE: Debian 9.3 (Stretch) and Ubuntu 16.04 LTS confirmed!

# References

> Adapted from: Serverfault.com
> [How do I force Linux to reacquire a new IP address from the DHCP server?][1]


<!-- REFERENCES -->
[1]:https://serverfault.com/questions/42799/how-do-i-force-linux-to-reacquire-a-new-ip-address-from-the-dhcp-server/784619#784619
