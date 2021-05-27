# How to install osu! (stable) on GNU/Linux with low latency

I could outline the manual steps, but I found it better to just make a script that automates everything. 
Although, you will need to 
[lower pulseaudio latency](https://github.com/Kyuunex/osu-linux/tree/main/pulseaudio-lower-latency.md) 
if you are using pulseaudio.

---

Firstly, make sure you have wine, winetricks, and it's dependencies installed.

Download the script in your `bin` folder and make it executable through these commands: 
(This assumes your `bin` folder is in your `PATH`. If not, add it or use a different folder that is.)
```sh
mkdir -p "$HOME/.local/bin/"
wget -O "$HOME/.local/bin/osu-stable" "https://raw.githubusercontent.com/Kyuunex/osu-linux/main/osu-stable"
chmod +x "$HOME/.local/bin/osu-stable"
```

Then open the script in your text editor of choice and on the first line, specify the installation location. 
If you already have the game files, just point it to that. It won't re-download them.

Feel free to edit other stuff you want to change, like the wine prefix location.

If you prefer using ALSA instead of `PulseAudio`, uncomment that line with `alsa` in it. 
Read [this guide](https://github.com/Kyuunex/osu-linux/tree/main/pulseaudio-vs-alsa.md) 
if you don't know what this means.

When you are done, run the script in the terminal, and during this, it will download the icon, make a menu entry, 
and set up the wine prefix. 

If it hangs while installing the .NET Framework, type `killall mscorsvw.exe` in a different terminal window.

When the script is done, it will launch osu. You can do whatever you want. 

All subsequent launches can be done the menu entry the script made.

Also, please don't use Audio Compatibility Mode, it makes the latency higher.

---

This guide is based on [the wine registry stuff](https://osu.ppy.sh/community/forums/topics/367783) 
Franc[e]sco posted about in their forum post,
