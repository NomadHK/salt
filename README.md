### SALT v1
This is a scripted record of my LFS installation. If you have a computer like mine (Dell Inspiron 7347, Core i5-4210U) and share my penchant for no-frills geeky system, you may like to try it:

Boot into Lubuntu LiveCD/USB.

Create three partitions:
* /dev/sda1, FAT32, 100MB, esp
* /dev/sda2, ext4, whatever size minus swap, root
* /dev/sda3, swap

Run the downloaded script.

The whole process takes about 7 hours to complete on my computer. Other than the base LFS programs (without Attr, Acl, Libcap, Grub, Libpipeline, Man-DB, Texinfo and any documentation), it also installs Sudo, ALSA, Dhcpcd, Wpa_supplicant, X, Wacom driver, Openbox, Gimp, Urxvt, Xfe, Xfw, Mplayer, Transmission-cli and Chrome. I've also included Wayland and Vulkan for study; if you don't want them, comment out lines 1532-1545 and remove the mention of wayland and vulkan at the end of line 1551. The programs are up-to-date (upstream) as of the first week of Oct 2016. They take up a little less than 1GB of disk space. Idle running uses less than 40MB of RAM; Chrome bumps it up to about 400MB.

The host name is compiled into the kernel as "Geronimo". If you want to change it, edit line 1068.

After boot-up, you can log into the "me" account. Type `startx` to bring up the GUI, and then right-click to bring up the programs menu.

To connect to a wireless point, first set it up by typing (as root)

`wpa_passphrase [SSID] [password] >> /etc/wpa_supplicant.conf`

and then connect by typing `dhcpcd` and disconnect by `dhcpcd -k`.

To scan for Wifi points, type (as root or sudo) `wpa_cli scan` followed by `wpa_cli scan_results`. You can use the same commands to check the signal strength of your wireless connection.

Sound volume is controlled via command `alsamixer`. If you get no sound, check /etc/asound.conf.

Chrome sometimes refuses to appear. In that case, kill the lurking chrome processes in the terminal, exit and re-enter the GUI, and try starting Chrome again. To update Chrome, [download the deb package](https://www.google.com/chrome/browser/desktop/) in Lubuntu LiveCD/USB, open it up with `dpkg-deb -x`, and then replace the chrome directory in the SALT system's /opt with the one in the deb package. Many software packages can be installed this way. For example, the Blender tarball can be extracted into /opt and run via

`LD_LIBRARY_PATH=/opt/[dir]/lib /opt/[dir]/blender`

Finally, to shutdown, type `init 0`, or to reboot, type `init 6`.

**If your computer has different parts from mine or if you don't like the way SALT is packaged**

This script can be used as a reference (in combo with [Linux From Scratch](http://www.linuxfromscratch.org/)) to write your own. You *can* build LFS into a usable desktop OS. Updates to certain softwares are as simple as replacing a directory in /opt, as the above cases of Chrome and Blender show. If you need to update or uninstall a software deeper in the dependency dough, you can edit a few lines in your build script and rebuild your system (sure it takes 7+ hours but you can let it run while you sleep; Microsoft Windows often take as long to update). I updated my script/system within an hour of reading about the Xlib vulnerability a few days ago. The Linux kernel of course as usual can be updated by compiling it and replacing the old one in /dev/sda1/efi/boot with it.
