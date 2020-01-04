---
layout: post
title: Install dotnet core in Raspberry Pi 4 
category: blog
tags: .net dotnet core Raspberry Pi 4 Open Source Debian Linux Raspbian
comments: true
---
# Raspberry Pi 
## Background
I recently bought a Raspberry Pi 4, I installed it [Raspbian Linux](https://downloads.raspberrypi.org/raspbian_lite_latest), derived from Debian and without graphical environment, on a 32 Gb micro SD card.
## Context
I recently developed a dotnet core command line application, and I needed to run it on my Raspberry Pi, using a cron to automate its execution.
In order to run the application I had to install dotnet core on Raspberry.
![_config.yml]({{ site.baseurl }}/images/raspberrypi_version.png)
Here are all the steps I performed:

# Install
## Download dotnet core for ARM
First you'll need to download the 32 bit binaries for linux ARM. My option at this time was on [dotnet core 2.2](https://dotnet.microsoft.com/download/dotnet-core/2.2).
![_config.yml]({{ site.baseurl }}/images/download_dotnet_core_arm32.png)
### to download the SDK
```bash
wget https://download.visualstudio.microsoft.com/download/pr/fca1c415-b70c-4134-8844-ea947f410aad/901a86c12be90a67ec37cd0cc59d5070/dotnet-sdk-2.2.207-linux-arm.tar.gz
```
### to dowload the Runtime
``` bash
wget https://download.visualstudio.microsoft.com/download/pr/97595553-470b-45bc-842d-aff8da46d4c4/46ee25ac85e4844df0e7f0fb9229755c/dotnet-runtime-2.2.8-linux-arm.tar.gz
```
Usually I do this with root user and inside one download folder, that then I can delete the old .tar.gz files.
### time to install it
My option is to install it inside one folder accecible by all users, in `/opt/dotnet`, because later I plan to create one technical user, with low level access to run the cron job with the app.
```bash
sudo mkdir /opt/dotnet
sudo tar zxf dotnet-runtime-2.2.8-linux-arm.tar.gz -C /opt/dotnet
sudo chmod 777
sudo tar zxf dotnet-sdk-2.2.207-linux-arm.tar.gz -C /opt/dotnet
```
Then to make sure that all users can call dotnet I run the following:
```bash
sudo ln -s /opt/dotnet/dotnet /usr/local/bin
```
## check the version
Make sure it is intalled and check the version/info about the dotnet core version installed.
```bash
dotnet --info
```
After run all those steps dotnet core 2.2 was installed and running (for all users) in my Raspberry Pi 4. My personal option was on 2.2, but if you want to install one 2.* version, maybe you should use 2.1, as it is a LTS version, otherwise you should consider 3.1, as it is the current LTS version.
![_config.yml]({{ site.baseurl }}/images/dotnet_info.png)

# next post
So probally my next post on this topic will be on how to upgrade dotnet core 2.2 to dotnet core 3.1 in Raspberry Pi (ARM 32).

# Some References:
* [Scott Hanselman post](https://www.hanselman.com/blog/InstallingTheNETCore2xSDKOnARaspberryPiAndBlinkingAnLEDWithSystemDeviceGpio.aspx/)
* [dotnet core 2.2](https://dotnet.microsoft.com/download/dotnet-core/2.2) 
* [Set permanent path on linux](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux-unix) 
* [install dotnet core on ubuntu 18.04](https://www.techrepublic.com/article/how-to-install-dotnet-core-on-ubuntu-18-04/)
* [programação raspberry pi com dotnet core (pt-br)](https://www.filipeflop.com/blog/programacao-raspberry-pi-com-net-core/)
