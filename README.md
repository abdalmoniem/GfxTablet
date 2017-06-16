# What is GfxTablet?
GfxTablet shall make it possible to use your Android device (especially tablets) as a graphics tablet.
It consists of two components:

1. GfxTablet Android app
2. Input driver for your PC

The GfxTablet app sends motion and touch events via UDP to a specified host on port 40118.
The input driver must be installed on your PC.
It creates a virtual "Network Drawing Tablet" on your PC that is controlled by your Android device.
So, you can use your Android tablet or smartphone to control the PC and,
for instance draw in GIMP, MyPaint or any other drawing software.
(even pressure-sensitive, if your hardware supports it).

## Features:
1. Pressure sensitivity supported
2. Size of canvas will be detected and sent to the client
3. Option for ignoring events that are not triggered by a stylus pen: so you can lay your hand on the tablet and draw with the pen.

## Requirements:
1. App: Any device with Android 4.0+ and touch screen
2. Driver: Linux with uinput kernel module (included in modern versions of Fedora, Ubuntu etc.)


## Installation:

Github repository: https://github.com/abdalmoniem/GfxTablet

### Part 1: uinput driver
1. Clone the repository:
   `git clone git://github.com/rfc2822/GfxTablet.git`
2. Install gcc, make and linux kernel header includes (`kernel-headers` on Fedora)
3. `cd GfxTablet/driver-uinput; make`

Then, run the binary. The driver runs in user-mode, so it doesn't need any special privileges.
However, it needs access to `/dev/uinput`. If your distribution doesn't create a group for
uinput access, you'll need to do it yourself or just run the driver as root:

`sudo ./networktablet`

Then you should see a status message saying the driver is ready. If you do `xinput list` in a separate
terminal, should show a "Network Tablet" device.

You can start and stop (Ctrl+C) the Network Tablet at any time, but please be aware that applications
which use the device may be confused by that and could crash.

`network_tablet` will display a status line for every touch/motion event it receives.


### Part 2: App

you can:

1. install the apk directly from a pc using adb (make sure that you have the android sdk):
	1. `cd GfxTablet/app-android`
	2. `adb install gfxtablet.apk`

2. or just copy the apk file to your phone/tablet and install it.

After installing, enter your host IP in the Settings / Host name and it should be ready.


### Part 3: Using it

Now you can use your tablet as an input device in every Linux application (including X.org
applications). For instance, when networktablet is running, GIMP should have a "Network Tablet"
entry in "Edit / Input Devices". Set its mode to "Screen" and it's ready to use.


### FAQ:

#### Using with multiple monitors:

If you're using multiple screens, you can assign the Network Tablet device to a specific screen
once it's running:

1. Use `xrandr` to identify which monitor you would like to have the stylus picked up on. In this example, `DVI-I-1`
   is the display to assign.
2. Do `xinput map-to-output "$( xinput list --id-only "Network Tablet" )" DVI-I-1`.