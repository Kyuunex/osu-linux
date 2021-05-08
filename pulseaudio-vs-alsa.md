# Pulseaudio vs ALSA
pulseaudio actually sits on top of alsa. 
The way the sound output works on Linux is, every device only accepts one audio stream per output at a time. 
But, on our PCs, we usually listen to multiple things at a time, 
for example, we can be in a Discord call while being in a multiplayer game or watching YouTube videos (together). 
Something needs to mix this into one stream for alsa to take, and that something is pulseaudio. 
Unfortunately, to do this, it adds latency. 

You can forcefully lower pulseaudio latency 
[using this guide](https://github.com/Kyuunex/osu-linux/tree/main/pulseaudio-lower-latency.md), 
but it will make pulseaudio take more CPU power, and your game may stutter when pulseaudio stutters. 

pipewire is a relatively new multimedia framework that aims to replace pulseaudio in the future, 
but in my testing, it's still in its infancy and is buggy. 

That leaves us with sending audio straight to alsa. 
Sending straight to alsa will have almost no latency and no elevated CPU usage, 
but you will be giving up the ability to output multiple audio streams per audio out. 

Well, that is unless you have multiple devices, 
in that case, pulseaudio can work as normal outputting to one audio device like your speakers,
while you route the game audio through headphones on another audio device.

Weather this is worth it for you is for you to decide. 

You can use `pasuspender` command like utility to suspend pulseaudio and send straight to alsa temporarily 
(if you only use one audio output device). 

You use it by typing `pasuspender` followed by the osu launch script you will make, and for the duration osu is running, 
it will ensure that pulseaudio is suspended, and you are sending audio straight to alsa. 

Remember, to send audio to alsa, you need to select your audio device in in-game settings.

In case of osu-stable in wine, if you do end up choosing alsa, 
you need to do `winetricks sound=alsa` once for your osu prefix to switch it to use alsa. 

When you do this, it will still use pulseaudio with some redirection plugin probably if you run it without `pasuspender`, 
unless, again, you output audio to a separate audio device.  

TODO: explain this better
