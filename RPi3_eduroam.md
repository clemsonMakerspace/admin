# General Overview

There are a few general steps to setting up a new Raspberry Pi on Eduroam at Clemson University.

1. [Download and flash](https://www.raspberrypi.org/downloads/raspbian/) the most recent version of Raspbian Lite
2. Enable SSH on the pi by adding a file to the boot directory called `ssh` without any extension
3. Install the SD card in the Raspberry Pi and connect it to ethernet, and the power supply
4. SSH into `pi@raspberrypi.local` with password `raspberry` and [change the hostname](#hostname-and-title)
5. Adjust the [Dynamic DNS](#dynamic-dns)
6. Adjust the [Eduroam settings](#eduroam)
7. Start computing!

### Hostname and Title
By default, the Raspbian distro has a hostname of `raspberrypi`. This means that you
will have to connect to it by pointing your browser or SSH client to
`raspberrypi.local`. With a large number of Pis on the network this is
unworkable, as you will never know which of the Pis you may have connected
to when working with `raspberrypi.local`.

The preferred solution is to give each pi a unique hostname. Giving a
unique hostname to each printer allows you to connect to
http://series1-10306.local or http://taz-1.local instead of http://raspberrypi.local
and know exactly which pi you're working with.


You can set a unique hostname by editing `/etc/hostname` and `/etc/hosts` on the
Raspberry Pi. For example, to change http://raspberrypi.local to http://mini-1.local
you would replace the entire contents of `/etc/hostname` with `mini-1` and then
edit the last line of `/etc/hosts` to read `127.0.1.1 mini-1`.

### Dynamic DNS
- Sign into http://freedns.afraid.org
- Select `Dynamic DNS`
- Select subdomain and choose `quick cron example`
- SSH into your pi, and using `crontab -e` add the cron example to your crontab
- Wait a few minutes and make sure your domain is updating

### Eduroam
- `ifconfig` then check the `wlan0` mac address
- Register your mac address on netreg.clemson.edu

Append the following to `/etc/network/interfaces`
```
auto lo
iface lo inet loopback

iface eth0 inet manual

allow-hotplug wlan0
iface wlan0 inet manual
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

allow-hotplug wlan1
iface wlan1 inet manual
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```
replace your `/etc/wpa_supplicant/wpa_supplicant.conf` with this
```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
        scan_ssid=1
        ssid="eduroam"
        key_mgmt=WPA-EAP
        eap=PEAP
        identity="username@clemson.edu"
        password=hash:NtPasswordHash
        phase2="MSCHAPV2"
}
```

To generate Nt Password hash
`echo -n plaintext_password_here | iconv -t utf16le | openssl md4 `

Prefix it with "hash:" in the wpa_supplicant.conf file, i.e.
`password=hash:6602f435f01b9173889a8d3b9bdcfd0b`

### Testing your work
Reboot your system with `sudo reboot` and then reconnect to your new hostname.  
You can use `sudo ifconfig` and `sudo iwconfig` to get more information about your wifi or ethernet connections.  
You can also use `ping -I wlan0 google.com` or something similar to test your new wifi connection.  
