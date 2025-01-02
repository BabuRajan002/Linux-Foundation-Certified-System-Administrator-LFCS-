# System wide Environment variables

* `printenv = $env` - Prints the system wide envuronment variables
* If a user wants edit their own environment variable `.bashrc` would be the right file
* `/etc/skel` - Whenever a new user created in the system all the files will be copied from this directory to user's home dir. 

# Configure user resource Limits

* `sudo vim /etc/security/limits.conf` - Set the user's resource limits

* Breakdown of Limits.conf file: 

1. domain - Tells the user names or group names
2. type - Indicates the soft or hard (hard: Maximum process a user can run, soft: minimum process a user can run)
3. Item - 
4. count

* `unlimit -a` - To see limits for the current session

# Manage user privilleges

* `groupadd -aG sudo jane` - Add a Jane user to the sudo group
