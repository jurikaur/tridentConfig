#!/bin/bash
sudo service klipper stop
cd ~/klipper
git pull

#update via USB
#make clean KCONFIG_CONFIG=config.octopus
#make menuconfig KCONFIG_CONFIG=config.octopus
#make KCONFIG_CONFIG=config.octopus
#read -p "Octopus firmware built, please check above for any errors. Press [Enter] to continue flashing, or [Ctrl+C] to abort"
#./scripts/flash-sdcard.sh /dev/ttyACM0 btt-octopus-f446-v1.1
#read -p "Octopus firmware flashed, please check above for any errors. Press [Enter] to continue, or [Ctrl+C] to abort"

##Update via CanBoot
python3 ~/CanBoot/scripts/flash_can.py -i can0 -u de6599069f13 -r

make clean KCONFIG_CONFIG=config.octopus
#make menuconfig KCONFIG_CONFIG=config.octopus
make KCONFIG_CONFIG=config.octopus
mv ~/klipper/out/klipper.bin ~/firmware/octopus_1.1_klipper.bin

read -p "Octopus firmware built, please check above for any errors. Press [Enter] to continue flashing, or [Ctrl+C] to abort"

python3 ~/CanBoot/scripts/flash_can.py -f ~/firmware/octopus_1.1_klipper.bin -d /dev/serial/by-id/usb-CanBoot_stm32f446xx_140048000450335331383520-if00

read -p "Octopus firmware flashed, please check above for any errors. Press [Enter] to continue, or [Ctrl+C] to abort"


#make clean KCONFIG_CONFIG=config.rpi
#make menuconfig KCONFIG_CONFIG=config.rpi
#make KCONFIG_CONFIG=config.rpi
#read -p "RPi firmware built, please check above for any errors. Press [Enter] to continue flashing, or [Ctrl+C] to abort"
#make flash KCONFIG_CONFIG=config.rpi

sudo service klipper start
