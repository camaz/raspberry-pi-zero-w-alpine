Make sure you know for sure which disk is the microSD. If you're not sure, don't follow these steps— *you could lose all the data on your Mac.*
```
diskutil list
diskutil partitionDisk disk2 MBR FAT32 VOL1 256MB "Free Space" VOL2 R

export U=http://dl-cdn.alpinelinux.org/alpine/v3.8/releases/armhf
export F=alpine-rpi-3.8.4-armhf.tar.gz
curl $U/$F -o ~/Downloads/$F
curl $U/$F.sha256 -o ~/Downloads/$F.sha256
cd ~/Downloads/
shasum -c alpine-rpi-3.8.4-armhf.tar.gz.sha256

cd /Volumes/VOL1
tar xzvf ~/Downloads/alpine-rpi-3.8.4-armhf.tar.gz

cat>usercfg.txt<<EOF
# On the Pi, the GPU and the CPU share RAM.  This is a headless install, so 
# give the GPU the least amount of RAM it can get by with (16MB).
# This also triggers the Pi to use a cutdown version of the firmware (start_cd.elf).
gpu_mem=16

# Turn off audio and bluetooth.  (Note "dt" stands for device tree.)
dtparam=audio=off,pi3-disable-bt

# Enable mini UART as serial port (/dev/ttyS0).  
# Also, fixes VideoCore IV (aka the GPU or the VPU) frequency to 250MHz.
enable_uart=1
EOF

cd ~
# unmount microSD and remove microSD card reader from the Mac
```
