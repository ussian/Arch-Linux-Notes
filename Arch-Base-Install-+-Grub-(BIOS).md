#### Table of contents
 * [Pre reading notes](Arch-Base-Install-+-Grub-(BIOS).md#pre-reading-notes)
 * [Create bootable media](Arch-Base-Install-+-Grub-(BIOS).md#create-bootable-media)
 * [Arch Linux iso image Boot Menu](Arch-Base-Install-+-Grub-(BIOS).md#arch-linux-iso-image-boot-menu)
 * [First things first](Arch-Base-Install-+-Grub-(BIOS).md#first-things-first)
 * [Partioning and mounting](Arch-Base-Install-+-Grub-(BIOS).md#partioning-and-mounting)
 * [Formating](Arch-Base-Install-+-Grub-(BIOS).md#formating)
 * [Configuration and installalltion base system](Arch-Base-Install-+-Grub-(BIOS).md#configuration-and-installalltion-base-system)

### Pre reading notes

 Notes completed 03-06-2017
 Last update 03-06-2017

To print this file in a decent format, click the "Raw" button in the top right corner of this box.
Disclaimer: Im no way a Linux or Arch Linux pro user, ive just written down what i did to make it work for me.


A good place to read up on most if not all of this stuff is the [Arch wiki](https://wiki.archlinux.org/).
Else a quick google search can get you a long way

### Create bootable media

Make a bootable device(USB, CD/DVD etc.) with archlinux iso ([Download site](https://www.archlinux.org/download/)). <BR>
You can do this with [Rufus](https://rufus.akeo.ie/) on windows (its free), or if using Ubuntu follow these [instructions] (https://www.ubuntu.com/download/desktop/create-a-usb-stick-on-ubuntu).
Else you run the following command or something similar.

`sudo dd bs=4M if=/home/ussian/Downloads/ArchVersion.iso of=/dev/sdb1 && sync`<BR>
replace the `/home/ussian/Downloads/ArchVersion.iso` and `/dev/sdb1` with the appropiate path.<BR>
### Arch Linux iso image Boot Menu<BR>
Boot the Arch install media (USB, CD/DVD etc.)<BR>
You do this by rebooting and at the beginning of the startup process (ie. BIOS startup) and hiting F9 (or whatever F# button that opens the bootmenu) and then choosing your install media. <BR>
If it´s booted correctly into the bootmedia, it should greet you with an Arch Linux menu and the following options <BR><BR>
    `Boot Arch Linux (x86_64)` (x86_64 means 64 bit) <BR>
    `Boot Arch Linux (i686)` (i686 means 32 bit) <BR>
    `Boot existing OS` <BR>
    `Run Memtest86+ (Ram test)` <BR>
    `Hardware Information` <BR>
    `Reboot` <BR>
    `Power Off` <BR><BR>
    
OBS: if you are using a Nvidia 900 series with booting the arch linux. To fix this: highlight your bit version of arch and hit E this will bring up some new  text. Go to the beginning of that text, using the left arrow key, type 'nomodeset' and a space. Then hit enter. [Reference](https://wiki.archlinux.org/index.php/nouveau#NVIDIA_900_series_card_issues) & [video](https://www.youtube.com/watch?v=MMkST5IjSjY)  
    
You can navigate the menu with the arrow keys and click enter to choose the highlighted option. <BR>
The option `Boot Arch Linux (x86_64)` wont be availble if your machine can´t run 64 bit OS <BR><BR>

For these notes i will be installing x86_64 Arch <BR>
After choosing 64 or 32 bit arch will do its thing for a while and when its ready it will greet you with the following:<BR>
    `Arch Linux "(version number)"` <BR>
    `archiso login: root (automatic login)`<BR>
    `root@archiso ~ #`<BR>

This means you have a root prompt ready for use. **Be warned** you can potentionaly destroy all data on your disks in this terminal. its most likely to happen during the partioning and formatting.<BR>

### First things first

First thing to is remove the anoying beeb sound whenever you tab-complete or try to tab-complete a command. (or doing something the terminal doesnt like in general)<BR>
`setterm -blength 0`


Second thing to do is load the correct keymap (The standard is US)

replace DK with yours country´s letter (eg. dk for denmark, uk for united kingdom, us for united states, es for spain)<BR>
`loadkeys dk`<BR>
This command is only temporary, that means after restart it will be back at US


Next up is to check your internet connection (I recommend that you use a Ethernet connection ie. a cable connection)<BR>
`ping google.dk`<BR>
To stop ping'en hit `CTRL + C´ <BR>
You can allways swap out google with a site you know that you can reach.<BR>
If you get "Name or service not know" you either typed the site wrong or you got no connection


To check the time of the system type:<BR>
`timedatectl`<BR>
This should show you a date and a clock, plus some more stuff. <BR>
to make sure the clock is accurate do this:<BR>
`timedatectl set-ntp true`<BR><BR>




### Partioning and mounting
There is a handfull of tools and commands avaiable ill only be using some of them. You check the list [here](https://wiki.archlinux.org/index.php/Partitioning#Partitioning_tools)<BR>
To list file systems and disks:<BR>
`lsblk -f -p`<BR>
This should show something like this:<BR>
```
    NAME         FSTYPE      LABEL       UUID
    /dev/sdb                                                                                                 
    └─/dev/sdb1  vfat        ARCH_201701 1702-0227                               /run/archiso/bootmnt        
    /dev/sr0                                                                                                 
    /dev/loop0   squashfs                                                        /run/archiso/sfs/airootfs   
    /dev/sda                                                                                                 
    └─/dev/sda1  ext4                    f2b67d75-090a-4d96-8dfb-e814dd07fd13                                
```
The "`/dev/sdb`" is my pc disk with my previous Arch Linux installation while the "`/dev/sda`" is my bootable usb-disk, where the Arch is image is placed.<BR>
Disk names in linux are named after the alfabet, so `sd*`(where * is the next letter in the alfabet). (eg. `sda` for the first disk, `sdb` for the secound disk, `sdc` for the third disk etc.)<BR>
The secound line "`└─/dev/sdb1`" is a partion of disk "`sdb`" so partions are named after number (eg. `sdb1`, `sdb2`, `sdb3`)<BR>
You should just ignore "`/dev/sr0`", "`/dev/loop0`" and your bootmedia (in my case `/dev/sda`). All of these are used by the bootmedia.<BR>

There is alot of different ways to partion your disks so for more info check out the [Arch Wiki](https://wiki.archlinux.org/index.php/partitioning#Partition_scheme) (or look it up on google)<BR>
For these notes i will make just 1 partion (a root partion) for everything.<BR>
A command for that is the following:<BR>
`cfdisk /dev/sda`<BR>

Replace "`sda`" with the disk you want to install Arch Linux on<BR>
That command will bring up a "Pseudo-graphics" interface. You use the arrow keys to navigate.<BR>

If you by mistake deletes a partion you didn´t want to delete, just click enter when you are hovering over "`[   Quit    ]`"<BR>

I only have 1 partion so i will be deleting just that by hovering over "`/dev/sda1`" with the up and down arrow keys and choose "`[ Delete ]`" by moving the hover bar with left and right arrow keys.<BR>
When you are hovering over the partion and "`[ Delete ]`" hit enter to delete it.<BR>

There should now be a new device called "`Free space`" which obviously isn´t a device but is just unallocated space<BR>
If you have multiple partions that you delete, those will add up into the free space.<BR>

To make a new partion hover over the "`Free space`" and "`[   New   ]`" then hit enter.<BR>
It will by default suggest the maximum size of the partion which should amount to the same amount of free space<BR>
And this is, as i said earlier, where there are multiple choicses on what to do and for those go look at the [Arch Wiki](https://wiki.archlinux.org/index.php/partitioning#Partition_scheme) or google it<BR>

Well i´ll be making just one root partion. To do this, you just choose the default size (eg. all of the free space)<BR>
After choosing the size it will ask if should be a "`[ primary ]`" or "`[extended]`" you should choose "`[ primary ]`" if you are gonna have  four partions or less (more information [here](http://www.howtogeek.com/193669/whats-the-difference-between-gpt-and-mbr-when-partitioning-a-drive/)).<BR>
When you have choosen the partion type you should be back at the same screen as when you ran the "cfdisk" command only difference would be the partions.<BR>

I will be marking my single partion as "bootable" by hovering over the partion and the option "[bootable]" then hitting enter<BR>
More information on which partion should be bootable in the [Arch Wiki](https://wiki.archlinux.org/index.php/partitioning#Partition_scheme)<BR>

To confirm the partion changes you will need to hover over the "`[  Write  ]`" option and hit enter<BR>
You will then be asked if you are sure that you want to make these changes. type "`yes`" or "`no`" depending if you are done. Be warned you type "`yes`" you wont be able to get the data back.<BR>
Afterwards you just the hoverbar to the "`[  Quit  ]`" and hit enter to exit.<BR>


### Formating
Now when we are done with partioning we need to format the partion(And again more details in [Arch Wiki filesystems](https://wiki.archlinux.org/index.php/File_systems#Create_a_filesystem))<BR>
We will just format our single root partion to the filsystem "ext4". To do this run this command:<BR>
`mkfs.ext4 /dev/sda1`<BR>
Remember that we format partions and NOT disk so put that number at the end, even if you only have one partion.<BR>
If the partion contains a filesystem (even if you did delete your partion) it will ask if you want to proceed anyway. Here you should answer "`y`".<BR>
if any errors occur, try googling it :)<BR>


Now we are done with partioning and formatting and we now need to mount our newly made ROOT partion filesystem, you do this like so:<BR>
`mount /dev/sda1 /mnt`<BR>
"`/dev/sda1`" is you partion and "`/mnt`" is where you mount the partion to.<BR>
you´ll also need to mount your other partions if you have any other partions. I wont be doing this i´ll only show how to do it. So i wont be doing anything inside the box below.<BR>

```
┌───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│# create a folder for your partion on you mounted root partion for this example ill be doing it for the "boot" partion │
│mkdir /mnt/boot                                                                                                        │
│# That will create the boot folder on your root partion                                                                │
│# Now you need to mount the boot partion to your boot folder:                                                          │
│mount /dev/sda2 /mnt/boot                                                                                              │
│# /dev/sda2 should be changed into the coresponding partion (ie. if you boot partion is "/dev/sdb3")                   │
└───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

### Configuration and installalltion base system


The `base` is needed for Arch Linux and the `base-devel` is used for build packages and more.<BR>
I recommend getting the base-devel aswell doesn´t use much space and if you dont know if you need it, might aswell get it.<BR>
`pacstrap /mnt base base-devel`<BR>
This will install the `base` system and `base-devel` on to the `/mnt` (And remember we mounted our `/dev/sda1` to `/mnt`. So we are bassicly installing these packages to the new partion)<BR> 

To generate the fstab file do the following:<BR>
`genfstab -U /mnt >> /mnt/etc/fstab`<BR>
The fstab file is used by the system to know what drives to mount automaticly and whetever we should have read/write access and [more](http://www.howtogeek.com/howto/38125/htg-explains-what-is-the-linux-fstab-and-how-does-it-work/)

Now to install grub, you´ll need to install `grub-bios`  onto your new system (located at `/mnt`)(this will make `grub-install` command availble when you are in your new system)<BR>
`pacstrap /mnt grub-bios`<BR>

This will change your root at the bootable media to the root on your newly installed packages.<BR>
`arch-chroot /mnt /bin/bash`<BR>


To disable the beeb sound again.<BR>
`setterm -blength 0` (non perm)<BR>
(this should be perm) Uncomment `set bell-style none` by changing<BR> 
`# set bell-style none`<BR>
to <BR>
`set bell-style none`<BR>
or if its not there just add it.<BR>

And to make a persistent keymap layout do:
To list all avaible keymaps do the following<BR> 
`find /usr/share/kbd/keymaps | less` <BR>

Or to find only those with your country letters. (this finds all files in keymaps that contains "dk". change "dk" to your country letters) <BR>
`find /usr/share/kbd/keymaps | grep -i dk` <BR>
You will propally see something like this <BR>
    `/usr/share/kbd/keymaps/i386/qwerty/dk-latin1.map.gz` <BR>
    `/usr/share/kbd/keymaps/i386/qwerty/dk.map.gz`<BR>
    `/usr/share/kbd/keymaps/i386/qwerty/mac-dk-latin1.map.gz`<BR>
    `/usr/share/kbd/keymaps/i386/qwertz/de-latin1-nodeadkeys.map.gz`<BR>
    `/usr/share/kbd/keymaps/mac/all/mac-de-latin1-nodeadkeys.map.gz`<BR>
    `/usr/share/kbd/keymaps/mac/all/mac-dk-latin1map.gz`<BR>
    
For more information on "nodeadkeys" look here ([dead keys](http://askubuntu.com/questions/56560/what-exactly-is-meant-by-eliminate-dead-keys)). <BR>
I dont know the difference between "latin1" and non "latin1", so just try the one or the other and test the layout by typing speciel characters. if the wrong character comes on the screen try another keymap.

To set current keymap, and to make it permantly do the following (replace dk.map.gz with your keymap:<BR>
`localectl set-keymap --no-convert dk.map.gz`


Now the continueation of grub install. (This will install grub and not only the tools for it)<BR>
`grub-install /dev/sda`<BR>
Then to generate the grub config file:<BR>
`grub-mkconfig -o /boot/grub/grub.cfg`<BR>

To view all the avaible time zones run this command<BR>
`ls /usr/share/zoneinfo`<BR>
This will list all the different regions and the next command you´ll need to replace region with your region<BR>
`ls /usr/shar/zoneinfo/region`<BR>
This will list all cities in that region.<BR>
To set the timezone, replace Region and City with you region and city (Remeber it´s case sensitive)<BR>
`ln -sf /usr/share/zoneinfo/Region/City /etc/localtime`<BR>

The following will set the hardware clock from the system clock.<BR>
`hwclock --systohc`<BR>


The command below i used to generate an intial ramdisk based on the linux preset (read more in `man mkinitcpio` or [Here](https://wiki.archlinux.org/index.php/mkinitcpio#Image_creation_and_activation))<BR>
`mkinitcpio -p linux`<BR>

Replace `HOSTNAME` with a hostname of your choice<BR>
`echo HOSTNAME > /etc/hostname`<BR>

For a list of you network devices<BR>
`ip link`<BR>
You´ll get mulitiple network devices depending on your pc (ie. a laptop may have three. one for loopback another for your ethernet and last one for wireless)<BR>
You should look for one with the `state UP mode` and remeber/take note of the first couple of letter like `enp0s25` <BR>
Then to enable your ethernet dhcp service for your ethernet<BR>
`systemctl enable dhcpcd@enp0s25.service`<BR>

Now to install some not 100% esiential, but i still think that it´s nesesarry.<BR>
`pacman -S dialog iw networkmanager network-manager-applet gnome-keyring`<BR>
you´ll be asked what package to choose. Ill just go with the default `mesa-libgl` (you may want to google the different packages)<BR>

Create a root password.<BR>
`passwd`<BR>
Now enter a root password of choice, twice. (Make it secure!)<BR>

Now the restart process. First jump out to the root on the bootmedia:<BR>
`exit`<BR>
Then unmount your drives and partions<BR>
`umount -R /mnt`<BR>
And at last reboot the system<BR>
`systemctl reboot`<BR>
When the system is shutdown (in the reboot sequence) unplug the bootmedia or make sure that your bios doesnt boot into you arch-iso bootmedia

When its booting you´ll see the grub screen (here you can choose what os to boot) it will automatically boot arch in couple of secounds.<BR>
And arch i done booting when you see:<BR>
```
Arch Linux "inser version number"-ARCH (tty1)<BR>
<BR>
localhost login:<BR><BR>
```
The user name is `root` and the password is what you set it to earlier.<BR>

And congratulaition you now have an Arch Linux OS running! <BR>
The rest of this is just personalisation, but feel free to read on. (change packages out like you want (ie. desktop envoirment etc.))<BR>


### Personalisation of arch

#### add user
First off we´ll wanna add user so we dont run around as root (for more information on `useradd` do `man useradd`)<BR>
`useradd -m -g users -s /bin/bash ussian`<BR>
swap out `ussian` with the username of your choice<BR>
And then set the password by doing like so (and again swap out ussian with your username): <BR>
´passwd ussian`<BR>
Then enter the desired password twice.<BR>
#### sudo
Now install sudo to be available to be able to do things as root<BR>
`pacman -S sudo`<BR>
`pacman` is arch package manager.<BR>

edit the sudo file to add your user `visudo` will open it in `vi` editor, else change nano into your preferred editor<BR>
`EDITOR=nano visudo`<BR>
Now when it´s open scroll down to "user privelege specification" and add the following line beneath root (replace `ussian` with your username)<BR>
`ussian ALL=(ALL) ALL`<BR>
to save the file click "Ctrl" and "o" then "enter". To exit click "Ctrl" and "x" <BR>
you can now logout of root and login into your user.<BR>

#### pacman.conf adding "multilib" and installing "yaourt" (AUR support)
Open you pacman.conf file<BR>
`sudo nano /etc/pacman.conf`
Go down to `#[multilib]` uncomment that, the line under and add the other 3 lines:<BR>

```
[multilib]
Include = /etc/pacman.d/mirrorlist
```
```
[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/$arch
```

To save and exit: "Ctrl" + "o" -> "enter", then "Ctrl" + "x".<BR>
Then Run this: (Do `man pacman` to learn what the different parameters do)<BR>
`sudo pacman -Syyu`
`sudo pacman yaourt`

#### Kill that beep sound for good (ie. killing pc speaker)
Jump into a root shell:<BR>
`sudo su`<BR>
then do:<BR>
`echo "blacklist pcspkr" > /etc/modprope.d/nobeep.conf`<BR>
Then exit to get back to the session with your user.<BR>
`exit`<BR>

#### Some good to install packages
`sudo pacman -S multilib-devel fakeroot git jshon wget make pkg-config autoconf automake patch` <BR>
It will ask which one you want. Just hit "enter" to choose default which is all
Then you´ll be asked if you want to remove "gcc", and here type "y" and hit enter (because default is the capital letter which is "N")<BR>
And the same as before except its "gcc-libs" instead. just do the same as before "y" -> "enter"<BR>
To the last one just hit enter (default is "Y")<BR>

#### Alsa(sound)
for sound you´ll need alasa utilities get them like so:<BR>
`sudo pacman -S alsa-utils`<BR>
then to edit the volume, mute, unmute and more do:<BR>
`alsamixer`<BR>
you control it with the arrow keys, "esc" to save and quit.<BR>

#### Xorg for desktop envoirment
To install it:<BR>
`sudo pacman -S xorg-server xorg-xinit xorg-server-utils mesa`<BR>
(do the [Nvidia] install if you have a nvidia card) and then: (only if you have nvidia)<BR>
`sudo pacman -S nvidia-xconfig`<BR>

#### Nvidia
Some Nvidia goodies<BR>
`sudo pacman -S nvidia lib32-nvidia-utils`<BR>

#### Broadcom wifi
This only needed if you have broadcom wifi card<BR>
`yaourt -S broadcom-wl`<BR>
Then reboot (`systemctl reboot`)<BR>
<BR>
`sudo systemctl enable NetworkManager`<BR>
`sudo systemctl start NetworkManager.service`<BR>

For a list of you network devices<BR>
`ip link`<BR>
You´ll get mulitiple network devices depending on your pc (ie. a laptop may have three. one for loopback another for your ethernet and last one for wireless)<BR>
You should look for one with the `state UP mode` and remeber/take note of the first couple of letter like `enp0s25` <BR>
Then to enable your wifi dhcp service for your ethernet<BR>
`systemctl enable dhcpcd@enp0s25.service`<BR>
Rember to replace "`enp0s25`" with your broadcom device<BR>

#### Desktop Envoirment: xfce4 with lxdm
install some xorg utilites needed for xfce4:<BR>
`sudo pacman -S xorg-twm xorg-xclock xterm`<BR>
then to test it:<BR>
`startx`<BR>
this should open 3 windows move mouse to each of them and type "`exit`"<BR>
Now to install lxdm and xfce4<BR>
`sudo pacman -S lxdm xfce4 xfce4-goodies`<BR>
And then enable and start the lxdm<BR>
`sudo systemctl enable --now lxdm.service`<BR>
The "`--now´" just starts the service after enabling it<BR>

#### Destop Envoirment: sddm with kde plasma
install both packages: <BR>
`sudo pacman -S plasma-desktop sddm `<BR>
Then enable and start sddm: <BR>
`sudo systemctl enable --now sddm.service`<BR>
The plasma-desktop is minimal install so it doesnt contain a terminal emulator,  install a terminal (eg."konsole")<BR>
`sudo pacman -S konsole`<BR>

#### Yakuake
install the package:<BR>
`sudo pacman -S yakuake`<BR>
Make it start at bootup:<BR>
system settings -> Startup and Shutdown -> Autostart -> Add Program -> under system -> click yakuake -> OK<BR>

#### Configure sddm
[sddm arch wiki](https://wiki.archlinux.org/index.php/SDDM)<BR>

#### Change mouse speed
look here: https://bbs.archlinux.org/viewtopic.php?id=75614<BR>

#### File decompressing and sound control and create user folders
`sudo pacman -S file-roller unrar p7zip pulseaudio pulseaudio-alsa pavucontrol xdg-user-dirs`<BR>



