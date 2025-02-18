## Question 2: 

Add a cron job for the user called john. Don't use the system-wide crontable, but rather add it to the personal crontable of the user called john.

Make sure that this cron job runs every Wednesday at 4AM. The command it should execute is find /home/john/ -type d -empty -delete.


Switch back to the bob user once the task is done.

***Answer***

```sh
sudo -i
su - john
crontab -l
crontab -e

# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
0  4 * * 3 find /home/john/ -type d -empty -delete

```
==================================================================================================================================================================================

## Question 3: 

There is a network interface on this system which has the IP address 10.5.5.2 associated with it. What is the name of this network interface? Create a file in /opt/interface.txt and add a single line to it containing the exact name of that interface.

***Answer***

```sh
root@caleston-lp10 ~ ➜  ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:92:7e:fc brd ff:ff:ff:ff:ff:ff
    altname enp0s9
    altname ens9
    inet 192.168.121.183/24 metric 100 brd 192.168.121.255 scope global dynamic eth0
       valid_lft 2807sec preferred_lft 2807sec
    inet6 fe80::5054:ff:fe92:7efc/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:38:1b:0e brd ff:ff:ff:ff:ff:ff
    altname enp0s10
    altname ens10
    inet 10.5.5.2/24 scope global eth1
```
==================================================================================================================================================================================

## Question 4: 

An administrator added a new user called jane to this system. But a few mistakes were made. Fix the following problems:

The administrator wanted to allow jane to run sudo commands. But instead of adding "jane" to the secondary/supplemental "sudo" group, the administrator changed the primary group to sudo. Fix this by doing the following: Set the primary/login group back to the group called jane. And add the user to the secondary/supplemental group called sudo. In essence the primary group for the user called "jane" should be "jane". And the secondary group should be "sudo".

Currently, the home directory path for the jane user is set correctly. But the directory itself is missing. Fix this by creating the /home/jane/ directory. Make sure that the directory is owned by the jane user and jane group.

The default shell for the user called jane is set to /bin/sh. Change the default shell to /bin/bash.

Finally, set the password for jane to 1234.

```sh
usermod -aG sudo jane

usermod -g jane jane

cat /etc/passwd | grep jane

mkdir -p /home/jane

chown -R jane:jane /home/jane

usermod -s /bin/bash jane

```