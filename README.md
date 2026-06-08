# pidisplay
This branch is for Raspberry Pi Debian Trixie 64-bit    
Currently works on a Raspberry Pi Zero 2 W    

Use a raspberry pi to display a video in a loop.  If there is a USB drive inserted when it is booted, copy a new video from the drive.  The new video must be in 'mydirectoryname' on the USB drive.

Setup:
Username is assumed to be "pidisplay".     
If you choose a different username, update "/home/pidisplay" in all the files, and update the commands below.    

Create a user and set the password (might need to edit the list fo groups):       
```
sudo useradd -c 'play video in a loop' -m -s /bin/bash -U -G adm,dialout,sudo,video,render pidisplay
sudo passwd pidisplay
sudo -l -U pidisplay
```
Make sure the user has sudo ALL without a password.

For a little security, it will only update a new video from a USB stick if it is in the right directory.    
- Create a file containing the directory name, edit it to set the name
```
sudo cp pidisplay.env /home/pidisplay/pidisplay.env
sudoedit /home/pidisplay/pidisplay.env
```
- So on the usb stick it will look for "my_display_directory_name/*.mp4"    

create /home/pidisplay/bin and copy these files there:  
```
sudo mkdir /home/pidisplay/bin
sudo cp -p checkusb displayloop play start /home/pidisplay/bin
```

Create and start a new systemd service:
```
sudo cp pidisplay.service /usr/lib/systemd/system/pidisplay.service
sudo systemctl enable --now pidisplay.service
```

Create log file dir and file:
```
sudo mkdir /var/log/pidisplay
sudo chown pidisplay /var/log/pidisplay
```

Update sudoers to allow a few commands without a password:
```
sudo cp 314_pidisplay /etc/sudoers.d/314_pidisplay
sudo chmod 440 /etc/sudoers.d/314_pidisplay
```

For security:    
Turn off automatic login and set boot to terminal mode using
```
sudo raspi-config
```

A "pi zero 2" is sufficient.  A "pi zero" might work.
