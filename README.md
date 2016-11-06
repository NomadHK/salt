### SALT1
This is a scripted record of my LFS installation. If you have a computer like mine (Dell Inspiron 7347, Core i5-4210U) and share my penchant for no-frills geeky system, you may like to try it:

Boot into Lubuntu LiveCD/USB. Connect to the internet. Download the build script, the kernel .config, and the wget list.

Create two partitions:
* /dev/sda1, FAT32, 100MB, to be mounted as esp
* /dev/sda2, ext4, to be mounted as root

The host name is compiled into the kernel as "Geronimo". If you want to change it, edit line 71 in .config. Change the timezone at line 345 in the build script. You can check the timezone directory names in the LiveCD's /usr/share/zoneinfo.

Run build.

The installation can be tripped by an unsuccessful download in the wget command at line 10. After this command is done (you see the terminal output moving again after a long pause during the download), a log file is placed in ~/Documents/wgetlog. Check it to see that the number of downloaded packages matches the number of lines in the list file — if they don't match, either because of bad connection or dead repository, stop the script and retry later. Also check to see that the directories linux-firmware and xf86-input-libinput are present in /mnt/sources and not empty when the script starts to compile stuff (terminal messages scrolling by furiously).

The whole process takes about 8 hours to complete on my computer. Other than the base LFS programs (without Attr, Acl, Libcap, Grub, Libpipeline, Man-DB, Texinfo and any documentation), it also installs Sudo, ALSA, Dhcpcd, Wpa_supplicant, X, Wacom driver, Openbox, Gimp, Inkscape, Unrar, Unzip, Urxvt, Xfe (with Xfw), Mplayer, Transmission-cli and Chrome.

After boot-up, you can log into the "me" account. Type `startx` to bring up the GUI, and then right-click to bring up the programs menu. Alt+Tab to bring a window to the front.

To adjust screen brightness, go into /sys/class/backlight/intel_backlight and echo (as root, not sudo) a number into "brightness". This number must be less than the one stipulated in "max_brightness". To make the change permanent, edit the corresponding line in /etc/init.d/setup.

To connect to a wireless point, first set it up by typing (as root)

`wpa_passphrase [SSID] [password] >> /etc/wpa_supplicant.conf`

and then connect by typing (as root or sudo) `dhcpcd` and disconnect by `dhcpcd -k`.

To scan for Wifi points, type (as root or sudo) `wpa_cli scan` followed by `wpa_cli scan_results`. You can use the same commands to check the signal strength of your wireless connection.

Sound volume is controlled via command `alsamixer`. If you get no sound, check /etc/asound.conf.

Chrome sometimes refuses to appear. In that case, kill the lurking chrome processes and try starting it again. To update Chrome, [download its deb package](https://www.google.com/chrome/browser/desktop/) in Lubuntu LiveCD/USB, open it up with `dpkg-deb -x`, and then replace the chrome directory in the SALT system's /opt with the one in the deb package. Many software packages can be installed this way. For example, [Blender](https://www.blender.org/)'s tarball can be extracted into /opt and run via (or added into /etc/xdg/openbox/menu.xml and run through the right-click menu):

`LD_LIBRARY_PATH=/opt/[dir]/lib /opt/[dir]/blender`

Finally, to shutdown, type (as root or sudo) `init 0`, or to reboot, type `init 6`.

**If your computer is different from mine or you don't like how SALT is packaged**

Note the focus of this script is building a particular simple flavor of (B)LFS, not automation, nor how you should partition your system, etc. Thus the script procedure isn't quite robust — I omit error checkings, stop-and-resume points and so on to avoid obfuscating the main steps, so that you can easily find out how to set up wireless connection for example. So...

Use this script as a reference (in combo with [Linux From Scratch](http://www.linuxfromscratch.org/)) to write your own. You *can* build LFS into a usable desktop OS. Updating certain softwares is as simple as replacing a directory in /opt, as the above examples of Chrome and Blender show. If you need to update or uninstall a software deeper in the dependency dough, you can edit a few lines in your build script and rebuild your system. Sure, it's going to take 7+ hours, but you can let it run while you sleep; Microsoft Windows often take as long to update.

Consult lines 1247-1269 in the build script on how to install fonts.
