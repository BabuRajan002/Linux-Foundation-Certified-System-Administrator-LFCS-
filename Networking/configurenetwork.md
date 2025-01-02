# Configure IPv4 and IPv6 

* `sudo ip addr add 10.0.0.1/24 dev enp0s8` - To assign a IPv4 address to an interface
* `sudo ip addr add <ipv6 address> dev enp0s8` - To assign a IPv6 address to an interface
* `sudo ip link set dev enp0s8 down` - To make the interface down
* `sudo ip link set dev enp0s8 up` - To make the interface up

## Ubuntu

netplan is tool widely used in Ubuntu system to configure the network settings.

* Configuration file for the netplan is `/etc/netplan/50-cloud-init.yaml`
* `sudo netplan get` - Get the Ipv4 settings for a device

## Start, stop and check the status networking service in Linux

To see the services which are waiting for the networking connections using `ss` and `netstat` commands. `ss` is newest tool and `netstat` is older one.
* `sudo ss -ltunp` - To see the services which are waiting for the network connections

- l = Listening
- t = tcp connections
- u = udp connections
- n = numeric values which is useful to display the port numbers
- p = Which process is involved for the each entry of our output
