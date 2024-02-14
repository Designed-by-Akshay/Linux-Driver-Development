# Linux-Driver-Development
Hi. In this repository I will be publishing files related to Driver development using the linux kernel 

## Download the kernel headers for the local piOS
```
sudo apt install raspberrypi-kernel-headers
```

## Confirm the version of piOS on your own Raspberry Pi
```
$ uname -a
Linux raspberrypi 5.15.32-v8+ #1538 SMP PREEMPT Thu Mar 31 19:40:39 BST 2022 aarch64 GNU/Linux
```


## Common errors
### Missing build directory
```
make[1]: *** /lib/modules/[version]/build: No such file or directory. Stop.
```
The cmd `sudo apt install raspberrypi-kernel-headers` will simply install the latest kernel headers available on the mirror. Which indeed matches the latest available kernel on the mirror. But not necessarily the installed kernel on the system.

The Raspbian maintainers always remove the older kernel headers from the repository index files. Not sure why they do that.

Solutions：

#### 1. Upgrade piOS, and build again.
`sudo apt update`
`sudo apt full-upgrade`
`sudo apt install raspberrypi-kernel raspberrypi-kernel-headers`

Specifically, for the Raspberry Pi 4 series, 32-bit PiOS will automatically switch to 64-bit mode after upgrading to the latest version. However, the raspberrypi-kernel-headers package is missing the build directory for the v8+ mode.

For example:
```
$ uname -a
Linux raspberrypi 6.1.21-v8+ #1642 SMP PREEMPT Mon Apr 3 17:24:16 BST 2023 aarch64 GNU/Linux
```
There is no build directory under `/lib/modules/6.1.21-v8+/`.

If this happens, you can add `arm_64bit=0` to the `/boot/config.txt` file and then restart the Raspberry Pi to switch back to 32-bit mode.

#### 2. Install specific version kernel headers.

Download the deb package for the version of piOS you are currently using from this link and install it.
https://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/

For tag name, please determine according to the local piOS version and raspberrypi OS tags.

https://github.com/raspberrypi/linux/tags

#### Use rpi-source
Use rpi-source from https://github.com/RPi-Distro/rpi-source it sets up everything you need to build your own kernel (from the current running kernel by default).


# References

https://www.raspberrypi.org/documentation/linux/kernel/building.md
