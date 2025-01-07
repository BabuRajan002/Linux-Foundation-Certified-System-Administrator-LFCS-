# Bridge

* Build a bridge between Network1 and Network2 
* Let's say a server has two NIC cards and Connecting a two network interface cards is called Bridge. 
* In Linux Networking terms connecting a two networks is also called Controller

![Bridge](Images/BridgeNetwork.png)

# Bond

* Connecting two network interfaces called Bonding
* Even though one of the network interfaces go down another would be there for backup
* Increase the network throughput

![bond](Images/Bond.png)

## Bonding Modes

* Mode0 - round robin
* Mode1 - Active backup
* Mode2 - XOR
* Mode3 - Broadcast
* Mode4 - IEEE 802.3ad
* Mode5 - Adaptive transmit load balancing
* Mode6 - Adaptive load balancing

# Firewall and packet filtering

In Ubuntu we can use a tool called UFW (Uncomplicated Firewall), through we can setup the packet filtering rules.

* `sudo ufw status` - to check the firewall status
* By default, if ufw is active it will BLOCK all the incoming traffics
* `sudo ufw allow 22` - This will allow the traffic on port 22 for both TCP and UDP protocols
* `sudo ufw enable` - to enable the ufw
* `sudo ufw allow from 10.0.0.192 to any port 22` - Allow the ssh connection only from this IP 10.0.0.192.
* `sudo ufw status numbered` - To check the firewall rules priority
* `sudo ufw delete 1 ` - Which will delete the rule number 1
* `sudo ufw insert 1 deny from 10.0.0.37` - Which will insert this rule as 1st rule.
* `sudo ufw deny out on snp0s3 to 8.8.8.8` - Which specifically deny the outgoing traffic to IP address 8.8.8.8



