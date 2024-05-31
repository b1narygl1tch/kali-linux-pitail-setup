# Raspberry Pi-Tail Zero 2 W setup

Despite the setup process is pretty easy, it requires some problem solving which takes time. Additionally, the official guide https://www.kali.org/docs/arm/raspberry-pi-zero-2-w-pi-tail/ is outdated, so I decided to make my own. \
\
Refer to https://github.com/Re4son/RPi-Tweaks/blob/master/pi-tail/Pi-Tail.HOWTO for some general tips. But keep in mind that the values may vary.

### 📱 Devices
* Raspberry Pi Zero 2 W
* microSD card (32 GB and higher)
* Micro USB cable to power Raspberry Pi
* OTG adapter and cable to connect to a smartphone
* Android smartphone
* [optional] Laptop/PC (with Linux preferably)

### 💾 Software
* Kali Linux ARM image https://www.kali.org/get-kali/#kali-arm I used `Raspberry Pi Zero 2 W (PiTail)`
* Your favourite tool to write an image to microSD card. Both, [Rufus](https://rufus.ie/en/) and [Raspberry Pi Imager](https://www.raspberrypi.com/software/) work well.
* Smartphone applications:
  * Any SSH client. For example, [ConnectBot](https://play.google.com/store/apps/details?id=org.connectbot), [JuiceSSH](https://play.google.com/store/apps/details?id=com.sonelli.juicessh), [Termux](https://play.google.com/store/apps/details?id=com.termux) with SSH installed.
  * [optional] VNC client. For example, [RealVNC](https://play.google.com/store/apps/details?id=com.realvnc.viewer.android)
  * [optional] [Hacker's Keyboard](https://play.google.com/store/apps/details?id=org.pocketworkstation.pckeyboard)

 ### 👾 Installation and setup
1. Download Kali Linux ARM image and write it to microSD card
2. Enable WiFi hotspot on the smartphone and connect to it from a laptop. Say, we have a hotspot *HomeWiFi* and *R4t4m4h4tt4* as a password.
3. On the laptop execute `ip a` to get assigned IP address and IP mask. For example, `192.168.123.x/24` (or `255.255.255.0` as IP mask)
4. On the laptop execute `route -v` to get Gateway IP address. For example, `192.168.123.173`
5. Go to `BOOT` partition on the microSD card and edit `interfaces` file:
    * Rename `iface sepultura inet static` to `iface HomeWiFi inet static`
    * Change IP address from `192.168.43.254` to `192.168.123.254`
    * Change gateway IP address from `192.168.43.1` to `192.168.123.173`
    * [optional] Remove entries for `mobile-1` and `mobile-2` interfaces.
6. [optional] If you want to keep the hotspot password in secret, use the command `wpa_passphrase <SSID> <PASSWORD>`. In our case the command will be `wpa_passphrase HomeWiFi R4t4m4h4tt4`. This will generate a config. DON'T FORGET TO REMOVE PLAIN TEXT PASSWORD!
```
network={
	ssid="HomeWiFi"
	psk=5c69230fd516a10840baf62eff26449e7edd6b821b5bdec86ca2086457674ca4
}
``` 
7. At `BOOT` partition find and edit `wpa_supplicant.conf` file:
    * Add `id_str="HomeWiFi"` string to the config and write it to the file. The larger number in `priority=` parameter, the higher the priority.
    * [optional] Remove the other entries in the file.
8. Insert microSD card to Raspberry Pi. Plugin power cable to the Raspberry Pi. The first boot may take some time (~10 min)
9. [optional] You can connect Raspberry Pi directly to smartphone via OTG adapter and micro-USB cable. \
OTG adapter in smartphone, standard cable in Pi-Tail power (USB is free for dongles). More details are here: https://github.com/Re4son/RPi-Tweaks/blob/master/pi-tail/Pi-Tail.README
10. Wait until the Raspberry Pi connects to your hotspot (you can check the number of connected devices).
11. Connect from smartphone via installed SSH-client to SSH server `kali@192.168.123.254` default password `kali`
12. Launch an update in terminal `sudo apt update && sudo apt upgrade`
