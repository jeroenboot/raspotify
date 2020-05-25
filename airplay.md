#Add airplay functionality to raspberry zero, with RO filesystem
Be aware, the soundcard is exclusive to one process.  
If Airplay is selected, then Raspotify doesn't have a soundcard available.


####Preparation  
Adjust /etc/fstab to add more tmpfs space for APT "/var/cache", else the update will fail.  
`tmpfs    /var/cache         tmpfs   defaults,noatime,nosuid,size=300m                    0 0`

Fix NTP  

There might be an issue with the clock, not sure if related to read-only FS
```
$ sudo systemctl stop ntp
$ sudo ntpd -gq
```

Add the ca-certificates back, these might have changed preventing apt to work correctly.  
`sudo apt install --reinstall ca-certificates`

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