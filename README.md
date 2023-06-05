#versatile-arm for qemu/busybox

Compiled with kernel (stable) 6.3.5.

Toolchain: crosstool-ng

 
   PATH=${HOME}/x-tools/arm-unknown-linux-gnueabi/bin/:$PATH
   
    cd linux-stable
    if [ $? != 0 ]; then echo "ERROR"; exit; fi
   
    make ARCH=arm CROSS_COMPILE=arm-unknown-linux-gnueabi- mrproper
    if [ $? != 0 ]; then echo "ERROR"; exit; fi
  
    make ARCH=arm versatile_defconfig
    if [ $? != 0 ]; then echo "ERROR"; exit; fi

    make -j4 ARCH=arm CROSS_COMPILE=arm-unknown-linux-gnueabi- zImage
    if [ $? != 0 ]; then echo "ERROR"; exit; fi
 
    make -j4 ARCH=arm CROSS_COMPILE=arm-unknown-linux-gnueabi- modules
    if [ $? != 0 ]; then echo "ERROR"; exit; fi
 
    make ARCH=arm CROSS_COMPILE=arm-unknown-linux-gnueabi- dtbs
    if [ $? != 0 ]; then echo "ERROR"; exit; fi


sh QEMU_AUDIO_DRV=none qemu-system-arm -m 256M -nographic -M versatilepb -kernel zImage --append "console=ttyAMA0, 115200" -dtb versatile-pb.dtb

QEMU_AUDIO_DRV=none \

qemu-system-arm -m 256M -nographic -M versatilepb -kernel \ zImage

-append "console=ttyAMA0,115200" -dtb versatile-pb.dtb
