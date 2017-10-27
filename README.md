# DIY Raspberry Pi Zero W Bluetooth Radio

I'm documenting the steps to setup a **Raspberry Pi Zero W** to stream any internet radio station over bluetooth. I couldn't find all the proper steps at a single place - hope it helps you.

This is a headless bluetooth audio setup, which is more difficult than just using the GUI.

1. Download and flash [Raspbian Stretch Lite](https://www.raspberrypi.org/downloads/raspbian/) to an SD card (using [Etcher](https://etcher.io/)).

2. Open the SD card on your computer.
    - Create a file named **ssh** in the boot partition to allow login over SSH
    - Open config.txt and append: **dtoverlay=dwc2**
    - Open cmdline.txt and add after *rootwait*: **modules-load=dwc2,g_ether**
    
3. Insert the SD card and plug in the PI to your laptop, and wait for it to boot.
4. SSH into the PI: ssh pi@raspberrypi.local
5. Setup wifi configuration in /etc/wpa_supplicant/wpa_supplicant.conf
    ````
    network={
        ssid="Network1"
        psk="passwordhere"
        priority=1
        id_str="one"
    }

    network={
        ssid="Network2"
        psk="passwordhere"
        priority=2
        id_str="two"
    }
    ````
6. *sudo wpa_cli reconfigure*. Use *sudo reboot* if you can't see connected IP in *ifconfig wlan0*
7. Install Pulse Audio: sudo apt-get install pulseaudio pulseaudio-module-bluetooth
8. Configuration: sudo nano /etc/bluetooth/main.conf
    - Add **Enable=Source,Sink,Media,Socket** after *[General]*
9. Configuration: sudo nano /etc/pulse/daemon.conf
    - exit-idle-time = 10800
10. sudo pulseaudio --start

11. sudo bluetoothctl
    - scan on
    - pair XX:XX:XX:XX:XX:XX
    - connect XX:XX:XX:XX:XX:XX

11. sudo pacmd list-sinks
    sudo pacmd set-default-sink bluez_sink.XX_XX_XX_XX_XX_XX.a2dp_sink
 
12. sudo apt-get install mplayer
13. sudo mplayer http://37.187.114.43:8000/stream

## Debug Commands
`sudo service bluetooth status`
`alsamixer`

This is just an outline. Please feel free to contribute or to open an issue if you're facing issues.

Note: We shouldn't be using `sudo` as much as we did, but I couldn't spend time to fix this.

