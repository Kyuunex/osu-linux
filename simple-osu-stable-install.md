# How to install osu! (stable) on GNU/Linux with low latency

I could outline the manual steps, but I found it better to just make a script that automates everything. Although, you will need to [lower pulseaudio latency](https://github.com/Kyuunex/osu-linux/tree/main/pulseaudio-lower-latency.md) if you are using pulseaudio.

---

Firstly, make sure you have wine, winetricks and it's dependencies installed.

Download the script in your `bin` folder and make it executable through these commands: (This assumes your `bin` folder is in your `PATH`. If not, add it or use a different folder that is.)
```sh
mkdir -p "$HOME/.local/bin/"
wget -O "$HOME/.local/bin/osu-stable" "https://raw.githubusercontent.com/Kyuunex/my-linux-stuff/main/scripts/osu-stable-install/osu-stable"
chmod +x "$HOME/.local/bin/osu-stable"
```

Then open the script in your text editor of choice and on the first line, specify the installation location. If you already have the game files, just point it to that.

Feel free to edit other stuff you wanna change, like the wine prefix location.

If you prefer using `alsa` instead of `pulseaudio`, uncomment that line with `alsa` in it.

When you are done, run the script in the terminal, and during this, it will download the icon, make a menu entry, and set up the wine prefix. 

If it hangs while installing the .NET Framework, type `killall mscorsvw.exe` in a different terminal window.

When the script is done, it will launch osu. You can do whatever you want. 

All subsequent launches can be done the menu entry the script made.

Also, please don't use Audio Compatibility Mode, it makes the latency higher.

---

This guide is based on [this one](https://osu.ppy.sh/community/forums/topics/367783) by Franc[e]sco

Other than this, there's [ThePooN's guide](https://blog.thepoon.fr/osuLinuxAudioLatency/) for this but I have had many stability problems using a modifed versions of wine and also that wine build is outdated.

Alternatively, you can use [this lutris install script](https://lutris.net/games/osu/) that uses Franc[e]sco's solution with tkg's custom wine build, but, it does not set up file associations and `osu://` link handling I think, not sure.
