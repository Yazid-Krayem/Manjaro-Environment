# Manjaro Nvidia driver install 

 Step 1: remove `Nvidia bumblebee` // check from Manjaro Settings   		 Manager  >> Hardware Configuration

### reboot the system 

 Step 2: Install the `NVIDIA driver` // Also from  Hardware Configuration **OR** you can use `mhwd` 
 
## DO NOT REBOOT THE SYSTEM NOW 

Step 3: install `LightDM` && `GDM`. 


Step 4: set up a new Xorg configuration
		
   1. `sudo rm /etc/X11/Xorg.conf.d/90-mhwd`
		
   2.  `sudo nano /etc/X11/xorg.conf.d/optimus.conf`
   
   and insert inside optimus.conf this :
   
	   
  	 Section "Module"
    	Load "modesetting"
	  EndSection

	  Section "Device"
	    Identifier "nvidia"
	    Driver "nvidia"
   		BusID "PCI:1:0:0"
   	 	Option "AllowEmptyInitialConfiguration"
  	EndSection
	   
		
  3. Make double check on `BusID` using this command `lspci | grep -E "VGA|3D"`
  	 
Step 5: Refine blacklisting

PRIME relies on nvidia-drm and mhwd puts it in a blacklist by default,
but to ensure the nvidia kernel module will load we still need to blacklist certain other modules.
Therefore, you’ll have to do some editing of the files in `/etc/modprobe.d/`

   1. Remove the existing blacklist
     
   	  	sudo rm /etc/modprobe.d/mhwd*
   		
   2. create new file 
   
   		 sudo rm /etc/modprobe.d/nvidia.conf	
   		 
>> INSERT inside it 

   		blacklist nouveau
		blacklist nvidiafb
		blacklist rivafb

Step 6: Enable nvidia-drm.modeset

 Create a new file in `/etc/modprobe.d` folder
    
    		sudo nano /etc/modprobe.d/nvidia-drm.conf    		
>> INSERT 
 		
 		options nvidia_drm modeset=1

Step 7 : Set the output source for your DM

1. Create a new file in `/usr/local/bin` folder
    		
    		sudo nano /usr/local/bin/optimus.sh
    		
>> INSERT 

		#!/bin/sh

		xrandr --setprovideroutputsource modesetting NVIDIA-0
		xrandr --auto
	
2. Make sure to set it would read-execute:
	
		chmod a+rx /usr/local/bin/optimus.sh

Step 8: Now you have to get this to load in your DM’s startup sequence

1. **Edit**  `/etc/lightdm/lightdm.conf` 

	sudo nano /etc/lightdm/lightdm.conf 
>> Search  for :

		#Display-setup-script= Script to run when starting a greeter 	session (runs a root )
		
>> and Replace it with : 
		display-setup-script=/usr/local/bin/optimus.sh

Step 9: `GDM` >>  Create a new file in `/etc/local/share/` folder 

		sudo nano /usr/local/share/optimus.desktop
>> Insert 

		[Desktop Entry]
		Type=Application
		Name=Optimus
		Exec=sh -c "xrandr --setprovideroutputsource modesetting NVIDIA-0; xrandr --auto"
		NoDisplay=true
		X-GNOME-Autostart-Phase=DisplayServer

2. And link it into place so it starts with GDM and on Login

		sudo ln -s /usr/local/share/optimus.desktop /usr/share/gdm/greeter/autostart/optimus.desktop

		sudo ln -s /usr/local/share/optimus.desktop /etc/xdg/autostart/optimus.desktop

Step 10: `SDDM` 

(you can install it if you don't have it  `sudo pacman -S sddm` )

1. Create a new file in `/usr/share/sddm/scripts/` folder 
	
		sudo nano /usr/share/sddm/script/Xsetup
		
>> INSERT 

		xrandr --setprovideroutputsource modesetting NVIDIA-0
		xrandr --auto



## REBOOT 

IF gives you a `black screen`  :

1. Press `Alt + Ctrl + F2` to take you to ttye2 
2. login 
3. use this command `startx`

 ## [README](README.md)
