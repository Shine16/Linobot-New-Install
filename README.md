# Linobot New Install



#### Starting Hardware Setup

1 Using a 2.5A 5v micro usb power adapter

2 Raspberry Pi 3B Model V1.2

3 Hdmi cable to monitor 



After connected ubuntu mate to wifi, <u>remove hdmi cable</u> and use Windows computer, Putty SSH to Rpi.

After Swapspace and Lino install, to test Platformio and teensy detection, connect <u>rpi to a teensy 3.2 by micro usb cable</u>.





##### Connecting ubuntu mate to wifi

use Desktop, select wifi, enter password



##### SSH

to find rpi address: hostname -I 

`ping -ip address- to check from windows command prompt/linux`

enable ssh in raspi-config

`sudo raspi-config`

reboot



enter ip address to putty at port 22 for ssh, registry key will be wrong, press OK



https://www.instructables.com/id/Use-ssh-to-talk-with-your-Raspberry-Pi/

https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md



 log in with username and password of RPI





Extra:

To find in wifi list name with KeywordInWifiName with command line

`sudo wlan0 scan | grep KeywordInWifiName`		





#### Installing Linobot on rpi as a robot computer 



##### guide to create swap space

https://www.tecmint.com/create-a-linux-swap-file/



If swap space is not created, lino make will hang at 64-74%: https://groups.google.com/forum/#!topic/linorobot/dF8q4g4_4U4





create 1.5gb of swap space for ros, 1610612736 from 1.5gb in bytes

`sudo dd if=/dev/zero of=/mnt/swapfile bs=1024 count=1610612736`



make readable by only root user

`sudo chmod 600 /mnt/swapfile`



setup file for swap space

`sudo mkswap /mnt/swapfile`



edit /etc/fstab

`sudo nano /etc/fstab`

enable swap file by adding this line in /etc/fstab

`/mnt/swapfile swap swap defaults 0 0`



edit file to utilize memory

`sudo nano /etc/sysctl.conf`

add following line to utilize swap file in memory

`vm.swappiness=100`





commands to check on swap space

`swapon -s`

`free`

`cat /proc/swaps`



##### Install Git

`sudo apt install git`



##### Clone and install methylDragon quick install script

go to home directory, entering command cd ~

type in:

`git clone https://github.com/methylDragon/quick-install-scripts.git`



go to directory 

`cd ~/quick-install-scripts/Linux`

enable permissions to all files in directory, files will turn green

`chmod +x *`

run ros_lino_base_install

`./ros_lino_base_install`



options while in install:

ok

r

2wd

rplidar

y





turn off swap file to protect sd card

https://github.com/MrChrisJ/fullnode/issues/20





turn off swapfile

`sudo swapoff -a`



edit file 

`sudo nano /etc/sysctl.conf`

add # to comment out line

`#vm.swappiness=100`



edit /etc/fstab

`sudo nano /etc/fstab`

comment out this line in /etc/fstab

`#/mnt/swapfile swap swap defaults 0 0`



remove swapfile

`sudo rm /mnt/swapfile`



Reboot Rpi after removing swap space, swap space should be 0 after rebooting.

`sudo reboot`



 



Hardware:

Connect teensy 3.2 with micro usb cable to Rpi.



https://github.com/linorobot/linorobot/wiki/2.-Base-Controller



Test Teensy detection by Rpi and Platformio and by uploading code. 

`roscd linorobot/teensy/firmware` 

`platformio run --target upload`

#### 



##### Extra:  Platformio to teensy 3.2 LED Blink test



This is to understand platformio a little better

http://docs.platformio.org/en/latest/quickstart.html



On a windows computer, through Putty SSH to rpi



go home directory

`cd ~`

create and go to new directory

`mkdir platformio_test`

`cd platformio_test`



initialize platformio

`platformio init --board teensy31`



lib, platformio.ini and src will be created



go to src directory

`cd src`

create main.cpp in src directory

`touch main.cpp`

edit main.cpp

`sudo nano main.cpp`



enter

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

back out to platformio_test directory

`cd -`

compile and upload

`platformio run --target upload`



platformio should upload, see led blink on teensy!



