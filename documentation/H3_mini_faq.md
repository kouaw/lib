# Armbian H3 mini FAQ -- 5.05

**OS images with legacy Kernel (3.4.110)**

Armbian started beginning with release 5.04 to support all available H3 based Orange Pi boards (including [One](http://forum.armbian.com/index.php/topic/724-quick-review-of-orange-pi-one/) and Lite when available).

***Changes in 5.05***

- Auto detection for the Orange Pi 2 does work now
- Mali acceleration works for all users not only root
- verbose boot logging on 1st boot and after crashes (you can toggle verbose logging using 'sudo armbianmonitor -b')
- more WiFi dongles supported due to backported firmware loader patch
- all 3 USB ports on Orange Pi One (Lite) available ([2 of them need soldering](http://forum.armbian.com/index.php/topic/755-tutorial-orange-pi-one-adding-usb-analog-audio-out-tv-out-mic-and-ir-receiver/))
- I2S possible on all Orange Pis (compare with the [mini tutorial](http://forum.armbian.com/index.php/topic/759-tutorial-i2s-on-orange-pi-h3/) since you need to tweak script.bin)
- default display resolution set to 720p60 to fix possible overscan issues on 1st boot
- HW accelerated video decoding works for most formats
- Booting from eMMC on OPi Plus now possible

***Changes in 5.04 compared to pre-releases***

- HDMI/DVI works (bug in boot.cmd settings)
- Reboot issues fixed (bug in fex settings)
- 1-Wire useable (we chose to stay compatible to loboris' images so the data pin is 37 by default. You're able to change this in the [fex file](https://github.com/igorpecovnik/lib/blob/6d995e31583e5361c758b401ea44634d406ac3da/config/orangepiplus.fex#L1284-L1286))
- changing display resolution and choosing between HDMI and DVI is now possible with the included _h3disp_ tool (should also work in the [stand-alone version](http://forum.armbian.com/index.php/topic/617-wip-orange-pi-one-support-for-the-upcoming-orange-pi-one/?p=5480) with Debian based OS images from loboris/Xunlong). Use _sudo h3disp_ in a terminal to get the idea.
- Ethernet issues fixed (combination of kernel and fex fixes)
- USB-to-SATA bridge on the Orange Pi Plus works
- stability problems on Orange Pi One fixed (due to undervoltage based on wrong fex settings)
- problems with 2 USB ports on the PC fixed (wrong kernel config)
- Mali400MP acceleration (EGL/GLES) works now
- suspend to RAM and resume by power button works now (consumption less than 0.4W without peripherals)
- Enforce user account creation before starting the GUI
- USB and Ethernet IRQs distributed nicely accross CPU cores
- Full HDMI colour-range adjustable/accessible through _h3disp_ utility
- already useable as stable headless/server board

***Known issues with 5.05***

- Playing HEVC/H.265 video with 10 bit depth not supported

***Important to know***

- 1st boot takes longer (up to 5 minutes). Please do not interrupt while the red LED is blinking, the board reboots automatically one time and the green LED starts to blink when ready
- our [User documentation](http://www.armbian.com/documentation/) (one exception currently: use _h3disp_ to adjust display settings)
- our [Geek documentation](http://www.armbian.com/using-armbian-tools/) (in case you want to build your own images)
- CPU frequency settings are 648-1200 MHz on OPi One/Lite and 480-1296 MHz on the other boards (cpufreq governor is _interactive_ therefore the board only increases CPU speed and consumption when needed)
- In case you experience instabilities, think about installing [RPi-Monitor for H3](http://forum.armbian.com/index.php/topic/617-wip-orange-pi-one-support-for-the-upcoming-orange-pi-one/?p=5076) to get an idea whether you suffer from overheating
- In case you're unsure whether to test a desktop or CLI image simply try out the GUI version since you can always get 'CLI behaviour' by running 'sudo update-rc.d -f nodm disable' later (this disables the start of X windows and desktop images behave like those made for headless use afterwards. If you're experienced you could also reclaim disk space by removing the _libxfce4util-common_ package and doing an _apt-get autoremove_ later)
- especially for desktop images the speed of your SD card matters. If possible try to use our _nand-sata-install_ script to move the rootfs away from SD card. The script also works with USB disks flawlessly ([some background information](http://forum.armbian.com/index.php/topic/793-moving-to-harddisk/))

***Areas that need testing/feedback***

- SPI
- I2C
- GPIO in general
- Wi-Fi on OPi Plus, Plus 2 and 2
- USB wireless dongles
- user experience

***What you can do to improve the situation***

- improve software support for Orange Pi One by [testing DRAM reliability](http://forum.armbian.com/index.php/topic/617-wip-orange-pi-one-support-for-the-upcoming-orange-pi-one/?p=5455)!
- get back to us with [feedback regarding our OS images](http://forum.armbian.com/index.php/topic/617-wip-orange-pi-one-support-for-the-upcoming-orange-pi-one/?view=getlastpost)
- fork our repo, fix things and send pull requests

***Known to NOT work (reliably) yet***

- Camera support. We included [@lex' patches](http://www.orangepi.org/orangepibbsen/forum.php?mod=redirect&goto=findpost&ptid=443&pid=7263) but miss [phelum's basic patches](http://www.orangepi.org/orangepibbsen/forum.php?mod=redirect&goto=findpost&ptid=70&pid=2905). Fixes welcome
- live display resolution switching. Fixes welcome (anyone willing to port the stuff from the [H3 OpenELEC port](https://github.com/jernejsk/OpenELEC-OPi2)?)
- onboard Wi-Fi (it works somehow but chip/driver are cheap and bad -- we can't do much to improve the situation)

**OS images with vanilla Kernel (4.x)**

Mainlining effort for H3 and Orange Pi's is progressing nicely but since Ethernet support still isn't ready we currently do not provide OS images with vanilla kernel (for the impatient: early patches [here](http://sunxi.montjoie.ovh/patchs_current/) and discussion [there](https://groups.google.com/forum/#!topic/linux-sunxi/ZrVjF74mliY)). Our build system is already prepared, board auto detection also works with mainline kernel so as soon as Ethernet is ready we'll release OS images (in the lab an Orange Pi PC is serving files as NAS since weeks stable with kernel 4.4)

But since we collected a bunch of [necessary H3 patches](https://github.com/igorpecovnik/lib/commit/79c7662a491b46caf07f05880403903dccc33cd1) you're already able to build your own 4.4.x image at this time. Just choose Orange Pi Plus as target or Orange Pi H3 for PC/One/2/Lite. But please remember that you end up with a rather limited image where just SMP, UART and USB is working. 

The good news: With a GbE Ethernet dongle network will be way faster on all Oranges except the Plus and since you can make use of [USB Attached SCSI](http://linux-sunxi.org/USB/UAS) with mainline kernel USB performance also increases when your drive enclosure supports UAS.

BTW: please don't expect that a driver for the onboard Wi-Fi chip of the various Oranges will ever be mainlined.
