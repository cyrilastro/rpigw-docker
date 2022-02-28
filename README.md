## Docker container for Game and Watch and RPi

This repository will help you to build Docker image & container ready-to-use with Game and Watch under Rapsberry Pi.
This will prevent you starting from scratch, facing building issues and save lot of time.


# Warnings for Raspberry Pi 3 users
If your testing environment run under Raspberry Pi 3, I highly recommend you 2 tips:
- Increase Swap memory to its max 2048. Refer to this [Cloud-oriented Life How-to article](https://cloudolife.com/2021/01/01/Raspberry-Pi/Resizing-or-disable-Swap-Size/) to help you.
- Decrease GPU mem to its lowest capacity to 16Mb by running below shell command. Go to "Performance Options" Menu, then "GPU memory" submenu.
> ``$ sudo raspi-config``

# Overall requirements
Before to start building your docker image, please consider following requirements and tips:
- This will build an Aarch64 arm image so I recommend you to flash your RPi with 64Bits Raspberry Pi OS, Bullseye is recommended one.
- Add your shell host user to the gpio group. If we consider "pi" is still your host username then :
> 
