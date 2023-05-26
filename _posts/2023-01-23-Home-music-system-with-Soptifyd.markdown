---
layout: post
title: How to create a fancy home music system with Spotifyd.
date: 2023-01-23
excerpt: "Set up an open source Spotify client to run as a daemon streaming music in your apartment, which you can control from any device connected to your internet."
---

Imagine. You arrive home, and you want to put up some ambient music.
You open your Spotify app on your phone and play your playlist of choice. 
It would be cool if this music could diffuse automatically into your apartment. 
With [Spotifyd][spotifyd], that is possible.
You simply select to play the music on your [Spotifyd][spotifyd] daemon instead of on your phone.
You can even adjust the volume from your mobile phone, and there is no interference with other audio from your phone (calls, ringtones, alarms). You can change the music from the Spotify app on any other device connected to your internet.

That evening, a friend comes to visit. She is enthusiastic about a new album release that she wants you to hear. 
She pulls her phone, connects to your guest’s wifi, plays the album on her Spotify app, and chooses your [Spotifyd][spotifyd] daemon as the device where to play it.

If these scenarios feel appealing to you, you must create a [Spotifyd][spotifyd] daemon and connect it to your audio system. 
You need a computer where to run the daemon.
This computer can be an old laptop you do not use anymore, or a [Raspberry Pi][raspberrypi] device. It will run 24h/7. 

# The hardware

I run my [Spotifyd][spotifyd] daemon on [Ubuntu][ubuntu] OS on a old machine. You could host [Spotifyd][spotifyd] on a [Raspberry Pi][raspberrypi]. 
You must connected your hardware to your home internet. 

And you must connect your audio system or the loudspeaker to the machine hosting [Spotifyd][spotifyd].
To install and configure [Spotifyd][spotifyd], it is sufficient that you can SSH the machine. You do not need a monitor.

# The software

Follow the [installation instructions][spotifyd-installation] on the Spotifyd GitHub Page to install [Spotifyd][spotifyd] on your system. It is rather easy.
Once the installation is done, [configure your Spotifyd daemon][spotifyd-configuration]. Here, you can set your cache parameters, if any, your initial volume settings, the audio bitrate, etc. etc.
Finally, you need to [set up the daemon to run as a service][spotifyd-service].

# Power managment

The computer is old, and it is constantly on. To minimise power consumption, I installed [pm-powersave][pm-utils]
```
sudo apt-get install pm-utils
```
To enter low power mode, it is sufficient to run 
```
sudo pm-powesave true
```
Enjory it!

# References:
**Spotifyd**: https://github.com/Spotifyd/spotifyd#spotifyd-

**Raspberrypi**: https://www.raspberrypi.com

**Ubuntu**: https://ubuntu.com/download/desktop

[spotifyd]: https://github.com/Spotifyd/spotifyd#spotifyd-
[spotifyd-installation]: http://spotifyd.github.io/spotifyd/Introduction.html
[spotifyd-configuration]: http://spotifyd.github.io/spotifyd/config/File.html
[spotifyd-service]: http://spotifyd.github.io/spotifyd/config/services/index.html
[raspberrypi]: https://www.raspberrypi.com
[ubuntu]: https://ubuntu.com/download/desktop
[pm-utils]:https://help.ubuntu.com/community/PowerManagement/ReducedPower
