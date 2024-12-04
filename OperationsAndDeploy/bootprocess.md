# Different targets

`sudo systemctl get-default` - To display current target
`sudo systemctl set-default multi-user-target` -  To change the systemd target to multiuser target - requires reboot
`sudo systemctl isolate graphical-target` - Switching the target without any reboot

`sudo systemctl isolate emergency-target` - root filesystem mounted read-only. Useful for debugging
`sudo systemctl isolate rescue-target` - in this target it will land into root shell with only few programs loaded. 

Inorder to log in these targets we must know the root user passwords. 
