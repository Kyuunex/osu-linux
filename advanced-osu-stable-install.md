# Introduction
Getting osu! to run on Linux under wine is not hard at all, however, it may be a bit tricky if you want the absolute lowest latency or want everything to work perfectly. I've had much worse experience dealing with people in the osu! mapping community than getting this game to run perfectly on Linux, so that should give you a sort of idea how piece of cake this is. So, in this guide, I will put everything I know about getting osu! (Stable) to run under wine.

# Before getting started
Before getting started, you need to create a wine prefix to use exclusively for osu! A wine prefix is basically folder that contains all the dependencies osu! needs on top of wine to run perfectly. The stuff we will be installing basically. To make a new prefix, simply do this  
```sh
mkdir -p "$HOME/.local/share/wineprefixes/"
export WINEPREFIX="$HOME/.local/share/wineprefixes/osu-stable"
export WINEARCH=win32 
```
Don't just blindly do this. Pay attention on the path, this is where the prefix for osu will be. Change it if you want to, but this is the default path where the prefixes are. We will also be making the prefix 32-bit, since the game is 32-bit.

# .NET Framework
osu! is built using the .NET Framework, specifically, the binaries seem to be compiled to work with version 4.0. If you install .NET Framework 4.0 in your wine prefix, you should be good to go in this regard. `winetricks -q --force dotnet40`

# GDI+
osu! uses GDI+ to draw some icons. You need this. `winetricks -q gdiplus`

# Missing Fonts
Here is a list of fonts that osu! utilizes: msgothic.ttc meiryo.ttc malgun.ttf simsun.ttc micross.ttf segoeui.ttf tahoma.ttf tahomabd.ttf marlett.ttf arialbd.ttf ariali.ttf arial.ttf SourceHanSans-Regular.ttc  
Unfortunately, not all of them are available through winetricks. You will need to source some from a Windows install or the internet.  
Here's what we got so far `winetricks -q cjkfonts arial vlgothic meiryo tahoma sourcehansans`

# Latency
Oh boy oh boy, this is a really tricky topic. 

## pulseaudio or alsa? or maybe even pipewire
First, you need to decide if you want to use pulseaudio or alsa. pulseaudio actually sits on top of alsa. The way the sound output works on Linux is, every device only accepts only one audio stream per output at a time. But on our PCs, we usually listen to multiple things at a time, for example, we can be in a Discord call while being in a multiplayer game or watching youtube videos (together). Something needs to mix this into one stream for alsa to take, and that something is pulseaudio. Unfortunately, to do this, it adds latency. You can forcefully lower pulseaudio latency [using this guide](https://github.com/Kyuunex/osu-linux/tree/main/pulseaudio-lower-latency.md), but it will make pulseaudio take more CPU power and your game may stutter when pulseaudio stutters. pipewire is a relatively new multimedia framework that aims to replace pulseaudio in future, but in my testing, it's still in it's infancy and is buggy. That leaves us with sending audio straight to alsa. Sending straight to alsa will have almost no latency and no elevated CPU usage, but you will be giving up the ability to output multiple audio streams per audio out. Weather this is worth it for you is for you to decide. You can use `pasuspender` command like utility to suspend pulseaudio and send straight to alsa temporarily. You use it by typing `pasuspender` followed by the osu launch script you will make, and for the duration osu is running, it will ensure that pulseaudio is suspended and you are sending audio straight to alsa. I personally recommend lowering pulseaudio latency with what I linked, and call it a day, the tradeoffs are not worth it. If you do end up choosing alsa, you need to do `winetricks sound=alsa` once for your osu prefix to switch it to use alsa. When you do this, it will still use pulseaudio probably if you run it without `pasuspender`.  
TODO: explain this better, pulse can do some audio stream redirects

## Lowering wine latency
This can be done in few ways. I used to use modified wine builds made by ThePooN and his community, but I have had many problems with that build. I for stutters, and underruns. I recently switched to a better solution, official builds with some registry tweaks to lower latency.
```sh
wine reg add "HKCU\Software\Wine\DirectSound" /v "HelBuflen" /t REG_SZ /d "512" /f
wine reg add "HKCU\Software\Wine\DirectSound" /v "SndQueueMax" /t REG_SZ /d "3" /f 
```   

# to be continued