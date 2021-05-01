# osu!lazer on Raspberry Pi

The steps are identical to compiling for normal Linux, because, well, Raspberry Pi runs Linux.  
Only real difference is that you need to publish the package for `linux-arm` instead of `linux-x64` for obvious reasons,

# Building
It is important to mention that building osu!lazer on your Raspberry Pi will take a long time. You have an option of doing this on your PC and copying the built files over. It will be much faster.
```sh
git clone https://github.com/ppy/osu.git
cd ./osu
git submodule update --init
dotnet restore osu.sln
dotnet build osu.sln
cd ./osu.Desktop
dotnet publish --self-contained --configuration Release --runtime linux-arm
```

# enabling the OpenGL drivers
`sudo raspi-config` > 'Advanced Options' > 'GL Driver' > 'GL (Fake KMS'). Press OK, Finish and Reboot.

# libbass
you can get libbass from [here](https://un4seen.com/forum/?topic=13804.0)  
if you are on 32 bit raspberry pi os, you will need to grab the binaries from the `hardfp` folder.
don't forget to get the fx24 one too 

# Running
After all these, just run the osu! file that is build in ./bin/Release/
Making sure you drop the libbass files first. After that, try running.

# Troubleshooting
## something something SDL
if you get a libSDL realted error, you need to symlink `/usr/lib/arm-linux-gnueabihf/libSDL2-2.0.so.0.9.0` to `/usr/lib/arm-linux-gnueabihf/libSDL2.so` or copy it to the game's folder as that filename.
## Attempted to read or write protected memory
Do not install `libasound2-dev`. if you did, uninstall it.
## Audio latency
This is because recently, Raspberry Pi OS has added Pulseaudio, which buffers sound and this leads to latency. One way around this is to use `pasuspender` to paunch osu which will suspend pulseaudio. Then in the osu!'s sound device settings, manually select an audio device you wish to hear the audio from.   
Unfortunately, you will need an external sound card for this to work. I was not able to get a stable sound this way though the built in headphone jack or HDMI.

# video
[video demonstration](https://youtu.be/G4YP2UsY9bk)
