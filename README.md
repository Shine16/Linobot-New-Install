# Linorobot Install to New RPI

<br />

#### Starting Hardware Setup

1 Using a 2.5A 5v micro usb power adapter

2 Raspberry Pi 3B Model V1.2

3 Hdmi cable to monitor 

<br />

After ubuntu mate is connected to wifi, <u>remove hdmi cable</u> and use Windows computer, Putty SSH to Rpi.

After Swapspace and Lino install, to test Platformio and teensy detection, connect <u>rpi to a teensy 3.2 by micro usb cable</u>.

<br />

<br />

##### Connecting ubuntu mate to wifi

use Desktop, select wifi, enter password

<br />

##### SSH

to find rpi address: hostname -I 

`ping -ip address- to check from windows command prompt/linux`

enable ssh in raspi-config

`sudo raspi-config`

reboot

`sudo reboot`

<br />

Enter ip address to putty at port 22 for ssh, registry key may give an error, press OK

<br />

https://www.instructables.com/id/Use-ssh-to-talk-with-your-Raspberry-Pi/

https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md

<br />

 log in with username and password of RPI

<br />

<br />

Extra:

To find in wifi list name with KeywordInWifiName with command line

`sudo wlan0 scan | grep KeywordInWifiName`		

<br />

<br />

#### Installing Linobot on rpi as a robot computer 

<br />

##### Guide to create swap space

https://www.tecmint.com/create-a-linux-swap-file/

<br />

If swap space is not created, lino make will hang at 64-74%: https://groups.google.com/forum/#!topic/linorobot/dF8q4g4_4U4

<br />

<br />

create 1.5gb of swap space for ros, 1610612736 from 1.5gb in bytes

`sudo dd if=/dev/zero of=/mnt/swapfile bs=1024 count=1610612736`

<br />

Make readable by only root user

`sudo chmod 600 /mnt/swapfile`

<br />

Setup file for swap space

`sudo mkswap /mnt/swapfile`

<br />

Edit /etc/fstab

`sudo nano /etc/fstab`

Enable swap file by adding this line in /etc/fstab

`/mnt/swapfile swap swap defaults 0 0`

<br />

Edit file to utilize memory

`sudo nano /etc/sysctl.conf`

Add following line to utilize swap file in memory

`vm.swappiness=100`

<br /><br /><br />

Commands to check on swap space

`swapon -s`

`free`

`cat /proc/swaps`

<br />

##### Install Git

`sudo apt install git`

<br />

##### Clone and install methylDragon quick install script

Go to home directory, entering command cd ~

`git clone https://github.com/methylDragon/quick-install-scripts.git`

<br />

Go to directory 

`cd ~/quick-install-scripts/Linux`

Enable permissions to all files in directory, files will turn green

`chmod +x *`

Run ros_lino_base_install

`./ros_lino_base_install`

<br />

Options while in install:

ok 			

r

2wd

rplidar

y

<br />

<br />

Turn off swap file to protect sd card

https://github.com/MrChrisJ/fullnode/issues/20

<br />

<br />

Turn off swapfile

`sudo swapoff -a`

<br />

Edit file 

`sudo nano /etc/sysctl.conf`

Add # to comment out line

`#vm.swappiness=100`

<br />

Edit /etc/fstab

`sudo nano /etc/fstab`

Comment out this line in /etc/fstab

`#/mnt/swapfile swap swap defaults 0 0`

<br />

Remove swapfile

`sudo rm /mnt/swapfile`

<br />

Reboot Rpi after removing swap space, swap space should be 0 after rebooting.

`sudo reboot`

<br />

 <br />

<br />

Hardware:

Connect teensy 3.2 with micro usb cable to Rpi.

<br />

https://github.com/linorobot/linorobot/wiki/2.-Base-Controller

<br />

Test Teensy detection by Rpi and Platformio and by uploading code. 

`roscd linorobot/teensy/firmware` 

`platformio run --target upload`

#### <br />

<br />

##### Extra:  Platformio to teensy 3.2 LED Blink test

<br />

This is to understand platformio a little better

http://docs.platformio.org/en/latest/quickstart.html

<br />

On a windows computer, through Putty SSH to rpi

<br />

Go home directory

`cd ~`

Create and go to new directory

`mkdir platformio_test`

`cd platformio_test`

<br />

Initialize platformio

`platformio init --board teensy31`

<br />

Lib, platformio.ini and src will be created

<br />

Go to src directory

`cd src`

Create main.cpp in src directory

`touch main.cpp`

Edit main.cpp

`sudo nano main.cpp`

<br />

Enter

```
#include "Arduino.h"

#ifndef LED_BUILTIN
#define LED_BUILTIN 13
#endif

void setup()
{
  // initialize LED digital pin as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop()
{
  // turn the LED on (HIGH is the voltage level)
  digitalWrite(LED_BUILTIN, HIGH);

  // wait for a second
  delay(1000);

  // turn the LED off by making the voltage LOW
  digitalWrite(LED_BUILTIN, LOW);

   // wait for a second
  delay(1000);
}
```

Save and back out to platformio_test directory<br />

`cd -`<br />

Compile and upload <br />

`platformio run --target upload`<br />

<br />

Platformio should upload, see led blink on teensy!

<br />

##### Credits

Credits to Linorobot and methylDragon for code and scripts!

https://github.com/linorobot/linorobot

https://github.com/methylDragon