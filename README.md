# Programming examples for iCE40 FPGA
All instructions are platform specific and given for environment is Ubuntu 16.04.6 LTS. Software dependencies tested on Dec 2019.

## Hardware

Programming FPGA includes both wiring internal logic in the chip and operating IO on the evaluation board. My examples are compiled for [NandLand Go board](https://www.nandland.com/goboard/introduction.html). It is reasonably priced at $65+SH and comes with great educational lectures.

### Testing new board

Up on arrival Go board has default "program" to help you make sure it was not damaged in shipping. [Official instructions](https://youtu.be/wWMIY9kjlJ0) are given for Windows. In Ubuntu 16 or any Linux with modern kernel (4.4.0) your steps will be slightly different:

1. No need to install drivers. Just plug Go board to usb port and kernel will create serial interfaces. Go board has FT2232C chip with two serial interfaces, default demo uses second one (**ttyUSB1**).

``` bash
   $ lsusb
Bus 003 Device 006: ID 0403:6010 Future Technology Devices International, Ltd FT2232C Dual USB-UART/FIFO IC
   $ ls /dev/ttyUSB*
crw-rw---- 1 root dialout 188, 0 Dec  9 21:47 /dev/ttyUSB0
crw-rw---- 1 root dialout 188, 1 Dec  9 21:47 /dev/ttyUSB1
```

2. Install your favorite terminal emulator to work with serial interface on Go board. I used **screen**.

``` bash
   $ sudo apt install screen
   $ sudo screen /dev/ttyUSB1 115200
```

To close terminal session in screen press ^a then k

## Software

