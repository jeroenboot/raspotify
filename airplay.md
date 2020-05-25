#Add airplay functionality to raspberry zero
Be aware, the soundcard is exclusive to one process. If Airplay is selected, then Raspotify doesn't have a soundcard available. This can be solved by using a middleware application like pulse-audio.


####Preparation  
Adjust /etc/fstab to add more tmpfs space for APT "/var/cache"  
`tmpfs    /var/cache         tmpfs   defaults,noatime,nosuid,size=300m                    0 0`

Add the ca-certificates back  
`sudo apt install --reinstall ca-certificates`

Fix NTP  

There might be an issue with the clock, not sure if related to read-only FS
```
$ sudo systemctl stop ntp
$ sudo ntpd -gq
```

####PulseAudio
Might require Pulseaudio, not sure yet  
`#sudo apt-get install -y pulseaudio pulseaudio-module-zeroconf alsa-utils avahi-daemon pavucontrol paprefs`


###Install

Get all the dependencies
```
sudo apt-get update
sudo apt-get install autoconf automake avahi-daemon build-essential git libasound2-dev libavahi-client-dev libconfig-dev libdaemon-dev libpopt-dev libssl-dev libtool xmltoman
```

Get the code  
`git clone https://github.com/mikebrady/shairport-sync.git`

Install the software, alsa backend for now
```
cd shairport-sync
autoreconf -i -f
./configure --with-alsa --with-avahi --with-ssl=openssl --with-systemd --with-metadata

make
sudo make install
```


####Test and enable
`$ sudo service shairport-sync start`\
`$ sudo systemctl enable shairport-sync`