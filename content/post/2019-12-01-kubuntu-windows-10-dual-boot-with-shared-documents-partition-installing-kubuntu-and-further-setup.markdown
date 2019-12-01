---
title: 'Kubuntu-Windows 10 dual boot with shared documents partition: installing Kubuntu
  and further setup'
author: Arjan
date: '2019-12-01'
slug: kubuntu-windows-10-dual-boot-with-shared-documents-partition-installing-kubuntu-and-further-setup
categories:
  - Kubuntu
  - Dual boot
  - Installation
  - Guide
  - Linux
tags: []
comments: no
images: ~
---

# Introduction
Starting my second postdoc position at the UMC Utrecht I decided to switch to working on GNU/Linux, or Linux in short. Various reasons come to mind, but the main reasons at that time were the superior terminal, bash scripting, and excellent support for a myriad of (bioinformatics) tools. On top of that, while getting used to it more and more, additional reasons now are the stunning desktop environment which is KDE Plasma, reliability of the operating system, the open source nature (perhaps the most important to me) and the excellent community. All this is for free. Money-wise, that is. It did cost some time to set it up, and get used to it. And even now, 15 months in, I need to look up how to do certain things for the first, third, tenth time. My terminal skills are still relatively mediocre. Luckily, the graphical interphase of - in my case - Kubuntu is outstanding, so no need to stress out over doing everything in Terminal. If you don't want to, you often don't need to.  

Originally I had my computer set up as dual boot Windows 10 / [OpenSUSE][6] 15 Leap. I chose openSUSE at that time, since it seemed the only Linux distribution that installed on my HP laptop. For some reason I couldn't get other Linux distribution like Ubuntu and Mint started on that computer, while on other systems this was no issue at all. Although openSUSE came with KDE plasma, setting things up in te beginning took me hours and hours, simply because everything was new. Additionally, there were several issues that at that time I couldn't immediately wrap my head around, like using multiple monitors (didn't seem to work out of the box), openSUSE not starting up if laptop not connected to power outlet (or crashing when plug removed from power outlet), no mounting of SD card (seems to be related to the laptop, since in Kubuntu I still have this issue, while in Windows it works fine), and 'drive is full' issues (partly resolved by removing snapshots and changing settings in snapper). Also Windows taking up so much space, while hardly being used, didn't help.  

Therefore, at the beginning of 2019, I decided to completely switch to [Kubuntu][5] and can't say I've regretted this a bit. I completely removed Windows, wiped the entire computer, and then installed Kubuntu, after fixing the issue of the live USB not starting on my laptop. This turned out to be an easy [fix][11]. Since then, I have installed Linux (Lubuntu, Raspbian, Armbian) on several of my personal machines at home. And in the end decided to set up a dual boot system on my main laptop. The reason that I wanted to keep Windows on this machine is that I sometimes use Photoshop and Illustrator, that I bought via my University, and haven't had time to look into alternatives like [Gimp][2] or [Inkscape][3].  
So, I reinstalled Windows using the Windows 10 media creation tool. The guide at [How-To Geek][1] is relatively straight-forward. Although a heads up when done installing and restarting your machine would be nice (remove the installation medium to avoid redoing the entire thing;). While re-installing Windows, I made a partition of 80GB available for it. While looking at this partition now, there's still >50GB free space, so probably could've made this smaller, although Windows notoriously takes up a lot of space with its updates etc.  
For setting up the dual boot, the guide (again) at [How-To Geek][14] is a nice starter, but a bit short, and lacking certain steps that take care of issues I ran into.  

Therefore, below, I discuss how to install Kubuntu besides Windows, and set up a shared partition for documents, downloads, music, photos, etc...  


# Assumptions:
- Windows already installed  
- Plenty of disk space for additional install of Kubuntu and a separate partition for documents and such  
- Back up made of all of your files (if you re-install Windows, too, better backup first)  


# Make a live USB
To be able to install Kubuntu, we need an installation medium. Therefore, I made a live USB containing Kubuntu on my other computer, also running Kubuntu.  
To do this, I did the following:  

## Install Etcher
I like to use [Etcher][4] to write images to USB and SD cards, so I installed it on Kubuntu. It is also available for Windows and Mac, in case you don't have a computer running Linux, yet.  

```bash
sudo apt-get update
wget https://github.com/balena-io/etcher/releases/download/v1.5.56/balena-etcher-electron_1.5.56_amd64.deb		
## if you opened Terminal and did not change directory, the etcher deb file should be in your home folder
sudo dpkg -i balena-etcher-electron_1.5.56_amd64.deb
```

While installing, I got the following output:  

> Selecting previously unselected package balena-etcher-electron.  
(Reading database ... 249018 files and directories currently installed.)  
Preparing to unpack balena-etcher-electron_1.5.56_amd64.deb ...  
Unpacking balena-etcher-electron (1.5.56) ...  
dpkg: dependency problems prevent configuration of balena-etcher-electron:  
 balena-etcher-electron depends on libpango1.0-0; however:  
  Package libpango1.0-0 is not installed.  

>dpkg: error processing package balena-etcher-electron (--install):  
 dependency problems - leaving unconfigured  
Processing triggers for hicolor-icon-theme (0.17-2) ...  
Processing triggers for desktop-file-utils (0.23-1ubuntu3.18.04.2) ...  
Processing triggers for gnome-menus (3.13.3-11ubuntu1.1) ...  
Processing triggers for mime-support (3.60ubuntu1) ...  
Errors were encountered while processing:  
 balena-etcher-electron  

The above shows us that Etcher Electron USB Writer package is asking for some dependencies to complete the installation.  
To take care of this, run the following command to install required dependencies:  

```bash
sudo apt-get install -f
```

This should complete installation of Etcher.  

Check install:  

```bash
sudo dpkg -l balena-etcher-electron
```


## Download Kubuntu
Get the latest version of [Kubuntu][5] or latest LTS version of Kubuntu  
In this case, I chose the latest version, which was Kubuntu 19.04, which is supported with security and maintenance updates until July 2020. That's a period of 9-10 months from when I installed this version of Kubuntu (somewhere in August 2019). If you want longer support, choose to install the LTS version.  

Find Kubuntu image downloads [here][7].  


## Checksums
I manually checked the checksums on another Linux machine (running Kubuntu 18.04.3 LTS).  
These were to be found via this [link][8].  
SHA256 for kubuntu-19.04-desktop-amd64.iso: 8e43da4ddba84e1e67036aac053ba32079e6fb81a28aaedae8a8e559ac1a4d3f  
MD5SUM for kubuntu-19.04-desktop-amd64.iso : 9a5cdb753ab86cd98ff426347faf9989  

So, for checking, in the Terminal go to the directory where you downloaded the Kubuntu image.  

```bash
cd download directory
```

in my case:  

```bash
cd /home/arjan/Downloads/  
```

Then run the following line:  

```bash
sha256sum filename.iso
```

in my case:  

```bash
sha256sum kubuntu-19.04-desktop-amd64.iso  
```

output:  

> 8e43da4ddba84e1e67036aac053ba32079e6fb81a28aaedae8a8e559ac1a4d3f  kubuntu-19.04-desktop-amd64.iso  


```bash
md5sum kubuntu-19.04-desktop-amd64.iso
```

output:  

> 9a5cdb753ab86cd98ff426347faf9989  kubuntu-19.04-desktop-amd64.iso  

These indeed correspond with the checksums mentioned above.  
If you want to check these in Windows, search how to do this. ;)  


## Flash USB stick
Insert USB stick with enough space - the iso is 1.8 GB.  
Start Etcher.  

1. Select Kubuntu iso, where you downloaded it  
2. Select USB stick  
3. Flash!  

Etcher needs root privilege to perform its tasks, so will ask for root pw.  
This process is the same in Linux as it is for Windows (as said above, Etcher is available for both, and Mac).  


# Install Kubuntu
Insert USB stick in computer on which Kubuntu needs to be installed.  
Make sure USB stick is booted. If it doesn't boot you may need to change boot order settings in BIOS.  

## Change boot order in BIOS
There's plenty of different possible keys to get into your computer's bios. Check for a short overview [this][15] post, under method 1.  
For my HP Spectre I had do the following:  

- Start up computer > press ESC > press F10 to open BIOS > go to 'system config' tab > boot options  
- In boot options you can see the UEFI boot order  
- Move USB hard disk / drive to the first position by selecting it and pressing F5 to move it up  
- Press F10 to save and exit  

The computer will now restart and if all is well, Kubuntu live USB will start.  

## Live environment does not start?
If the live environment does not start after all, it could be that you need to 'disable the intel graphics features by setting the "nomodeset" option before boot', as explained [here][11]. This is what I had to take care on my HP Zbook 17 G5 to get Kubuntu installed.  
In short:  

- Start up your computer with USB stick containing Kubuntu in it  
- When the GRUB bootloader screen appears, select Kubuntu, and then press 'e' to edit the commands before booting  
- In the editor that appears you can change Kubuntu parameters  
- Look for the line starting with "Linux /boot/vmlinuz ... "  
- Go to the end of the line, and type "nomodeset", in front of $vt_handoff ...  
- Then press ctrl+x, which prompts your system to boot the live environment / installer  


## Kubuntu live USB
First screen:  

1. set language  
Choose 'install Kubuntu', when asked whether you want to try or install Kubuntu  

Then:  

2. set keyboard layout  
3. connect to wireless connection  
4. choose installation mode (I chose 'normal'), and other options: I chose 'download updates while installing', and 'install third-party software for graphics etc.'  
5. disk setup: manual, since I already set it up when installing Windows
I reserved 50GB for Kubuntu. This is quite a bit, but since part of the home directory is going to be on this same partition, that's fine. No separate home partition in this case.  
As mentioned, a third partition (besides the Windows and Kubuntu partitions) will have most personal files like documents, music, movies, etc. I reserved the rest of the relatively small hard disk for this (about 120GB me thinks)  

In the end I chose to set the free space up like this:  
**Swap partition**  
Created a new partition, and filled in:  

- 8000  
- Beginning of this space  
- swap area  

**Primary partiton for Kubuntu installation**  
Created a new partition, and filled in:  

- 44429  
- Beginning of this space  
- Ext4 journalling file system  
- Mount point: /  

The left-over free space partition I left alone for now, since I want to format it as ntfs, so Windows can also access it, but couldn't find the option to do this in the Kubuntu setup.  
Result:  

> /dev/sda  
	  /dev/sda1	ntfs		554 MB  
 	  /devsda2	efi		104 MB  
 	  /dev/sda3				16 MB  
 	  /dev/sda4	ntfs		83209 MB		Windows primary partition  
  	/dev/sda5	swap	7999 MB  
  	/dev/sda6	ext4		44429 MB		Kubuntu primary partition  
  	/dev/sda7				119744 MB	Reserved for personal files; set to 'do not use this partition'  

Click install now

6. Choose timezone  
7. Set user info  
8. Install!  


# Format storage partition and label
Now, it's time to set up the shared partition that will act as storage for mutual folders and files, including documents, music, video, photos.  

- Go to the application launcher >> start typing “disks” >> and open KDE partition manager  
- Right-click the windows primary partition >> click properties >> and fill in the text box behind ‘Label:’ For instance fill in “Windows”  
- Likewise, I right-clicked the unused partition that I had >> clicked properties >> chose from the File System drop-down ntfs:  
  a pop-up comes up, which can be clicked ‘change file system’. Additionally, fill in Label, e.g. “storage”  
- Click apply (top-left corner) to apply these changes.  

In my case, the ‘storage’ partition has been changed. The Windows partition hasn’t.  
Perhaps it needs to be changed from Windows itself?  


# Disable fast startup Windows
One additional ‘feature’: make sure to disable fast startup  
If you don’t do this, you will not have write access to your nfts partitions  

On [superuser][12] this is explained, and the solution below suggested. In short:  
With fast startup enabled in Windows, Windows doesn't properly shutdown, but logs off the current user and then puts the system core in hibernation. However, this means that the drive is not safe to be modified by other operating systems like Linux until Windows is properly shutdown. To prevent mistakes from taking place, Linux mounts the Windows drive as read-only. Not doing so makes changes to the Windows partition possible, possibly resulting in Windows being unstable, crashing or data loss.  
Since you don't often need to use the Windows partition, this wouldn't be a big issue if the shared partition was not ntfs, too. Either way, fast startup needs to disabled.  

+ Log into Windows
+ Go to the start menu >> start typing ‘power’
+ Click power options >> and go to ‘Choose what the power buttons do’
+ Click ‘Change settings that are currently unavailable’ (at the top) >> then uncheck ‘Turn on fast start-up’ (bottom of page, at shut-down settings’)


# Auto-mount your storage partition
If you're going to use your storage partition to store and access your documents, etc, it is nice to immediately have access to it, and not having to manually mount it every time you start up your computer. Therefore, you need to have the system auto-mount the storage partition.  

1. Install ntfs - very likely already installed (if so, you'll get a message that it is)  

```bash
sudo apt-get install ntfs-3g
```

> Reading package lists... Done  
Building dependency tree  
Reading state information... Done   
ntfs-3g is already the newest version (1:2017.3.23AR.3-2ubuntu1).  
ntfs-3g set to manually installed.  
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.  

2. Make a directory 'storage' under /media

```bash
sudo mkdir /media/storage
```
This makes sure the partition will show up under ‘Devices’ in Dolphin   

3. Changing fstab

Make a backup of fstab

```bash
sudo cp /etc/fstab /etc/fstab.backup
```

Look up storage partition UUID

```bash
sudo blkid
```

Output:  

> /dev/sda1: LABEL="Recovery" UUID="4C7E04F87E04DD18" TYPE="ntfs" PARTLABEL="Basic data partition" PARTUUID="9cb64f7c-5213-45ba-baf0-2bc12914c80b"  
/dev/sda2: UUID="A805-F0A5" TYPE="vfat" PARTLABEL="EFI system partition" PARTUUID="4330b270-9acc-473f-a64b-017584193f98"  
/dev/sda3: PARTLABEL="Microsoft reserved partition" PARTUUID="256a7b94-824f-4026-b165-1df0c1a1740d"  
/dev/sda4: UUID="7C7013AE70136E60" TYPE="ntfs" PARTLABEL="Basic data partition" PARTUUID="8c1eaf24-7232-4b4a-95e7-ab3c4f3ed5bc"  
/dev/sda5: UUID="3c8ef5e8-ea00-481b-a432-6cce34c1055e" TYPE="swap" PARTUUID="afa0980e-3d92-44ac-bd9d-02f3290d6b4c"  
/dev/sda6: UUID="5586e9cb-3345-4780-81e8-09285576188d" TYPE="ext4" PARTUUID="051ac3cb-6bb2-4925-9c3a-82395e27b93f"  
/dev/sda7: LABEL="storage" UUID="0EA6FB8B2FA06A29" TYPE="ntfs" PTTYPE="dos" PARTUUID="25379726-db3c-4fe2-b63f-b45ef1aaf621"  

Above you can find your storage partition, and its UUID. You will need this to be able to have it auto-mounted.  
The original source I used for this used gedit to change fstab  
I don't have it installed, nor do I want to have it installed, since I never use it  

The original lines were  
gedit admin:///etc/fstab  

So, at first I replaced this with vim  
However, the command vim admin:///etc/fstab for some reason opened an empty file.  
Not sure what the admin:// adds  

To get this working simply type:

```bash
sudo vim /etc/fstab
```
This does work, opening the file fstab, of which the content looks like:  

>  /etc/fstab: static file system information.  
 Use 'blkid' to print the universally unique identifier for a  
 device; this may be used with UUID= as a more robust way to name devices  
 that works even if disks are added and removed. See fstab(5).    
<file system> <mount point>   <type>  <options>       <dump>  <pass>  
/ was on /dev/sda6 during installation  
UUID=5586e9cb-3345-4780-81e8-09285576188d /               ext4    errors=remount-ro 0       1  
/boot/efi was on /dev/sda2 during installation  
UUID=A805-F0A5  /boot/efi       vfat    umask=0077      0       1  
swap was on /dev/sda5 during installation  
UUID=3c8ef5e8-ea00-481b-a432-6cce34c1055e none            swap    sw              0       0  

Add the following lines to the bottom of fstab, substituting your own UUID instead of mine:  

+ Press insert
+ Paste:

> storage mount
UUID=0EA6FB8B2FA06A29	/media/storage/	ntfs-3g auto,user,rw 0 2

+ Press esc
+ Type :exit! or :wq! to quit and force write
+ Restart computer


# Configure Your Sub-folders (Linux)
Next, you want the folders that normally are found in your home folder to be moved to your storage folder.  

+ Make a backup of the configuration file

```bash
cp .config/user-dirs.dirs .config/user-dirs.backup
```

+ Then edit the original config file that contains folder information  

```bash
vim .config/user-dirs.dirs
```

The original looked like this:  

> XDG_DESKTOP_DIR="\$HOME/Desktop"  
XDG_DOWNLOAD_DIR="\$HOME/Downloads"   
XDG_TEMPLATES_DIR="\$HOME/Templates"  
XDG_PUBLICSHARE_DIR="\$HOME/Public"  
XDG_DOCUMENTS_DIR="\$HOME/Documents"  
XDG_MUSIC_DIR="\$HOME/Music"  
XDG_PICTURES_DIR="\$HOME/Pictures"  
XDG_VIDEOS_DIR="\$HOME/Videos"  

Change these into the entries below, replacing ‘$HOME/’ by /media/storage/  
(fill in your own storage location instead of /media/storage/, if you chose a different location than what I use here)  

> XDG_DESKTOP_DIR="\$HOME/Desktop"  
XDG_DOWNLOAD_DIR="/media/storage/Downloads"  
XDG_TEMPLATES_DIR="\$HOME/Templates"  
XDG_PUBLICSHARE_DIR="/media/storage/Public"  
XDG_DOCUMENTS_DIR="/media/storage/Documents"  
XDG_MUSIC_DIR="/media/storage/Music"  
XDG_PICTURES_DIR="/media/storage/Pictures"  
XDG_VIDEOS_DIR="/media/storage/Videos"  

+ Restart computer  


# Configure Dolphin (default KDE file manager)
In my case the storage partition was not placed under ‘Places’, but under ‘Devices’ (both in left bar in Dolphin, top and bottom, respectively).   

To move the storage folder up to 'Places', right-click ‘Places’, and click ‘Add Entry...’  
Here you can you add an entry, by setting a label, e.g. “My Files”; setting the location to link to, in my case ‘/media/storage’ and choosing an icon. If you click ‘OK’, your storage folder appears in places. If you want it on top (it’s placed at the bottom of the Places list), just drag it there.  

In places, there is also a ‘Downloads’ link, that you can redirect to your new Downloads folder. Right-click, ‘Edit’, set ‘Location’ to the proper folder (in my case ‘media/storage/Downloads’  

Lastly, when you open Dolphin, it automatically opens with the Home folder. Since your principal files are now elsewhere, you can have Dolphin open with a different folder. Click ‘Control’ in the menu bar, then ‘Configure Dolphin’ (or Ctrl+Shift+,). In the pop-up screen, click the ‘Startup’ tab, and change the text box behind "Start in:". In my case: /media/storage  


# Configure Your Sub-folders (Windows)
In File Explorer one can find the most used sub-folders under 'quick access' and under 'This PC' in the navigation pane (panel on the left).  
Now, right-click 'Documents' under 'This PC' >> click properties >> click the tab 'Location' >> click 'Find target' and browse to the folder that you want to link 'Documents' to. In my case this was now on the F: drive, called storage, in the folder 'documents'; so: F:\\Documents.  
Do the same for Downloads, Music, Pictures, and Videos  

If any of these folders appear under both 'This PC' and 'Quick access', they will both be updated if you change it in one of the locations.  

Don't forget to check the downloads folder for your favourite browser(s).  
For instance, for Firefox, click the open menu button (the hamburger icon) >> click 'options' >> scroll down to 'Files and Applications' >> and check what is filled in in the box behind 'save files to'. If not the Downloads folder on the new storage disk, click the Browse button, and link to the correct location.  


# Fix time issue in Windows
I noticed that after setting up the dual boot, Windows time is not set correct anymore, despite set to 'Set the time automatically'.  
To manually fix this for the session, click the time and date (in system tray) >> click Date and time settings >> toggle Set 'the time automatically' off and on again. Now time is correct.  

To properly fix it:  

+ Click Start >> start typing regedit.exe >> open regedit when it pops up by hitting enter
+ Navigate to HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation
+ Right click anywhere in the right-hand pane >> click New >> DWORD (32-bit) Value; name it RealTimeIsUniversal.
+ Double click on it and give it a value of 1.
+ Reboot into Kubuntu >> reboot into Windows. In the system tray time should now be correct.


# Get GRUB bootloader working
For me, after installing Kubuntu next to Windows, GRUB bootloader does not appear when restarting the computer. When booting, Windows is automatically started. To get into Kubuntu, for the time being I simply pressed ESC at start up, then F9 to get bootloader choices, and from there start Kubuntu. However, the preferred situation would be starting into a bootloader from which I can choose Kubuntu or Windows, with Kubuntu being the default (top) option.  

Apparently it has to do with HP's EFI implementation, which boils down to that the booting from Microsoft's bootloader is hardcoded, making it difficult to boot from anything else.  

I first followed the first and second answer [here][13].  
What I did is the following:  
1. Check where Windows bootloader is located  

```bash
sudo efibootmgr -v
```
This runs efibootmgr application in verbose mode, which without further arguments gives you an overview of your bootloader settings.
According to to its man page, this shows the following:  

>BootCurrent - the boot entry used to start the currently running system  
BootOrder - the boot order as would appear in the boot manager. The boot manager tries to boot the first active entry in this list. If unsuccessful, it tries the next entry, and so on.  
BootNext - the boot entry which is scheduled to be run on next boot. This supercedes BootOrder for one boot only, and is deleted by the boot manager after first use. This allows you to change the next boot behavior without changing BootOrder.  
Timeout - the time in seconds between when the boot manager appears on the screen until when it automatically chooses the startup value from BootNext or BootOrder.  
Five boot entries (0000 - 0004), along with the active/inactive flag (* means active) and the name displayed on the screen.  

When looking at the five boot entries (last item in the list above; in my case I actually have seven entries), one of them says Windows boot manager, and the number in front of it - in my case Boot0000 - is the one that is first in the BootOrder above.  
In the Windows boot manager entry you will find the location of the bootloader, where it states /File(path). Write down the path for later use. In my case it stated in \\EFI\\Microsoft\\Boot\\bootmgfw.efi  
That means that it is in /boot/efi/EFI/Microsoft/Boot/bootmgfw.efi  

Go to that directory to check whether this is indeed the case  
You will find that you do not have permission to enter /boot/efi and further  
To be able to do this, you need to get a root login shell  

\*\**root login shell intermezzo*\*\*  


```bash
$ sudo bash
```
You will see that this changes your shell from username@computername: to root@computername:  
You can do something similar by running the following, which also elevates yourself to super user.  

```bash
$ sudo -s
```
or  

```bash
$ sudo -i  
```

All these can be used to navigate through directories that need root access, and these interactive shells can be exited by typing:  

```bash
exit  
```
btw, when entering sudo -i, the interactive shell is not at the same directory as you were as a default user. Using the other two interactive shells, the current directory remains the same.  

**After having taken care of the thing that you needed root access for, don't forget to return to default (non-super) user**  

\*\**end of intermezzo*\*\*  

2. Make a backup of the Windows bootloader by copying it up one level  
In my case:  

```bash
$ sudo cp /boot/efi/EFI/Microsoft/Boot/bootmgfw.efi /boot/efi/EFI/Microsoft/bootmgfw.efi
```

3. Copy GRUB's bootloader in the place of the windows bootloader, which results in the system starting the GRUB bootloader instead of Windows bootloader.  

```bash
sudo cp /boot/efi/EFI/ubuntu/grubx64.efi /boot/efi/EFI/Microsoft/Boot/bootmgfw.efi
```


# Get Windows as option in Grub bootloader
So, then I had my Linux GRUB bootloader set as if it is the windows bootloader; this makes GRUB start up, and me being able to start up Linux.  
However, the windows bootloader is not found anymore.  

You can force boot into Windows via BIOS, like so:  

+ In the GRUB menu >> system setup  
+ In resulting BIOS >> F9 Boot drive options >> Boot from EFI file >>  
+ Pick the hard drive / partition that contains bootloader >> EFI >> Microsoft >> bootmgfw.efi  
+ Windows will start up  

However, like the above Kubuntu work-around via BIOS, this method is cumbersome, and one can actually put Windows in the GRUB menu to make it available in a more straightforward way.
For me, adding chainloader /EFI/Microsoft/bootmgfw.efi to the 40_custom in /etc/grub.d/ as explained on [Ask Ubuntu][10], did not work.  
After multiple logins it did suddenly appear in my GRUB menu, but didn't load Windows.  
Exact entry:  

> menuentry 'Windows 10' { set root=(hd0,gpt2) chainloader 	bootmgfw.efi }

After altering the file, I ran:  


```bash
grub-mkconfig -o /boot/grub.cfg
```

While doing this now, and rebooting several times, the 'Windows 10' entry did not show up for me. When I did this on a different machine, at a certain moment after days/weeks, after giving up on it, the entry suddenly appeared. However, clicking it did not start Windows.

Later on, I found the guide over [here][9], which turned out to work nicely. The gist of it is that the method above does not work for uefi disks.  
To use this method, first you will need to find the UUID of your Windows partition.  


```bash
sudo fdisk -l
```

The important bit of this command's output is the following:  

> Device         Start       End   Sectors   Size Type  
/dev/sda1       2048   1085439   1083392   529M Windows recovery environment  
/dev/sda2    1085440   1290239    204800   100M EFI System  
/dev/sda3    1290240   1323007     32768    16M Microsoft reserved  
/dev/sda4    1323008 163842047 162519040  77,5G Microsoft basic data  
/dev/sda5  163842048 179466239  15624192   7,5G Linux swap  
/dev/sda6  179466240 266242047  86775808  41,4G Linux filesystem  
/dev/sda7  266242048 500117503 233875456 111,5G Microsoft basic data  

This is your main drive, with its partitions.
From this list, choose the one that contains the EFI system.
Then run the following command:

```bash
sudo blkid /dev/sda2
```

Output:

> /dev/sda2: UUID="A805-F0A5" TYPE="vfat" PARTLABEL="EFI system partition" PARTUUID="4330b270-9acc-473f-a64b-017584193f98"

From that information, take the UUID, and use it for the following menuentry in the 40_custom file.
Note that the UUID bit is pasted behind the 'set=root' part of the code.

Then put this in the 40_custom file:  


```bash
menuentry 'Windows 10' {
    search --fs-uuid --no-floppy --set=root A805-F0A5
    chainloader (${root})/EFI/Microsoft/bootmgfw.efi
}
```

To do this:  

+ Open Konsole

```bash
cd /etc/grub.d/
ls -lh
```
Check list of files; the file you need is 40_custom

```bash
sudo vim 40_custom
```
+ Press 'ins'
+ Move cursor to end of document
+ Type the following:  


```bash
menuentry 'Windows 10' {
    search --fs-uuid --no-floppy --set=root A805-F0A5
    chainloader (${root})/EFI/Microsoft/bootmgfw.efi
}
```

+ Replace my UUID with that of your own partition
+ You can copy paste this menu entry; remember to use ctrl+shift+v for pasting in the shell
+ Press 'esc'
+ Type :exit!
+ Press enter

And although the guide on the aformentioned link states to then run: 

```bash
sudo grub-mkconfig -o /boot/grub.cfg
```
I restarted the computer a few times, without avail.
After one of the restarts I then ran the following:

```bash
sudo update-grub
```
After that and another restart, the Windows 10 entry was in the GRUB menu.  

Deleting the 40_custom entry, restarting (entry is gone from the GRUB menu), again putting the entry in 40_custom and then only running sudo update-grub immediately puts the Windows entry in the GRUB menu. So it seems to me the sudo grub-mkconfig entry is not needed, but instead the sudo update-grub does the job.  
If this doesn't work for you, try them both anyway.  

# **That's it!**  
Now you should have a Kubuntu / Windows 10 dual boot machine that auto-starts Kubuntu and shares folders between the two operating system.  





[1]: https://www.howtogeek.com/224342/how-to-clean-install-windows-10/
[2]: https://www.gimp.org/
[3]: https://inkscape.org/
[4]: https://www.balena.io/etcher/
[5]: https://kubuntu.org/
[6]: https://www.opensuse.org/
[7]: https://kubuntu.org/getkubuntu/
[8]: https://kubuntu.org/alternative-downloads
[9]: https://ihaveabackup.net/article/grub2-entry-for-windows-10-uefi
[10]: https://askubuntu.com/questions/244261/how-do-i-get-my-hp-laptop-to-boot-into-grub-from-my-new-efi-file
[11]: https://www.dell.com/support/article/nl/nl/nlbsdt1/sln306327/manual-nomodeset-kernel-boot-line-option-for-linux-booting?lang=en
[12]: https://superuser.com/questions/1166344/i-cant-write-anything-on-ntfs-drives-in-kubuntu-16-10
[13]: https://askubuntu.com/questions/244261/how-do-i-get-my-hp-laptop-to-boot-into-grub-from-my-new-efi-file
[14]: https://www.howtogeek.com/howto/35807/how-to-harmonize-your-dual-boot-setup-for-windows-and-ubuntu/
[15]: https://www.tomshardware.com/reviews/bios-keys-to-access-your-firmware,5732.html
