## init - system
* Init system knows to start the applications. 
* systemd units is responsible for starting the applications

## units

* service
* socket
* device
* timer

* ***`systemctl resload sshd.service`*** - Its a gentle way of restarting the application without any interruption. But not all application will support this graceful restart. 
* ***`systemctl restart sshd.service`*** - It will restart the application in disruptive way. Any user using that service will be affected!.
* ***`systemctl reload-or-restart sshd.service`*** - This command tried graceful reload first if that is not supported by that application it will restart it. 
* ***`systemctl mask atd.service`*** - this will prevent the application to start automatically. 
* ***`systemctl list-units --type service -all`*** - This will list all the systemd unit services available in the system
* ***`systemctl daemon-reload`*** - When we add/remove the systemd units we need to run this command first!