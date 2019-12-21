# Programming examples for iCE40 FPGA
All instructions are platform specific and given for environment of Ubuntu 16.04.6 LTS. Software dependencies tested on Dec 2019. Software setup may take few days and can be done while you waiting for your FPGA board to arrive. ;)

## Hardware

Programming FPGA includes both wiring internal logic on the chip and operating IO on the evaluation board. My examples are created for [NandLand Go board](https://www.nandland.com/goboard/introduction.html). This board is reasonably priced at $65+SH and comes with great [educational lectures](https://www.nandland.com/goboard/index.html).

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

2. Install your favorite [terminal emulator](https://en.wikipedia.org/wiki/Pseudoterminal) to work with serial interface on Go board. I used **screen**.

``` bash
   $ sudo apt install screen
   $ sudo screen /dev/ttyUSB1 115200
```

To close terminal session in screen press ^a then k.



## Software

### Work flow

The work-flow of "programming" FPGA requires using of few programs in sequence steps (aka tool chain).

 1. Write Your VHDL or Verilog
 2. Simulate Your Design (optional, but recommended)
 3. Synthesize Your Design (create bit-stream file)
 4. Program the bit-stream to Go Board

Steps 1-3 are executed in the IDE or by specialized command line tools.
Step 4 is executed by **Diamond Programmer Standalone** program.

Your tool chain options:

- [iCEcube2](http://www.latticesemi.com/en/Products/DesignSoftwareAndIP/FPGAandLDS/iCEcube2) IDE by Lattice Semiconductor, the oldest and simplest one.
- [Diamond](http://www.latticesemi.com/en/Products/DesignSoftwareAndIP/FPGAandLDS/LatticeDiamond) IDE by Lattice Semiconductor, full of features for advanced users.
-  [IceStorm](http://www.clifford.at/icestorm/) cmd line open source, for seasoned warriors :)

The Lattice Semiconductor, iCE40 FPGA manufacturer, requires you to register and obtain **Node-Locked free license** with binding to your local Ethernet card MAC address. Note, software download is not available until account is "activated", may take couple days.

### iCEcube2 for Linux

1. Create user account on [Lattice website](http://www.latticesemi.com/Accounts/AccountRegister) . Confirm your email and wait until your account is activated.
2. Request [iCECube2 Software Free License](http://www.latticesemi.com/Support/Licensing/DiamondAndiCEcube2SoftwareLicensing/iceCube2), you will receive email with attached **license.dat**.
   Save it to ~/lscc/license folder. 
   Warning, Lattice software lives in stone age and checks **MAC address of eth0 only!** Before you request lic create eth0 device and use its MAC in lic request.
```bash
    $ sudo su
    # ip link add eth0 type dummy
    # ls -l /sys/class/net/eth0
lrwxrwxrwx 1 root root 0 Dec 18 22:47 /sys/class/net/eth0 -> ../../devices/virtual/net/eth0
    # ifconfig eth0
eth0      Link encap:Ethernet  HWaddr 26:b1:fc:65:5e:d9  
          inet6 addr: fe80::24b1:fcff:fe65:5ed9/64 Scope:Link
```
HWaddr 26:b1:fc:65:5e:d9 => MAC 26-b1-fc-65-5e-d9
3. download [iCEcube2](http://www.latticesemi.com/en/Products/DesignSoftwareAndIP/FPGAandLDS/iCEcube2) , current version was 2017-08 for Linux, **iCEcube2setup_Sep_12_2017_1708.tgz** .
4.  Unarchived setup program is 32-bit LSB executable. On 64 bit PC install 32bit compatibility libs.
   `$ sudo apt install libc6:i386 libsm6:i386`
5. Run setup program. When prompted point to licence.dat file.
6. Try to start IDE by the link on desktop.

### Programmer for Linux 

This tool is for programming FPGA by bit-stream file. Current version **Programmer Standalone 3.11.1 64-bit for Linux**. Did I mention Lattice software lives in stone age? Well, you have to deal with rpm. Seriously, I am glad they keep some support for Linux! Using software from other manufactures could require you to host Windows VM.



