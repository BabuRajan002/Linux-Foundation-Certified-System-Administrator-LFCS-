# Create, Delete and modify the user

* `sudo adduser dan` - Adding a new user. Which creates group called dan, home directory /home/dan and it assigns a default shell /bin/bash
* `sudo deluser dan` - Delete a user
* `sudo adduser --system --no-create-home sysacc` - To create a system account
* `sudo usermode --home /home/somenewdir --move-home dan` OR `sudo usermode -d /home/somenewdir -m dan`  - Modify the user's home directory
* `sudo usermod -L jane` - To Lock the user account
* `sudo usermod --unlock OR -U jane` - To unlock the account
* `sudo usermod -e 2026-10-02 jane` - To set the expiry date for the user account
* `sudo chage -d 0 jane` - To set the immediate expiry date for the password of Jane
* `sudo groupadd developers` - To create a group
* `sudo gpaaswd --a dan developers` - to add user into the developers group
