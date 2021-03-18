# How to lower pulseaudio latency (for osu!)
Based on [ThePooN's guide](https://blog.thepoon.fr/osuLinuxAudioLatency/). I'm just putting this here to keep things short.

+ Make this folder if it already does not exist: `/etc/pulse/daemon.conf.d/`
    + You can use the command bellow to make this happen:
        ```
        sudo mkdir -p /etc/pulse/daemon.conf.d/
        ```
+ Create this file `/etc/pulse/daemon.conf.d/10-better-latency.conf`
    + You can use the command bellow to make this happen:
        ```
        sudo nano /etc/pulse/daemon.conf.d/10-better-latency.conf
        ```
        + You can press Ctrl+X, then Y and then Enter to save changes.
    + Put the following inside: (Right click and paste)
        ```yaml
        high-priority = yes
        nice-level = -15

        realtime-scheduling = yes
        realtime-priority = 50

        resample-method = speex-float-0

        default-fragments = 4
        default-fragment-size-msec = 2
        ```
        + You can adjust the last 2 values if this does not work for you, or you get crackling audio.
+ Edit this file: `/etc/pulse/default.pa`
    + You can use the command bellow to make this happen:
        ```
        sudo nano /etc/pulse/default.pa
        ```
        + You can press Ctrl+X, then Y and then Enter to save changes.
    + Locate this line: `load-module module-udev-detect`
        + Append `tsched=0` to it so it looks like this `load-module module-udev-detect tsched=0`
+ Create this file: `/etc/security/limits.d/10-audio.conf`
    + You can use the command bellow to make this happen:
        ```
        sudo nano /etc/security/limits.d/10-audio.conf
        ```
        + You can press Ctrl+X, then Y and then Enter to save changes.
    + Put the following inside:
        ```
        @audio - nice -20
        @audio - rtprio 99
        ```
        + Replace `@audio` with your username or add your user account to the `audio` group by `sudo gpasswd -u YOUR_USERNAME audio`.
+ Reboot
