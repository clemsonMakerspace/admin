# Clemson Makerspace Octopi Setup
----

## Summary
The FDM 3D Printers in the Clemson Makerspace are all controlled through the use
of [Octoprint](http://octoprint.org/). In general this means that each printer
has a [Raspberry Pi](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/),
a [webcam](https://www.raspberrypi.org/products/camera-module-v2/), and an SD
card with [Octopi](https://octopi.octoprint.org/) installed. Each Raspberry Pi
is connected with a USB A-B cable to the printer and then the Raspberry Pi serves
as the print host.

The Clemson Makerspace currently has 3 different models of FDM printers.
1. [Type A Machines Series 1 Pro](https://www.typeamachines.com/series-1-pro)
2. [Lulzbot Taz 6](https://www.lulzbot.com/store/printers/lulzbot-taz-6)
3. [Lulzbot Mini](https://www.lulzbot.com/store/printers/lulzbot-mini)

Each of these three printers require slightly different settings in Octoprint.

There are a few general steps to setting up a new install of Octoprint.

1. [Download and flash](https://octopi.octoprint.org/) the most recent version
    of Octopi onto an SD card
2. Install the SD card in the Raspberry Pi and connect it to the Webcam, the
    printer, ethernet, and the power supply
3. SSH into `pi@octopi.local` with password `raspberry` and
    [change the hostname](#octoprint-hostname-and-title) (and optionally adjust
    the [webcam settings](#webcam-settings) for the Type A Machines)
4. Adjust the [Printer Profile settings](#printer-profiles)
5. Adjust the [Connection settings](#connection-settings)
6. Start printing!

### Octoprint Hostname and Title
By default, the Octopi distro has a hostname of `octopi`. This means that you
will have to connect to it by pointing your browser or SSH client to
`octopi.local`. With a large number of printers on the network this is
unworkable, as you will never know which of the printers you may have connected
to when working with `octopi.local`.

The preferred solution is to give each printer a unique hostname. Giving a
unique hostname to each printer allows you to connect to
http://series1-10306.local or http://taz-1.local instead of http://octopi.local
and know exactly which printer you're working with.


You can set a unique hostname by editing `/etc/hostname` and `/etc/hosts` on the
Raspberry Pi. For example, to change http://octopi.local to http://mini-1.local
you would replace the entire contents of `/etc/hostname` with `mini-1` and then
edit the last line of `/etc/hosts` to read `127.0.1.1 mini-1`.

### Webcam Settings
The Type A Machines Series 1 Pro printers come with a webcam with a Product ID
of `4332:1313`. In order to enable support for the webcam on these printers you
must edit `/boot/octopi.txt` and add a line reading
`additional_brokenfps_usb_devices=("4332:1313")`. The ideal spot for this is
after line 42.

Additionally, if you are using the Type A Machines webcam you will find that
Octoprint defaults to a 16:9 aspect ratio, which will result in letterboxing
because the included webcams have a 4:3 aspect ratio. You will have to go
change `Settings > Webcam & Timelapse > Stream Aspect Ratio` in the online
control panel to resolve the letterboxing.

### Printer Profiles
1. Type A Machines Series 1 Pro
    * Print bed & build volume
        * Width - `305mm`
        * Depth - `305mm`
        * Height - `305mm`
    * Axes
        * Z Axis - `Invert Control`
    * Hotend & extruder
        * Nozzle Diameter - `0.4mm`
2. Lulzbot Taz 6
    * Print bed & build volume
        * Width - `280mm`
        * Depth - `280mm`
        * Height - `250mm`
    * Hotend & extruder
        * Nozzle Diameter - `0.5mm`
3. Lulzbot Mini
    * Print bed & build volume
        * Width - `152mm`
        * Depth - `152mm`
        * Height - `158mm`
    * Hotend & extruder
        * Nozzle Diameter - `0.5mm`

### Connection Settings
1. Type A Machines Series 1 Pro
    * Serial Port - `/dev/ttyACM0`
    * Baudrate - `230400`
2. Lulzbot Taz 6
    * Serial Port - `/dev/ttyACM0`
    * Baudrate - `250000`
3. Lulzbot Mini
    * Serial Port - `/dev/ttyACM0`
    * Baudrate - `115200`
