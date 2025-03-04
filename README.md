# Flsun-v400

This repo contains files for the Flsun v400 3d printer.

Use the menu in the upper left for easy navigation within this page.

Flsun ships the v400 with a "custom" Klipper image that has been slightly modified for ease of use.

All of the information in this repo assumes you either have flashed to the "official" Klipper image using the link below. (I highly recommend installing "official" Klipper in order to unlock added features) 
...or have a least enabled ssh by flashing the Flsun speeder-pad firmware (https://flsun3d.com/pages/speeder-pad) or using this guide to "reset" the password https://github.com/amoutiers/FLSun-Speeder-Pad-root-password.

NOTE: If you flash the klipper speeder-pad firmware, you will also need to get the v400 configs and load them.

# Install "official" Klipper

The guide in the link below offers an excellent overview, and instructions for installing the "official" Klipper software on the printer MCU, and Speeder-Pad.
See https://github.com/Guilouz/Klipper-Flsun-Speeder-Pad/wiki/Update-dependencies

For a more advanced use of using the v400 with official Klipper builds:
See https://github.com/danorder/OEM-VANILLA-V400-Klipper-

## How to "revert" to the official KlipperScreen version (After following the guide above)

In addition to installing the "official" Klipper, I preferred the official version of KlipperScreen so I did not use the one outlined in the guide (https://github.com/Guilouz/Klipper-Flsun-Speeder-Pad#install-official-builds-1-instance)

You do loose the added calibration menu included with the fork, so keep that i mind. Since I use official Klipper calibration docs which use the gcode console, it didn't really effect me.

It is simple to get the official KlipperScreen back if you've already installed the custom version. If you are just starting the process, and want to use the official version of KlipperScreen, simply use kiauh to install the "official" version and ignore setting up the "custom" KlipperScreen in the guide.

If you've already gone through all of the steps in the guide you can revert to official Klipper screen by following the following steps:
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

Wait for the installation to complete (This takes a few minutes)
(you mey need to enter the root password during install)

Restart
>sudo reboot now
(You may need to reconnect to eh printer from mainsail, or KlipperScreen)

Once complete, you should have the official Klipper Screen version.

I started from scratch and added the LED menu to control the LED lights from the touch panel.

To add  a web cam stream right to your KlipperScreen, add the following to KlipperScreen.cfg
>[printer Flsun v400]
>camera_url: http://127.0.0.1/webcam/?action=stream

For full KlipperScreen configuration details, see: https://github.com/jordanruthe/KlipperScreen/blob/master/docs/Configuration.md

# Links
## [calibration ](calibration)
Contains calibration related information including enhanced delta calibration

## [configs](configs)
Contains Flsun v400 config examples. Both Flsun Klipper, and Official Klipper config examples are included

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
#### Exclude Object during a print
Add the ability to exclude objects in a multipart print
Klipper supports the ability to exclude object in a multipart print. THis can be helpful if part of your print has failed, but you want to continue printing the rest.
The process is simple:
Add and empty section to you printer.cfg using the web interface (mainsail ui)
```
[exclude_object]
```

Then in "moonraker.cfg" add the following
```
[file_manager]
enable_object_processing: True
```

See https://docs.mainsail.xyz/overview/features/exclude-objects
For a more detailed explanation see https://www.klipper3d.org/Exclude_Object.html

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
