# pidisplay
This branch is for Raspberry Pi Debian Trixie 64-bit    
Currently works on a Raspberry Pi Zero 2 W    

Use a raspberry pi to display a video in a loop.  If there is a USB drive inserted when it is booted, copy a new video from the drive.  The new video must be in 'mydirectoryname' on the USB drive.

Setup:
- add to /root/.profile:  
PI_USB_DIR=mydirectoryname

create /root/bin and copy these files there:  
```
checkusb
displayloop
play
start
```

Create a user:    
```
useradd -c 'play video in a loop' -m -s /bin/bash -U pidisplay
```

Create and start a new systemd service:

```
sudo cp pidisplay.service /usr/lib/systemd/system/pidisplay.service
sudo systemctl enable --now pidisplay.service
```

For security:
Turn off automatic login and set boot to terminal mode using
```
sudo raspi-config
```

A "pi zero 2" is sufficient.  A "pi zero" might work.
