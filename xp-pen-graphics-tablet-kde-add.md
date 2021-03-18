# How to set up XP-Pen G430 or G640 on Linux (KDE)
It took me a while to figure this out, so I'm putting this here so you'll have an easier time.  
This is for KDE only. I have not used other DEs for osu! so I can't help you there.

# Install these packages
+ Arch based distro: `xf86-input-wacom` and `kcm-wacomtablet`
+ Debian based distro: `xserver-xorg-input-wacom` and `kde-config-tablet`

# Configure
+ Make the `/etc/X11/xorg.conf.d/` folder if it does not already exist. 
    + `sudo mkdir -p /etc/X11/xorg.conf.d/`
+ Create this file `/etc/X11/xorg.conf.d/70-tablet.conf`
    + For XP-Pen G640, put this in:
        ```sh
        Section "InputClass"
            Identifier "G640"
            MatchIsTablet "on"
            MatchProduct "XP-PEN STAR G640"
            MatchDevicePath "/dev/input/event*"
            Driver "wacom"
        EndSection
        ```
    + For XP-Pen G430, put this in:
        ```sh
        Section "InputClass"
            Identifier "G430"
            MatchIsTablet "on"
            MatchProduct "UGTABLET TABLET G3 4x3"
            MatchDevicePath "/dev/input/event*"
            Driver "wacom"
        EndSection
        ```
+ Reboot
+ System Settings > Hardware > Input Devices > Graphic Tablet > Manually register a tablet device > Save
+ Reboot
+ Go back to those settings and you should be able to configure your area, smoothing and etc.
