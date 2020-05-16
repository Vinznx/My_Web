---
layout: post
title:  "Setup Raspberry Pi 4"
date:   2020-05-16 14:28:03 -0400
categories: Interesting-projects
author: Ningxiao Zhang
permalink: "/posts/setup-raspberry-pi-4/"
---

Raspberry Pi 4 is a tiny desktop computer (it support 4K screen). People use it on small projects. I got this for my new IPad Pro 2020. In this case, I can use my IPad to log into the Raspberry Pi and do coding or editing. This is one step for me to use IPad to replace my laptop. Here I describe how I am going to setup a ubuntu system on this "tiny computer".

1. Download the ubuntu system from <https://ubuntu.com/download/raspberry-pi> and make a system drive.

2. (Or) Install system directly on SD card. <https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#2-prepare-the-sd-card> Insert the SD card into your computer. And install the right Raspberry Pi image. And start the imager and choose Ubuntu to install.

3. Setup Wi-Fi or Ethernet. I would suggest use Ethernet which is much quicker. If you choose to use Wi-Fi, you need to edit the network-config file to add your Wi-Fi name and passwords. Remove the "#" at the beginning and edit following lines.
```css
wifis:
  wlan0:
  dhcp4: true
  optional: true
  access-points:
    <wifi network name>:
      password: "<wifi password>"
```
```css
wifis:
  wlan0:
  dhcp4: true
  optional: true
  access-points:
    "your Wi-Fi name":
      password: "12345678"
```

4. Connect romotely (still in the same local network) to the Raspberry Pi. On a Mac you can use the commend below to check the IP address.
```css
arp -na | grep -i "b8:27:eb"
```
This commend will return you Raspberry Pi IP address. and you can ssh to it remotely.

5. Install a desktop for visual use.
```css
sudo apt update
sudo apt upgrade
sudo apt install xubuntu-desktop
sudo reboot
```

6. Done
