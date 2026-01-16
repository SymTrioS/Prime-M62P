<h2 align="center">Description files</h2>

- Prime-M62P_v24_Func.pdf - board functional diagram;  
- Prime-M62P_v24_Dim.pdf  - board dimensions, connector locations, and pin numbering.  
- BOX_G1183.pdf           - type and dimensions of enclosure suitable for the Prime-M62P board,  
- BOX_G1188.pdf           - type and dimensions of enclosure suitable for the Prime-M62P board,  
- BOX_G766_G767_G768.pdf  - type and dimensions of enclosure suitable for the Prime-M62P board,  
- BOX_120x97x40.jpg       - small-sized aluminum box for Prime-M62P board;  

<h2 align="center">Linux system build procedure</h2>

**Package installation**  
$ sudo apt install build-essential  
$ sudo apt install bison flex swig git  
$ sudo apt install openssl libssl-dev libelf-dev  
$ sudo apt install libudev-dev libpci-dev libiberty-dev  
$ sudo apt install libncurses5-dev gparted python3  
$ sudo apt install dkms autoconf  

**Installing cross-compiler**  
*get file https://disk.yandex.ru/d/VCLHKDqusnvv8A/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf.tar*  
$ tar xvf gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf.tar  
$ sudo mv gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf /opt/  
$ export CC=/opt/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf-  
*availability check*  
$ ${CC}gcc -v  

**Install Python for binman u-boot olders version**  
$ wget https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz  
*or get https://disk.yandex.ru/d/VCLHKDqusnvv8A/Python-2.7.18.tgz*  
$ tar xzf Python-2.7.18.tgz  
$ cd Python-2.7.18  
$ sudo ./configure --enable-optimizations  
$ sudo make altinstall  
$ sudo ln -s "/usr/local/bin/python2.7" "/usr/bin/python"  
$ sudo ln -s "/usr/local/bin/python2.7" "/usr/bin/python2"  

**Creating a working directory**  
$ mkdir Prime-M  
$ cd Prime-M  
Prime-M$ mkdir uSD  

**U-BOOT**  
Prime-M$ git clone https://github.com/SymTrioS/u-boot-m.git -b v3s-current  
Prime-M$ cd u-boot-m  
Prime-M/u-boot-m$ make ARCH=arm CROSS_COMPILE=${CC} distclean  
Prime-M/u-boot-m$ make ARCH=arm CROSS_COMPILE=${CC} prime_m_defconfig  
Prime-M/u-boot-m$ make ARCH=arm CROSS_COMPILE=${CC} menuconfig  
*making necessary changes, Device Drivers->Network device support->ON->Allwinner Sun8i Ethernet MAC support ON , save-exit*  
Prime-M/u-boot-m$ make ARCH=arm CROSS_COMPILE=${CC} -j16  
Prime-M/u-boot-m$ cp u-boot-sunxi-with-spl.bin ../uSD/  
Prime-M/u-boot-m$ cd ..  

**KERNEL 6.6.0** (arm-linux-gnueabihf 13.3.0 cross-compiler)  
Prime-M$ git clone --depth=1 https://github.com/SymTrioS/linux-m.git -b cedrus/h264-encoding  
Prime-M$ cd linux-m  
Prime-M/linux-m$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- distclean  
Prime-M/linux-m$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- sunxi-m_defconfig  
Prime-M/linux-m$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- menuconfig  

*Device Drivers->Network device support->Ethernet driver support-> + ->STMicroelectronics devices ON*  
*------------------------------------------------STMicroelectronics Multi-Gigabit Ethernet driver ON*  
*File systems->Network File Systems->                              + NFS...NFSv4.1 ON*  

Prime-M/linux-m$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j16  
Prime-M/linux-m$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j16 INSTALL_MOD_PATH=out modules  
Prime-M/linux-m$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- -j16 INSTALL_MOD_PATH=out modules_install  
Prime-M/linux-m$ cp arch/arm/boot/dts/allwinner/sun8i-v3s-prime-m.dtb ../uSD/  
Prime-M/linux-m$ cp arch/arm/boot/zImage ../uSD/  
Prime-M/linux-m$ cd ../uSD  

**Preparing the uSD card**  
*create text file boot.cmd*  
Prime-M/uSD$ nano boot.cmd  
*add the following lines to a text file and save*  

setenv bootargs console=ttyS0,115200 root=/dev/mmcblk0p2 rootwait panic=10  
load mmc 0:1 0x43000000 ${fdtfile}  
load mmc 0:1 0x42000000 zImage  
bootz 0x42000000 - 0x43000000  

**create scr-file**  
Prime-M/uSD$ mkimage -C none -A arm -T script -d boot.cmd boot.scr  

**Preparing uSD card:**  
Prime-M/uSD$ gparted  
*Create two partitions: part1=~32M,fat16; part2=ext4*  
*Remember the letter designation of the Flash device, below it is designated as x: sdx1,sdx2*  

*Write u-boot:*  
Prime-M/uSD$ sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdx bs=1024 seek=8  

*Write Linux Kernel:*  
Prime-M/uSD$ sudo mount /dev/sdx1 /mnt  
Prime-M/uSD$ sudo cp zImage /mnt  
Prime-M/uSD$ sudo cp boot.scr /mnt  
Prime-M/uSD$ sudo cp sun8i-v3s-prime-m.dtb /mnt  
Prime-M$/uSD sync  
Prime-M/uSD$ sudo umount /dev/sdx1  

*Get https://disk.yandex.ru/d/VCLHKDqusnvv8A/debian12.rootfs-m.tar rootFS in uSD folder or use another build*  

*Write FS:*  
Prime-M/uSD$ sudo mount /dev/sdx2 /mnt  
Prime-M/uSD$ sudo tar -C /mnt/ -xf debian12.rootfs-m.tar  
Prime-M/uSD$ sync  
Prime-M/uSD$ cd ../linux-m  
Prime-M/linux-m$ sudo cp -r out/lib /mnt/usr/  
Prime-M/linux-m$ sync  
Prime-M/linux-m$ sudo umount /dev/sdx2  
