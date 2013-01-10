download debian image from offical website

add new user:
  sudo useradd -m mononofu
  sudo passwd mononofu

add new user to groups (all groups where pi is member):
  sudo nano /etc/group

`chsh` to zsh

then `userdel pi`

to use less memory for video, do `sudo raspi-config` and change memory split

set a static ip in /etc/network/interfaces:

iface eth0 inet static
address 192.168.1.4
netmask 255.255.255.0
gateway 192.168.1.1

and probably dns too, in /etc/resolv.conf:
nameserver 8.8.8.8

as a webserver, use flask

http://www.penguintutor.com/linux/raspberrypi-webserver


setup wifi: 

install tightvncserver, the vnc into the pi. start the wifi gui and connect once to create the wpa supplicant config file,
then add it to rc.local using those instructions:

http://stinebaugh.info/auto-start-your-wifi-on-raspberry-pi/


to enable sound, add "hdmi_drive=2" to /boot/config
