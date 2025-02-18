## Question 1: 

In the /opt/findme/ directory you will find 10,000 files. You need to filter out specific files

Find all files that have the executable permission bit enabled for the user that owns them. Redirect the output of the find command to the /opt/foundthem.txt file.
Find all files that have the SETUID permission enabled and delete them.
Find any file that is larger than 1KB and copy it to the /opt/ directory

* Found files with executable permission?

***Answer*** `sudo find /opt/findme -type f -perm -u=x  > /opt/foundthem.txt`

* Deleted files with SETUID permission?

***Answer*** 
i. `find /opt/findme/ -type f -perm /4000`

ii. Remove the files 

* Is the file larger than 1KB moved to /opt?

i. `sudo find /opt/findme/ -type f -size +1k` 

ii. Copy the file

=================================================================================================================
## Question 2:
Perform the following two tasks:

Create a bash script that recursively copies the /var/www/ directory into the /opt/www-backup/ directory

Save your script at /opt/script.sh. Remember, the script file you create also has to be executable.

Make sure that your script /opt/script.sh automatically runs every day at 4AM. More specifically, create a cron job that runs that script every day at 4AM. Put this in the system-wide cron table (not root's local cron table) and make sure the script executes under the root user.


* Is the /opt/script.sh script executable?

* Is the script configured right?

* Is system-wide cron table updated?

***Answer***

```sh
#!/bin/bash

if [[ ! -d "/opt/www-backup" ]]; then
  sudo mkdir -p /opt/www-backup/
  echo "Directory /opt/www-backup created"
else
  echo "Directory /opt/www-backup already exists"
fi

cp -a /var/www/. /opt/www-backup/
```

* Is system-wide cron table updated?

```sh
cat /etc/crontab

# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
# You can also override PATH, but by default, newer versions inherit it from the environment
#PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
0 4 * * * root /opt/script.sh

```
==============================================================================================================================================================================

## Question 3: 

Enforce some limits on two users:

Set a limit on the user called john so that he can open no more than 30 processes. This should be a hard limit.

For the user called jane make sure she can create files not larger than 1024 kilobytes. Make this a soft limit.

***Answer***

```sh
root@caleston-lp10 ~ ➜  tail -10 /etc/security/limits.conf 
#@student        hard    nproc           20
#@faculty        soft    nproc           20
#@faculty        hard    nproc           50
#ftp             hard    nproc           0
#ftp             -       chroot          /ftp
#@student        -       maxlogins       4

# End of file
john   hard  nproc   30
jane   soft  fsize   1024

```

===============================================================================================================================================================================

## Question 4: 

Create a new user on this system called mary

* Set her password to 1234.
* Leave the full name and other personal details empty.
* Set her default shell to /bin/dash.
* Make sure she can execute sudo commands by adding her to the secondary group called sudo.
* At this point Mary's primary group is mary. And her secondary group is sudo. Change her primary group to developers. Without affecting her secondary group.

***Answer***

# Create the user mary with /bin/dash as the default shell
`sudo useradd -m -s /bin/dash mary`

# Add mary to the sudo group
`sudo usermod -aG sudo mary`

# Change mary's primary group to developers
`sudo usermod -g developers mary`

# Verify mary's groups
`groups mary`

# Verify mary's default shell
`getent passwd mary`

==================================================================================================================================================================================

## Question 5: 

Modify the following kernel runtime parameter:

vm.swappiness set it to a value of 10. This should be a persistent change, added to a file so that vm.swappiness is set to 10 every time the system boots up. However, after you create the proper file, also set this runtime parameter to 10 for this session as well. Otherwise said, the file will set the parameter to 10 the next time the system boots up, but we want to set it to 10 even for this current, active session, instead of waiting until the next boot until that takes effect.


* `sudo sysctl -a` - To dijsplay all the kernal parameters

* `sudo sysctl -w vm.swappiness=10` - Non persistent value

* `sudo sysctl -p /etc/sysctl.d/99-swappiness.conf` - Set the persistent value

==================================================================================================================================================================================

## Question 6: 

You have an xfs filesystem on /dev/vdb1. Also, there's an ext4 filesystem on /dev/vdb2.

Edit the correct file in /etc/ so that /dev/vdb1 is automatically mounted into the /backups directory every time the system boots up. Default mount options should be used.

/dev/vdb2 is already mounted in /mnt/. But there is a problem. Sensitive data exists on this ext4 filesystem and you want to make sure that it's not accidentally modified. To solve this problem, remount /dev/vdb2 into the /mnt directory, but this time, with the read-only mount option. It does not matter what the other mount options are. Just make sure this mount point is read-only so that users cannot change contents on this filesystem.

```sh
bob@caleston-lp10:~$ cat /etc/fstab 
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda1 during curtin installation
/dev/disk/by-uuid/8ee8caa8-cef0-4e5c-a626-03f2a0f13c00 / ext4 defaults 0 1
/swap.img       none    swap    sw      0       0
#VAGRANT-BEGIN
# The contents below are automatically generated by Vagrant. Do not modify.
#VAGRANT-END
/dev/vdb1   /backups xfs defaults 0     0 

```

`sudo mount -o remount,ro /dev/vdb1 /mnt`

==================================================================================================================================================================================

## Question 7: 

Use the Logical Volume Manager to perform the following tasks:

Add /dev/vdc and /dev/vdd as Physical Volumes to LVM.
Create a Volume Group on these two physical volumes. Call the volume volume1.
On the Volume Group called volume1 create a new Logical Volume. Call this Logical Volume website_files. Set the size of the Logical Volume to 3GB.

Is /dev/vdc and /dev/vdd initialized as PV?

Is Volume Group created?

Is website_files logical volume created?

```sh
    pvcreate /dev/vdc /dev/vdd
    vgcreate volume1 /dev/vdc /dev/vdd
    vgs
    lvcreate --size 3G --name website_files volume1
```

==================================================================================================================================================================================

## Question 8: 

In your home directory you will find a subdirectory called kode. Git tools are pre-installed. Switch to the kode subdirectory and perform the following tasks:

Initialize this subdirectory as an empty Git repository.
Associate this local Git repository with the remote repository found at https://github.com/kodekloudhub/git-for-beginners-course.git. Add this as a remote repository and call it (alias it as) origin.
Download all the latest changes from the master branch from that remote repository into your local repository.

***Answer***

```sh
git init
git remote -v
git remote add origin https://github.com/kodekloudhub/git-for-beginners-course.git
git remote -v
git fetch origin master
git branch
git pull origin master
```

==================================================================================================================================================================================

## Question 9:

A Docker container is running on node01. Perform the following tasks:

Stop and remove the container that is currently running, since it's not configured correctly.
In your home directory you will find a subdirectory called kode_web. It contains all the necessary build instructions for Docker. Use that directory to build a new Docker image. Call this image kodekloudwebserv.
Finally, launch a container based on the kodekloudwebserv image. In your command, make sure that all connections incoming to port 8081 on the host are redirected to port 80 of the container. Call this container webserver2.

Credentials to access node01:
Name: bob
Password: caleston123

***Answer***

* `docker run -d -p 8081:80 --name=webserver2  kodekloudwebserv`

==================================================================================================================================================================================

## Question 10:

NFS server and client tools are installed on caleston-lp10 system. Instruct the NFS server to share the /home directory in read-only mode with IP addresses in the 10.0.0.0/24 CIDR range.

***Answer***


```sh
bob@caleston-lp10:~$ cat /etc/exports 
# /etc/exports: the access control list for filesystems which may be exported
#               to NFS clients.  See exports(5).
#
# Example for NFSv2 and NFSv3:
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)
#
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#
/home   10.0.0.0/24(ro)

sudo exportfs -ra

```

==================================================================================================================================================================================

## Question 11: 

Find the application that is accepting incoming connections on port 80. Make note of the exact name of that application. You will need it later on.

As you investigated what application is accepting incoming connections to port 80, you might have noticed that two or more PIDs are associated with that. Basically, the application has forked multiple processes to do its job. Figure out which PID is associated with the master process.

With both of these things noted, create the following file: /opt/process.txt.

On the first line add the name of the application associated with port 80, in all lowercase letters.
On the second line add the PID number associated with the master process.


```sh
bob@caleston-lp10:~$ sudo ss -tulnp | grep :80
tcp   LISTEN 0      511                0.0.0.0:80         0.0.0.0:*    users:(("nginx",pid=7995,fd=6),("nginx",pid=7992,fd=6))  
tcp   LISTEN 0      128                0.0.0.0:8080       0.0.0.0:*    users:(("ttyd",pid=8191,fd=12))                          
tcp   LISTEN 0      511                   [::]:80            [::]:*    users:(("nginx",pid=7995,fd=7),("nginx",pid=7992,fd=7))  

bob@caleston-lp10:~$ ps aux | grep nginx
root        7992  0.0  1.2  55236 11892 ?        S    02:48   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
www-data    7995  0.0  0.5  55868  5496 ?        S    02:48   0:00 nginx: worker process
bob         9876  0.0  0.2   6480  2316 pts/6    S+   06:01   0:00 grep --color=auto nginx
```
==================================================================================================================================================================================

## Question 12: 

Explore your network settings and perform the following tasks:

There is currently one network interface which does not have any IPv4 address associated with it. Temporarily assign it the following IPv4 address: 10.5.0.1/24. This should not be a permanent change (no need to edit configuration files).

What is the default route for this system? Create a file, and add a single line where you save the IP address for the gateway used by this default route (i.e., requests are routed to what IP address?). Save the address in this file: /opt/gateway.txt.

For the final task, find out what is the IP address of the main DNS resolver configured for this system. Create the file /opt/dns.txt and add a single line to it with that IP address.

***Answer***

`sudo ip addr add 10.5.0.1/24 dev eth1`

```sh
bob@caleston-lp10:~$ ip route
default via 192.168.121.1 dev eth0 proto dhcp src 192.168.121.7 metric 100 
10.5.0.0/24 dev eth1 proto kernel scope link src 10.5.0.1 
192.168.121.0/24 dev eth0 proto kernel scope link src 192.168.121.7 metric 100 
192.168.121.1 dev eth0 proto dhcp scope link src 192.168.121.7 metric 100 
192.168.122.0/24 dev virbr0 proto kernel scope link src 192.168.122.1 linkdown
```

```sh
bob@caleston-lp10:~$ tail -4 /etc/resolv.conf 
nameserver 127.0.0.53
options edns0 trust-ad
search .

```

==================================================================================================================================================================================

## Question 13

There is a another virtual machine with name node01 is available for you. Perform this task on node01. You can access the node by ssh

username: bob
Host- node01
Password- caleston123 

You can identify the disk because it is currently unpartitioned, not mounted anywhere, and is 2 GB in size.
Disk /dev/vdb is unpartitioned, not mounted anywhere, and is 2 GB in size.

Create two partitions of equal size on this disk: 500M each. Ignore about remaining 1GB partition.
Create an ext4 filesystem on the first partition.
Create an xfs filesystem on the second partition.
Create two directories: /part1 and /part2.
Manually mount the ext4 filesystem in the /part1 directory. Manually mount the xfs filesystem in the /part2 directory.
Also, configure the system to automatically mount these the same way, every time the operating system boots up.

***Answer***

Access the node01 by ssh

username: bob
Host- node01
Password- caleston123 

Run lsblk command to list the block devices and find the block disk without any partitions.

To create 2 partition of 500M each, run the below command:

`echo -e "g\nn\n\n\n+500M\nn\n\n\n+500M\nn\n\n\n\nw" | sudo fdisk /dev/vdb`

Create an ext4 filesystem on the first partition and an xfs filesystem on the second partition.

```sh
sudo mkfs.ext4 /dev/vdb1
sudo mkfs.xfs /dev/vdb2
```

Create two directories for mounting the partitions.

```sh
sudo mkdir /part1
sudo mkdir /part2
```

Mount the ext4 filesystem on /part1 and the xfs filesystem on /part2

```sh
sudo mount /dev/vdb1 /part1
sudo mount /dev/vdb2 /part2
```

To ensure these partitions mount automatically at boot, you'll need to add them to the /etc/fstab file.

```sh
/dev/vdb1    /part1    ext4    defaults    0    2
/dev/vdb2    /part2    xfs    defaults    0    2
```

==================================================================================================================================================================================
## Question 14

Configure the system to use /swfile as a swap file on node01.

First, create /swfile. Make the size of this file exactly 1024 MB.
Then take all the necessary steps to temporarily mount this as swap. (So that it's immediately used as swap for this current boot session).
But also make sure to configure the system to also use this as swap every time it will boot up in the future.

Credentials to access node01:
Name: bob
Password: caleston123

***Answer***

1) Login to node01 server
2) Execute the following command to create a swap file of exactly 1024 MB

`sudo fallocate -l 1024M /swfile`

3) Change the permission of the /swfile as below:

`sudo chmod 600 /swfile`

4) Use mkswap to set up the file as Linux swap area:

`sudo mkswap /swfile`

5) Activate the Swap File

`sudo swapon /swfile`

6) To ensure that the swap file is used on every boot, you need to add it to the /etc/fstab file.

`/swfile none swap sw 0 0`

==================================================================================================================================================================================
## Question 15 

On node01two processes are overusing our storage device. One is executing a lot of I/O operations per second (small data transfers, but a very large number of such transfers). Otherwise said, the process has a high tps/IOPS. The other process is reading very large volumes of data.

Identify the process with the high tps. What partition is it using? Create the file /opt/devname.txtand write the device name of that partition inside that file. For example, if it's using /dev/vde5, you would simply write /dev/vde5 on a single line in that file. Note that there might be some abstractions behind this, and we're not interested in device mapper names, but rather, the real device the mapper is using.

Identify the process with the high read transfer rate/second. Create the file /opt/highread.pid and write the PID number of that process in that file. For example, if the PID is 3886 you just write 3886 in that file (only the number, on a single line).

Credentials to access node01:
Name: bob
Password: caleston123

***Answer***

Solution
To identify the process with high TPS and the partition it is using, follow the steps below:

Run the `sudo dstat --top-io --top-bio` command to get the process name with I/O activity.
Run the `pgrep python3` command to get the PID of the process.
Run `sudo lsof -p <PID>` to list the open files by the process.
Run `sudo lsof -p <PID> | awk '{print $9}' | while read file; do df $file; done` to get the device details.

Find the actual partition used by running the pvs command and store the actual device name in /opt/devname.txt.
Run the command below to get the PID of the process with high kB_read/s:

   `sudo pidstat -d 1`

==================================================================================================================================================================================
## Question 16 

On node01 list all filesystems to check out how much free space they have remaining. You'll find one which is almost full (should be around 98% full). To confirm it is the correct filesystem, see where it is mounted, and you should find many directories on it in the form of numbers from 1 to 999. Find the directory which has the largest file and delete that file (only that file, nothing else).

***Answer***

Run the below command to get the largest file:

```sh
bob@node01:~$ sudo find /data -type f -exec du -h {} + | sort -rh | head -n 1
196M    /data/683/lf
```

Delete only the largest file:

`sudo rm -rf /data/683/lf`

==================================================================================================================================================================================
## Question 17 

On caleston-lp10 change the configuration for the SSH daemon. Disable X11 forwarding globally. Then, make an exception for just one user called bob. For that user alone enable X11 forwarding.

Do not restart the SSH service after making the changes.

***Answer***

1) To disable X11 forwarding globally, find the line that contains X11Forwarding in /etc/ssh/sshd_config
. It may be commented out by default with a #. Change it to:

`X11Forwarding no`

2) To enable X11 Forwarding for User bob, add a conditional block at the end of the sshd_config file to enable X11 forwarding specifically for the user bob:

Match User bob
    `X11Forwarding yes`







