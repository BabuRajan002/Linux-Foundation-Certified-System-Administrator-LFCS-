# Kernal Runtime parameters

* Basically this is a fancy name for all kernal low level settings. Using this parameters we can adjust the memory allocation, hardware communication settings etc.

* `sudo sysctl -a` - To dijsplay all the kernal parameters
* `sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1` - To change the value of particular parameter. This is a non-persistent change. Means this settings will be there only Untill the system reboots. 
* In order to make it persistent we need to add a file under `/etc/sysctl.d/filename.conf`
* `sudo sysctl -p /etc/sysctl.d/swap-less.conf` - This command applies the changes immediately without the system reboot. 
* `sudo vim /etc/sysctl.conf` - This is the other place we can make the kernal parameters changes

