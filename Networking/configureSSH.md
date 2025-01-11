# Configure SSH

To manage the remote Linux servers, we need to two things one is ssh client on local server and ssh daemon on the remote server. 

* `sudo vim /etc/ssh/sshd_config` - this is ssh daemon config file ***Server***
* `sudo vim /etc/ssh/ssh_config` - this is ssh client config file  ***Client***
* `ssh-keygen -R 10.0.0.251` - This is to remove the remote server's fingerpring in the client machine in the `known_hosts` file
