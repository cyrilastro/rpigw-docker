# Docker container for Game and Watch and RPi

This repository will help you to build Docker image & container ready-to-use with Game and Watch under Rapsberry Pi.
This will prevent you starting from scratch, facing building issues and save you lot of time.


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
I recommend to clone for those fantastic and awesome below repos :
- [Game and Watch Backup](https://github.com/ghidraninja/game-and-watch-backup) from Ghidra Ninja
- [Game and Watch Flashloader](https://github.com/ghidraninja/game-and-watch-flashloader.git) from Ghidra Ninja
- [Game and Watch Patch](https://github.com/BrianPugh/game-and-watch-patch) from Brian Pugh
- [Game and Watch Retro-go with NewUI](https://github.com/olderzeus/game-and-watch-retro-go) from olderzeus

All of above repos will help you flashing your device with custom firmware and retro-go NewUI into upgraded EXT Flash module.
**Remember to first backup your firmware and unlock your device BEFORE upgrading your storage** [Take a look at this reddit Wiki for choosing the right mem chip](https://www.reddit.com/r/GameAndWatchMods/wiki/flash-upgrade)

Assume we create a "workdir" folder into "pi" home user space to store all repos
```
$ mkdir -p ~/workdir
$ cd ~/workdir
$ git clone --recurse-submodules https://github.com/olderzeus/game-and-watch-retro-go
$ git clone https://github.com/ghidraninja/game-and-watch-backup.git
$ git clone https://github.com/ghidraninja/game-and-watch-flashloader.git
$ git clone https://github.com/BrianPugh/game-and-watch-patch
```
## Building Docker image
Let's go back into your user home folder and clone this repo.
```
$ cd ~/workdir
$ git clone https://github.com/cyrilastro/rpigw-docker
$ cd rpigw-docker/
```
Dockerfile will create "user" with exactly same UID & GID as your local host user. Really helpful to prevent some read/write file access right issues.
To properly tag your Docker Image with your needs, please consider to change **#YOUR_DOCKER_REPO and #YOUR_IMAGE_NAME** as you wish in the build command below.
```
$ docker build --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) -t #YOUR_DOCKER_REPO/#YOUR_IMAGE_NAME:latest .
```
***Building process under Raspberry Pi 3 could require up to 45min to complete !***

## Start, log into your container and final building step
Let's create container based on your newly created docker image.
Change **#CONTAINER_NAME** with the desired one for your container name in the run cmd below.
Your local host workdir folder will be mounted into /opt/workdir/ container folder.
```
$ docker container run -d -it --privileged --cap-add SYS_RAWIO \
--name #CONTAINER_NAME \
-v ~/workdir/:/opt/workdir \
#YOUR_DOCKER_REPO/#YOUR_IMAGE_NAME:latest bash
```

Now let's connect to container console. You will be logged as root user and starting into /home/user folder.

`$ docker exec -it #CONTAINER_NAME bash`


**!!!! ALL Next command instructions to come after this point have to be run INTO your Docker Container console !!!!**


A shell script called ***build_env.sh*** is offered to you into /home/user container folder.
This script will enter into flashloader and retro-go cloned git repos and will proceed for final building steps.
Let's run for it
```
$ cd /home/user
$ ./build_env.sh
```

## Next ?

Your docker container is now ready to use with previously listed awesome Game and Watch github repos.
If you decide to start with a stock Game and Watch still locked, steps to follow are :
- Use Game and Watch Backup 5 scripts to backup, unlock and restore your device.
- Recommend to physically upgrade your EXT Flash.
- Use Game and Watch Patch repo to create custom firmware for Dual Boot.
- Use Game and Watch Retro-Go NewUI repo.

Shell command to use for uploading your custom firmware with Game and Watch Patch repo onto a 64Mb EXT Flash upgraded device. Consider to update arg ***--device=*** with your device type, mario or zelda.
```
$ cd /opt/workdir/game-and-watch-patch
$ make clean
$ make PATCH_PARAMS="--device=mario" LARGE_FLASH=1 flash_patched
```

Last thing ! Shell command to use for flashing retro-go NewUI with optional JPEG cover feature activated, still for a 64Mb EXT Flash upgraded device.
```
$ cd /opt/workdir/game-and-watch-retro-go
$ make clean
$ make -j$(nproc) EXTFLASH_SIZE_MB=60 EXTFLASH_OFFSET=4194304 COVERFLOW=1 JPG_QUALITY=90 INTFLASH_BANK=2 flash
```

## Thank you
I decided to create this repo to help community using RPi 3 with Game and Watch.
Even if I strictly have followed all Wiki, I faced many compiling issues and spent many hours to debug and fix all of its.
I hope you will enjoy this sharing and if you wish to reward it, you can consider making BTC or ETH donation :grinning: :+1:

My BTC wallet address :point_right: 1Jc5YFCnDwGh7ovAYHHXUbCDCi5UfPfuz8

My ETH wallet address :point_right: 0xa69eb2b19e9c780ba68f6fe4f845829fa817837b

Thanks a lot for your recognition !
