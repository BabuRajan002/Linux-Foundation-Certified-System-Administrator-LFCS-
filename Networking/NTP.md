# Network Time Protocol

Most of the modern devices today will automatically synchronize the timings using this protocol. 

Un Ubuntu we can use the `timedatectl` utility to set the timezones

* `timedatectl list-timezones`
* `timedatectl set-timezone America/Los_Angeles`

## Configure NTP in Ubuntu

* `sudo apt install systemd-timesyncd` - Install the timedatectl utility 
* `sudo timedatectl set-ntp true` - Set the NTP service to true will enable it
* `sudo vi /etc/systemd/timesyncd.conf` - Edit the conf file to give our own time servers if we have in our network
* `sudo show-timesync` - it will show current settings of NTP

