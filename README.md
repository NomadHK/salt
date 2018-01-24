This is the script of my (B)LFS installation on a Dell Inspiron 7347 Core i5-4210U,
using Lubuntu 17.10.1 Live as host. Note that

1. Line 3 installs necessary packages in Lubuntu as per LFS's host system requirements
   and a bit more.

2. Lines 6-12 create two partitions: a 100MB esp and a linux root partition with the
   remaining space. Lines 13-15 make the pertinent file systems in these partitions.

3. Line 1175 creates the kernel config. You can run just this line, and then place the
   generated .config into the kernel source directory, and make menuconfig to customize
   it for your system. The host name of the installed system is configured into the
   kernel as "Geronimo"; you can change it thru CONFIG_DEFAULT_HOSTNAME. Take note of
   the microcode update instruction in lines 1163-1167.

4. Lines 550-556 create the init scripts.

4. Read lines 2069-2091 for post-installation instruction.
