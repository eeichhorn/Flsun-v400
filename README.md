# Flsun-v400

This repo contains files for the Flsun v400 3d printer.

Use the menu in the upper left for easy navigation within this page.

Flsun ships the v400 with a "custom" Klipper image that has been slightly modified for ease of use.

All of the information in this repo assumes you either have flashed to the "stock" Klipper image using the link below. (I highly recommend installing "stock" Klipper in order to unlock added features) 
...or have a least enabled ssh by flashing the Flsun speeder-pad firmware (https://flsun3d.com/pages/speeder-pad) 

# Install "stock" Klipper

The guide in the link below offers an excellent overview, and instructions for installing the "official" Klipper software on the printer, and Speeder-Pad.
See https://github.com/Guilouz/Klipper-Flsun-Speeder-Pad#restore-os-image-file
## How to "revert" to the stock KlipperScreen (After following the guide above)

In addition to installing the "stock" Klipper, I preferred the stock version of KlipperScreen so I did not use the one outlined in the guide (https://github.com/Guilouz/Klipper-Flsun-Speeder-Pad#install-official-builds-1-instance)

This way the screen is stock KlipperScreen and keeps the default color themes intact etc..
It is simple to get the Stock KlipperScreen back if you've already installed the custom software. If you are just starting the process, and want to use the stock version of KlipperScreen, simply use kiauh to install the "official" version ang ignore setting up the "custom" KlipperScreen in the guide.

If you've already gone through al of the steps in the guide you can revert to official Klipper screen by following the following steps:
Login into the speeder-pad using ssh (default password: <b>flsun</b>)
>ssh pi@speeder-pad (or IP address if hostname does not work)
>cd 
> .~/KlipperScreen/scripts/Uninstall.sh
> 
Follow the prompts, and when finished delete the KlipperScreen folder
>rm -rf ~/KlipperScreen

I also deleted the KlipperScreen.cfg and started from scratch, but you don't need too.

Now start Kiua (This is the same tool used to install Klipper, Mainsail etc.)
>~/kiauh/kiauh.sh
> - Select install (1) 
> - Select KlipperScreen (5)

Now wait for the installation to complete (This takes a few minutes)
(you mey need to enter the root password during install)

Restart
>sudo reboot now
(You may need to reconnect to eh printer from mainsail, or KlipperScreen)

Once complete, you should have the stock Klipper Screen.

I started from scratch and added the LED menu to control the LED lights from the touch panel.

To add  a web cam stream, add the following to KlipperScreen.cfg
>[printer Flsun v400]
>camera_url: http://127.0.0.1/webcam/?action=stream

For full KlipperScreen configuration details, see: https://github.com/jordanruthe/KlipperScreen/blob/master/docs/Configuration.md

# Links
## [calibration ](calibration)
Contains calibration related information including enhanced delta calibration

## [original v400 USB drive contents](original-v400-USB-files)
Contains the original files include on the USB drive included with the v400.
    
## [profiles ](slicer-profiles)
Contains modifications to the default Cura profile provided by Flsun

The current change simply includes the removal of the "hard" coded temperature values for the extruder and bed. The default settings now use the material temps for these values.

## [themes](themes)
Contains custom themes for mainsail and Klipper Screen(includes the original sidebar image for the v400)

## [models](models)
Useful models for printing


# Helpful Hints

A Handy set of information for Klipper in general: https://github.com/rootiest/zippy-klipper_config/tree/master/guides

####Fixing periodic speeder pad wifi disconnects
This could be a few things. After you've ensured that your network is not the issue, there are a few things you can try.
- Check the power saving configuration
  -  ssh into the speeder-pad
     -  >ssh pi@speeder-pad , or ssh pi@<speeder-pad ip address>
     -  >iw wlan0 get power_save
     -  You can turn it off by using
        -  >sudo iw wlan0 set power_save off   (This will not survive reboots, but is more for testing if power saving is the issue)

      -  Finding more information about the wifi interface
         -  Get the name
            -  >ls /sys/class/ieee80211/phy0/device/net
