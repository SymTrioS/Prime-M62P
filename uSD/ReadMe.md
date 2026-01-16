
## Preparing a uSD card:  

Prime-M62P/uSD$ gparted  
*Create two partitions: part1=~32M,fat16; part2=ext4*  
*Remember the letter designation of the Flash device, below it is designated as x: sdx1,sdx2*  

*Write u-boot:*  
Prime-M62P/uSD$ sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdx bs=1024 seek=8  

*Write Linux Kernel:*  
Prime-M62P/uSD$ sudo mount /dev/sdx1 /mnt  
Prime-M62P/uSD$ sudo cp zImage /mnt  
Prime-M62P/uSD$ sudo cp boot.scr /mnt  
Prime-M62P/uSD$ sudo cp sun8i-v3s-prime-m.dtb /mnt  
Prime-M62P$/uSD sync  
Prime-M62P/uSD$ sudo umount /dev/sdx1  

*Get https://disk.yandex.ru/d/VCLHKDqusnvv8A/debian12.rootfs-m.tar rootFS in uSD folder or use another build*  

*Write FS:*  
Prime-M62P/uSD$ sudo mount /dev/sdx2 /mnt  
Prime-M62P/uSD$ sudo tar -C /mnt/ -xf debian12.rootfs-m.tar  
Prime-M62P/uSD$ sync  

*To set up the network, copy the interfaces file, having previously edited it.*  
Prime-M62P/uSD$ sudo cp interfaces /mnt/etc/network/

*Get https://disk.yandex.ru/d/VCLHKDqusnvv8A/linux-m.zip Linux drivers library and unzip in uSD folder*  

Prime-M62P/uSD$ cd linux-m  
Prime-M62P/uSD/linux-m$ sudo cp -r out/lib /mnt/usr/  
Prime-M62P/uSD/linux-m$ sync  
Prime-M62P/uSD/linux-m$ sudo umount /dev/sdx2  
