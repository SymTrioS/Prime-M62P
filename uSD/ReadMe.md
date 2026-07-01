
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

*Get https://disk.yandex.ru/d/AGH8wzkAhWugOQ/debian12.rootfs.tar rootFS in uSD folder or use another build*  

*Write FS:*  
Prime-M62P/uSD$ sudo mount /dev/sdx2 /mnt  
Prime-M62P/uSD$ sudo tar -C /mnt/ -xf debian12.rootfs.tar  
Prime-M62P/uSD$ sync  

*To set up the network, copy the interfaces file, having previously edited it.*  
Prime-M62P/uSD$ sudo cp interfaces /mnt/etc/network/

*If necessary, create a network folder and define access to it.*  
Prime-M62P/uSD$ sudo mkdir /mnt/srv/work  
*Add the appropriate line to the file /etc/exports, for example:*  
Prime-M62P/uSD$ sudo nano /mnt/etc/exports  
/srv/work 192.168.3.0/24(rw,sync,no_subtree_check,no_root_squash)  
^O^X  

*Unpack out.zip Linux drivers library in uSD folder*  

Prime-M62P/uSD$ sudo cp -r out/lib /mnt/usr/  
Prime-M62P/uSD$ sync  
Prime-M62P/uSD$ sudo umount /dev/sdx2  
