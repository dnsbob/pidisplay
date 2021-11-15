# pidisplay
Use a raspberry pi to display a video in a loop.  If there is a USB drive inserted when it is booted, copy a new video from the drive.

Setup:
- add to /root/.profile:  
PI_USB_DIR=mydirectoryname

create /root/bin and copy these files there:  
```
checkusb
clearconsole
displayloop
play
```

Add to /etc/rc.local before the 'exit 0' line:
```
/root/bin/checkusb
/root/bin/displayloop >> /var/log/displayloop.log 2>&1 < /dev/null &
```
