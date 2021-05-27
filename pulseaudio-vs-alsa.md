# PulseAudio vs ALSA
PulseAudio and ALSA actually work together on a normal system, PulseAudio actually sits on top of ALSA.
The way the sound output works on Linux is, 
every device only accepts one audio stream per output at a time (as to my understanding).

But, on our PCs, we usually listen to multiple things at a time, 
For example, we can be in a Discord call while being in a multiplayer game or watching YouTube videos (together).
Something needs to mix this into one stream for ALSA to take, and that something is PulseAudio.
Unfortunately, to do this, it adds latency. 

You can forcefully lower PulseAudio latency
[using this guide](https://github.com/Kyuunex/osu-linux/tree/main/pulseaudio-lower-latency.md), 
but it will make PulseAudio take more CPU processing power, 
and your game may stutter when PulseAudio stutters, when your CPU can't keep up.

PipeWire is a relatively new multimedia framework that aims to replace PulseAudio in the future,
but in my testing, it's still in its infancy and is buggy, 
so I don't feel comfortable recommending it at this point in time.

That leaves us with sending audio straight to ALSA.
Sending straight to ALSA will have almost no latency and no elevated CPU usage,
but you will be giving up the ability to output multiple audio streams per audio out. 

This can be inconvenient unless you have multiple audio output devices.
If you do, PulseAudio can work as normal outputting to one audio device like your HDMI/DisplayPort monitor speakers,
while you route the game audio through your motherboard's 3.5 mm audio jack.
Maybe at the end, you can get an audio interface that mixes down both line audio outputs in real time?

Whether this is worth it for you is for you to decide. 

If you have only one audio output device, you can use `pasuspender` command, followed by the osu launch script, 
to suspend pulseaudio for the duration osu is running, 
it will ensure that PulseAudio is suspended and is not using the ALSA audio output all to itself, 
freeing it up for osu! to use.

Remember, to send audio to ALSA, you need to select your audio device in in-game settings.

In case of osu-stable in wine, if you do end up choosing ALSA,
you need to do `winetricks sound=alsa` once for your osu prefix to switch it to use ALSA.
Make sure to have "Audio compatibility mode" DISABLED.

When you do this, it will still use PulseAudio with some redirection plugin probably if you run it without `pasuspender` 
and are outputting everything to one audio device. It will also bug the game audio out. 
To avoid this, make sure to switch the system default audio output to something you won't be outputting 
the game audio to before launching osu beforehand.

TODO: ALSA and 1 audio device terminology is interchanged in this post, make sure to clarify everything later.
