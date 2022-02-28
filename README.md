# Docker container for Game and Watch and RPi

This repository will help you to build Docker image & container ready-to-use with Game and Watch under Rapsberry Pi.
This will prevent you starting from scratch, facing building issues and save lot of time.


## Warnings for Raspberry Pi 3 users
If your testing environment run under Raspberry Pi 3, I highly recommend you below 2 tips:
- Increase Swap memory to its max 2048Mb. Refer to this [Cloud-oriented Life How-to article](https://cloudolife.com/2021/01/01/Raspberry-Pi/Resizing-or-disable-Swap-Size/) to help you. Default 100Mb SWAP file will not be enough for compiling keystone-engine core for example.
- Decrease GPU mem to its lowest capacity to 16Mb by running below shell command. Go to "Performance Options" Menu, then "GPU memory" submenu.

`$ sudo raspi-config`

## Before to start
Before to start building your docker image, please consider following requirements and tips:
- This will build an Aarch64 arm image so I recommend you to flash your RPi with 64Bits Raspberry Pi OS, Bullseye is recommended one.  [Access to Raspberry Pi OS 64Bit Download page](https://www.raspberrypi.com/software/operating-systems/#raspberry-pi-os-64-bit)
- This guide will assume that all shell cmd will be issued by default "pi" host user. Please update with host user of your own setup.
- Add your shell host user to the gpio group. If we consider "pi" is still your host username then :

`$ usermod -aG gpio pi`

## Clone Game and Watch repos
As the docker container will be mounted with local host folder, we will first start by cloning all Game and Watch repos into local host folder owned by user host user.
Assume we create a "workdir" folder into "pi" home user space to store all repos
```
$ mkdir -p ~/workdir
$ cd ~/workdir
```
I recommend to clone for those fantastic and awesome below repos :
- [Game and Watch Backup](https://github.com/ghidraninja/game-and-watch-backup) from Ghidra Ninja
- [Game and Watch Flashloader](https://github.com/ghidraninja/game-and-watch-flashloader.git) Ghidra Ninja
- [Game and Watch Patch](https://github.com/BrianPugh/game-and-watch-patch) from Brian Pugh
- [Game and Watch Retro-go with NewUI](https://github.com/olderzeus/game-and-watch-retro-go) from olderzeus

All of above repos will help you flashing your device with custom firmware and retro-go NewUI into upgraded EXT Flash module.
**Remember to first backup your firmware and unlock your device BEFORE upgrading your 
