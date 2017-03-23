---
layout: post
title:  "Setting up a Linux OS"
date:   2017-03-23 16:34:34 +0000
---


Alright, so you’re currently running a Mac OS X and you want to try coding in a LAMP environment. But you don’t want to completely replace your current OS.

Let’s figure this out.

<iframe src="//giphy.com/embed/gKvbeP7g4Vp3G" width="480" height="244.32432432432432" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/try-counsellor-qualitiesthings-gKvbeP7g4Vp3G">via GIPHY</a></p>

LAMP stands for Linux Apache MySQL PHP. It is a coding environment using a Linux operating system, an Apache server, a MySQL database, and PHP as a coding language.

So there are a couple options when it comes to installing a Linux OS, you can either install Ubuntu (a Linux OS) on a partition or you can install VirtualBox and run it as a Virtual Machine on your computer.

A partition is great because you have greater flexibility with what you want to do on your new Linux OS. (If you want to play games on it, choose this option).

A Virtual Machine is pretty cool because you can have your current OS running in the background while you play around on Ubuntu, and can switch back and forth between your two operating systems without having to restart your computer. (If you’re lazy and/or don’t need the aforementioned flexibility, choose this option).

There are upsides and downsides to both. So just choose the best fit for you. There’s nothing to stop you from having both a dual booting option and a virtual machine.

I’m not planning on doing anything graphic intensive on my Linux OS so I’m going to go ahead with Virtual Box.

Onwards!



Firstly, head over [here](https://www.virtualbox.org/wiki/Downloads) and select the OS X hosts option. This downloads a disk image (dmg) file which you then need to mount (click on it). 



Next run the installer and then open VirtualBox from your Applications folder. A welcome box will pop up. Leave it alone for a minute. We’ll get back to that later.



Next we are going to install Ubuntu. Head over [here](https://www.ubuntu.com/download) and then click the Ubuntu Desktop version and download it.

<iframe src="//giphy.com/embed/FgiHOQyKUJmwg" width="480" height="414.2608695652174" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/download-FgiHOQyKUJmwg">via GIPHY</a></p>


Head back over to your VirtualBox window and click on ‘New’. Now we are going to create our new Virtual Machine. Select Linux for your ‘type’ and Ubuntu (64-bit) as your ‘version’. Then add in a name. Mine is pretty boring - ‘Ubuntu 16.04.2’.

Click ‘Continue’ and select the amount of memory you want for your virtual machine. There will be a recommended memory size that it is already set to. Set it to your preference, I’m going to go ahead with the requirement.

Click ‘Next’ and some Hard disk dialogue will show up. Click on the ‘Create a virtual hard disk now’ option and click ‘Create’.

A Hard disk file type menu will show up, click on ‘VDI (VirtualBox Disk Image), and click ‘Continue’.

Next, choose the ‘Dynamically allocated’ physical hard disk option and click ‘Continue’. This will take up the least amount of space on your hard disk.

On the next menu, you will be asked to set the size of the virtual hard drive. The recommended size is 8GB, however, the first time I did this, I stuck with the minimum requirement and got an error message saying “You need at least 8.6GB to install Ubuntu, your system only has 8.6GB’. Ummm???

<iframe src="//giphy.com/embed/3ornk9rZlQKaDRmJcA" width="480" height="264.51063829787233" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/latenightwithsethmeyers-seth-meyers-late-night-with-3ornk9rZlQKaDRmJcA">via GIPHY</a></p>

So yeah, up it a bit. I made mine 20GB. Click ‘Create’.

Alrighty. Now you’ll be taken back to the VirtualBox Manager where you’ll see your Ubuntu virtual machine in the upper left corner set to ‘Powered Off’. 

Now we are going to load Ubuntu onto our virtual machine.

Alright, so on our VM VirtualBox Manager, your Ubuntu virtual machine is highlighted, and information about it is being displayed in the window. Click on the ‘Storage’ section and a window will pop up (make sure you click on the actual letters and not on the section itself, which will just collapse the section). Make sure that ‘Controller: IDE’ is highlighted, and click the little CD with a + sign on its right-hand-side, and select ‘choose disk’ to add a new optical drive. Now you need to find your Ubuntu ISO file that you downloaded earlier (its probably in your downloads folder), select it, and click ‘Open’.

Alright, so now you’ve got your ubuntu file mounted under Controller:IDE. Now you are going to go click on the ‘System’ tab. You will see a ‘Boot Order’ section that will have check marks next to ‘Floppy’, ‘Optical’, and ‘Hard Disk’. Click and drag optical to the top of that list then click ‘OK’.

Alright, now we are ready to get our machine up and running.

Click on the ‘Start’ arrow and your Ubuntu virtual machine will load in a separate window.

You will see a language option, choose your preferred language and then click the ‘Install Ubuntu’ option. Next, select ‘Download updates while installing Ubuntu’ and then ‘Continue’.

Then select ‘Erase disk and install Ubuntu’. And click ‘Continue’.

Next you will be asked to select your time zone from the map. Go ahead and do that. Then you will be prompted to select your keyboard layout. 

Almost there. You will need to fill in the ‘Who are you?’ section, and then Ubuntu will start to work its installation magic. 

Installation will require a restart, once that’s done, click on ‘Devices’ in the top menu and select ‘Insert Guest Additions CD image’. This will require you to type in your password.

This will boot up your terminal. When it is done running it will prompt you to press enter. You will need to restart your Ubuntu system one more time. In order to do that, click the power icon in the upper right hand corner of the screen. A window will pop up asking whether you want to restart or shutdown. Choose restart.

Lastly you will need to click on ‘View’ in the top bar, and then ‘Auto-resize Guest Display’. Now you’re all set!

<iframe src="//giphy.com/embed/f2fX7GtXh1nbi" width="480" height="312" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="http://giphy.com/gifs/yay-anchorman-f2fX7GtXh1nbi">via GIPHY</a></p>


