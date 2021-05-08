# Introduction
Getting osu! to run on Linux under wine is not hard at all, however, it may be a bit tricky if you want the absolute lowest latency or want everything to work perfectly. Even though, I've had much worse experience dealing with people in the osu! mapping community than getting this game to run perfectly on Linux, that should still give you some sort of idea how piece of cake this still is to do. So, in this guide, I will put everything I know about getting osu! (Stable) to run under wine.

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
First, you need to decide if you want to use pulseaudio or alsa.  
there is a dedicated document written about this in this repo

## Lowering wine latency
This can be done in few ways. I used to use modified wine builds made by ThePooN and his community, but I have had many problems with that build. I for stutters, and underruns. I recently switched to a better solution, official builds with some registry tweaks to lower latency.
```sh
wine reg add "HKCU\Software\Wine\DirectSound" /v "HelBuflen" /t REG_SZ /d "512" /f
wine reg add "HKCU\Software\Wine\DirectSound" /v "SndQueueMax" /t REG_SZ /d "3" /f 
```   

# to be continued