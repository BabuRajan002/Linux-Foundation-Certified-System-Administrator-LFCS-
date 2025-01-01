# apt - Ubuntu

* `sudo apt update` - This command updates the local database of the machine by downloading the latest packages which is stored in the server.

* `dpkg --listfiles nginx` - To list the package after installed
* `dpkg --search /usr/sbin/nginx` - To list this binary file related to which package
* `sudo apt remove nginx` - To remove the package only. Sometimes it will not remove its dependencies
* `sudo apt autoremove nginx` - To remove the package and its dependencies completely.

    
# Process Integrity

* `systemctl list-dependencies` - To check whether all the process in the system is running fine or not. 
