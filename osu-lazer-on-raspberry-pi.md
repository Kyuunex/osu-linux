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

# using a tablet
Since lazer has integrated OpenTabletDriver, you can just configure some udev rules, blacklist `hid_uclogic` and it will just work. Check out [this page](https://github.com/OpenTabletDriver/OpenTabletDriver/wiki/Linux-FAQ) on that.

# Troubleshooting
## something something SDL
`sudo apt install libsdl2-dev`
## Attempted to read or write protected memory
Do not install `libasound2-dev`. if you did, uninstall it.
## Audio latency
This is because recently, Raspberry Pi OS has added Pulseaudio, which buffers sound and this leads to latency. One way around this is to use `pasuspender` to launch osu which will suspend pulseaudio. Then in the osu!'s sound device settings, manually select an audio device you wish to hear the audio from.   
Unfortunately, you will need an external sound card for this to work. I was not able to get a stable sound this way though the built in headphone jack or HDMI.

# video
[video demonstration](https://twitter.com/Kyuunex/status/1388499354617622534)
