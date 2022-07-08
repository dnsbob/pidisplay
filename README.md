# pidisplay
Use a raspberry pi to display a video in a loop.  If there is a USB drive inserted when it is booted, copy a new video from the drive.  The new video must be in 'mydirectoryname' on the USB drive.

Setup:
- add to /root/.profile:  
PI_USB_DIR=mydirectoryname

create /root/bin and copy these files there:  
```
checkusb
clearconsole
displayloop
play
start
```

Add to /etc/rc.local before the 'exit 0' line:
```
/root/bin/start
```

For security, the pi should be configured not to auto-login, and not start he desktop.
A "pi zero" is sufficient.
Needs the older "buster" version of the operating system.  omxplayer does not seem to work on the newer "bullseye" version, and nothing else works for me.
2022/7/7
