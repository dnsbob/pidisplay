# pidisplay
This branch is for Raspberry Pi Debian Trixie 64-bit    
Currently works on a Raspberry Pi Zero 2 W    

Use a raspberry pi to display a video in a loop.  If there is a USB drive inserted when it is booted, copy a new video from the drive.  The new video must be in 'mydirectoryname' on the USB drive.

Setup:
Username is assumed to be "pidisplay".     
If you choose a different username, update "/home/pidisplay" in all the files, and update the commands below.    

Using the "Raspberry Pi Imager" or your favorite image writer, write the "Raspberry Pi OS (64-bit)" image to a micro-SDcard of at least 4GB size.  (This was tested with Debian Trixie)
You can try the settings if you like, but they did not work for me; I had to configure with screen, keyboard, and mouse after powering up.

Click the upper left "Raspberry" symbol, "Preferences", "Control Centre".  Then "Interfaces" and turn on SSH and set a password.  Then choose "System" on left side of Control Centre and change "Boot" to "To CLI", and turn off both "Console auto login" and "Desktop auto login".  Click "Close"

If you prefer to work from another machine, you can use SSH:
On the Pi, open a terminal window and run the command:
```
ip a | grep global
```
Then you can ssh from another machine using any of the IP Addresses shown, and the username you configured.

Clone the repo on the Pi:
Open a terminal window or ssh.
```
git clone https://github.com/dnsbob/pidisplay.git
cd pidisplay
```

If you do not have a "pidisplay" user, create it and set the password (might need to edit the list of groups):       
```
sudo useradd -c 'play video in a loop' -m -s /bin/bash -U -G adm,dialout,sudo,video,render pidisplay
sudo passwd pidisplay
```

For a little security, it will only update a new video from a USB stick if it is in the right directory.    
- Create a file containing the directory name, edit it to set the name
```
sudo cp pidisplay.env /home/pidisplay/pidisplay.env
sudoedit /home/pidisplay/pidisplay.env
sudo chown pidisplay /home/pidisplay/pidisplay.env
```
- So on the usb stick it will look for "my_display_directory_name/*.mp4"    

Create the required directories and sample video
```
sudo mkdir /home/pidisplay/current
sudo mkdir /home/pidisplay/new
sudo mkdir /home/pidisplay/old
sudo cp *.mp4 /home/pidisplay/current/
sudo chown pidisplay /home/pidisplay/current /home/pidisplay/new /home/pidisplay/old
```

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
sudo -l -U pidisplay
```

For security, if not already configured:    
Turn off automatic login and set boot to terminal mode using
```
sudo raspi-config
```
Choose "1 System Options", "S5 Boot", "B1 Console Text console", enter
Choose "1 System Options", "S6 Auto Login", tab to "No", enter, "Ok"

A "pi zero 2" is sufficient.  A "pi zero" might work.
