# AA-30.zero-raspberry.zero
Set of instructions to assemble AA-30 with rpi0 and Waveshare 5" 1.1 firmware touch screen.

Assuming you have all needed equipment which is:
- RigExpert AA-30.zero: https://rigexpert.com/products/kits-analyzers/aa-30-zero/ with upgraded firmware
- Raspberry pi zero: https://www.raspberrypi.org/products/raspberry-pi-zero/
- Waveshare 5" touch screen (firmware 1.1): https://www.waveshare.com/wiki/5inch_HDMI_LCD_(B)_(Firmware_Rev_1.1)_User_Manual
- Logic level converter(any will do): https://propix.com.pl/pl/p/Logic-Level-Converter-4-CH/520
- HDMI ribbons will be a great addition but not crucial: https://www.aliexpress.com/store/5892117?spm=a2g0s.9042311.0.0.d15c5c0fyazg6T
- Alternatively USB-OTG + HDMI-miniHDMI is enough but takes more space
- Some powerbank and housing to finish this up

1. Create the bridge between AA-30 and RPI and connect them via UART 
```
RPI     |     LOWSIDE-CONVERTER-HIGHSIDE   |   AA-30.zero
3.3V----|---------O                        |   
5V------|---------------------------O      |
5V------|----------------------------------|------5V
14-RX---|---------O                 O------|------TX-4
15-TX---|---------O                 O------|------RX-7
GND-----|---------O                        | 
GND-----|----------------------------------|-------GND

```
2. Connect HDMI and USB OTG to Touch port at LCD. This is also enough to power it up
3. Download rpi image (I'm using 2021-03-04-raspios-buster-armhf) version with desktop should be fine. Burn it on the SD card, start and run sudo apt-get update && sudo apt-get-upgrade as usual.  
4. Change config.txt file at your raspi boot folder to this one: https://github.com/normanruta/AA-30.zero-raspberry.zero/blob/main/config.txt
Basically you need:
```
disable_overscan=1
framebuffer_width=1280
framebuffer_height=768
hdmi_group=1
hdmi_cvt 800 480 60 6 0 0 0
enable_uart=1
```
This upscaled resolution is needed by AntScope2 app (https://github.com/rigexpert/AntScope2). Otherwise you won't be able to see full window. Then just try Raspbian's appearance setting to get this as much readable as possible. The rest parameters and overclocking is up to you.

5. Prepare good power source at proceed with AA-30 instructions: https://rigexpert.com/antscope2-for-rasberry-pi-raspbian/ Start your pi from terminal, any needed file either provide with pendrive or usb-network adapter. Do not try to run MAKEs with desktop fired up it will take days! Either a way be patient at this point! If something fails at the moment of starting app try to correct permissions
6. If AA-30 runs fine proceed with Waveshare LCD installation. This one needs some introduction. Firmware version 1.1 is the ugliest implementation there is. LCD is pretty useless in most of setups. After days of research and thinkering with rpi I found this repo: https://github.com/saper-2/rpi-5inch-hdmi-touchscreen-driver It needed some tweaks 'cause it is for older raspbian image so I urge you to use my version: https://github.com/normanruta/rpi-5inch-hdmi-touchscreen-driver 
7. At the very end you need sometime to input some values so proceed with: https://pimylifeup.com/raspberry-pi-on-screen-keyboard/
8. And you are done! Enjoy!

SP5NR 73!

*This instructions has been written after assembling device and following all the procedures presented at links above. All credits goes for authors of those repositories, software and instructions. I have only compiled it for my own needs. Although my device is working as expected please be aware that some DIY skills like soldering iron and some Linux as well as programming knowledge is needed to accomplish above. Also due to changes in system, modules, packages, apps etc. there may be unpredicted behaviour. In that case feel free to contribute but keep in mind that all you do is on your own risk.
