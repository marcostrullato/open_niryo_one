# HowTo load bootloader

Use Raspberry pi to load arduino bootloader to stepper PCB. This readme based on this documents:

* [Burning Zero bootloader with Beaglebone as SWD programmer](https://petervanhoyweghen.wordpress.com/2015/10/11/burning-zero-bootloader-with-beaglebone-as-swd-programmer/)
* [Programming Microcontrollers using OpenOCD on a Raspberry Pi](https://cdn-learn.adafruit.com/downloads/pdf/programming-microcontrollers-using-openocd-on-raspberry-pi.pdf)
* [Raspberry Pi and OpenOCD](https://iosoft.blog/2019/01/28/raspberry-pi-openocd/)
----

## Compilling OpenOCD

Connect to Raspberry Pi via SSH and make sequence of comands. It can take about 15-20 min.

```bash
sudo apt-get update
sudo apt-get install git autoconf libtool make pkg-config libusb-1.0-0 libusb-1.0-0-dev telnet
git clone http://openocd.zylin.com/openocd
cd openocd-code
./bootstrap
./configure --enable-sysfsgpio --enable-bcm2835gpio
make
sudo make install
```

## Create OpenOCD config

Make folder for bootloader and OpenOCD config files

```bash
cd ~
mkdir bootloader
cd bootloader
wget https://github.com/arduino/ArduinoCore-samd/raw/master/bootloaders/zero/samd21_sam_ba.bin
sudo nano
```

Make hardware configuration file "rpi3.cfg"

```bash
# rpi2.cfg: OpenOCD interface on RPi v2+

# Use RPi GPIO pins
interface bcm2835gpio

# Base address of I/O port
bcm2835gpio_peripheral_base 0x3F000000

# Clock scaling
bcm2835gpio_speed_coeffs 146203 36

# SWD                swclk swdio
# Header pin numbers 22    18
bcm2835gpio_swd_nums 25    24

# JTAG                tck tms tdi tdo
# Header pin numbers  22  18  16  15 
bcm2835gpio_jtag_nums 25  24  23  22
```

Make OpenOCD configuration file "openocd.cfg"

```bash
source rpi3.cfg
transport select swd

set CHIPNAME at91samd21g18
source [find target/at91samdXX.cfg]

# did not yet manage to make a working setup using srst
#reset_config srst_only
reset_config srst_nogate

adapter_nsrst_delay 100
adapter_nsrst_assert_width 100

init
targets
reset halt
at91samd bootloader 0
program samd21_sam_ba.bin verify
at91samd bootloader 8192
reset
shutdown
```

## Load bootloader

Run this comand:

```bash
sudo openocd -f openocd.cfg
```

You need to have same request:

```bash
Open On-Chip Debugger 0.10.0+dev-00957-g9de7d9c8 (2019-11-20-09:00)
Licensed under GNU GPL v2
For bug reports, read
        http://openocd.org/doc/doxygen/bugs.html
Info : BCM2835 GPIO JTAG/SWD bitbang driver
Info : JTAG and SWD modes enabled
Info : clock speed 400 kHz
Info : SWD DPIDR 0x0bc11477
Info : at91samd21g18.cpu: hardware has 4 breakpoints, 2 watchpoints
Info : Listening on port 3333 for gdb connections
target halted due to debug-request, current mode: Thread 
xPSR: 0xb1000000 pc: 0xfffffffe msp: 0xfffffffc
target halted due to debug-request, current mode: Thread 
xPSR: 0xb1000000 pc: 0xfffffffe msp: 0xfffffffc
** Programming Started **
Info : SAMD MCU: SAMD21G18A (256KB Flash, 32KB RAM)
** Programming Finished **
** Verify Started **
** Verified OK **
shutdown command invoked
```