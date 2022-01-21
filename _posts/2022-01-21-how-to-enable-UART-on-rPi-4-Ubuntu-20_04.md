# How to enable UART on raspberry Pi for following Ubuntu distro 

It's quite tricky to enable UART on raspberry PI GPIO pins on Ubuntu Mate 20.04. 

You can do it as follows: 

1. Download uboot from github using following command: 
```
git clone https://github.com/u-boot/u-boot.git
```

2. Checkout on branch that corresponds to your Linux distro, in our case that was Ubuntu 20.04 as follows: 
```
git fetch --tags && git checkout tags/v2020.4
```

3. Install software packages for building uboot binary that's neccessary for fixing bootloader: 
```
sudo apt-get install gcc \
                     aarch64-linux-gnu-gcc \
                     bison \
                     flex
```

4. This step includes editing some of the files in uboot repository as follows: 

a) First build uboot binary that disables interrupts during autoboot

```
cd <uboot_path> && cd configs 
```

b) Append following lines on the end of `rpi_4_defconfig`
```
CONFIG_BOOTDELAY=-2
CONFIG_SILENT_CONSOLE=y
CONFIG_SYS_DEVICE_NULLDEV=y
CONFIG_SILENT_CONSOLE_UPDATE_ON_SET=y
CONFIG_SILENT_U_BOOT_ONLY=y
```

c) After that, open following file: 

```
cd include/configs 
```

d) Replace last paragraph in `rpi.h` as follows: 

```
#define CONFIG_EXTRA_ENV_SETTINGS \
    "dhcpuboot=usb start; dhcp u-boot.uimg; bootm\0" \
    "silent=1\0" \
    ENV_DEVICE_SETTINGS \
    ENV_DFU_SETTINGS \
    ENV_MEM_LAYOUT_SETTINGS \
    BOOTENV
```

e) Build uboot.bin as follows: 

```
cd <uboot_path> && make rpi_4_defconfig && make CROSS_COMPILE=aarch64-linux-gnu-
```

5. Rename uboot.bin to uboot_rpi_4.bin and copy it to SSD card in `/boot/firmware` directory. 

6. Enable UART in `syscfg.txt` by setting `enable_uart=0` to `enable_uart=1` 

** Your Raspberry PI now has UART enabled. If you want to test serial communication and
be sure that permissions are fine use this [tutorial](https://www.youtube.com/watch?v=StFZj7gSwNs). 

Have a nice day :) 
 


