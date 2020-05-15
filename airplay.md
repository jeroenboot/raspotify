#Add airplay functionality to raspberry zero
It requires pulse-audio, because alsa needs exclusive access to an hardware audiocard


####Preparation  
Adjust /etc/fstab to add more tmpfs space for APT "/var/cache"  
`tmpfs    /var/cache         tmpfs   defaults,noatime,nosuid,size=300m                    0 0`

Add the ca-certificates back  
`sudo apt install --reinstall ca-certificates`

pi@zero1:~ $ sudo systemctl stop ntp
pi@zero1:~ $ sudo ntpd -gq    

####PulseAudio
Might require Pulseaudio, bot sure yet  
`sudo apt-get install -y pulseaudio pulseaudio-module-zeroconf alsa-utils avahi-daemon pavucontrol paprefs`


###Install

Get all the dependencies
```
sudo apt-get update
sudo apt-get install autoconf automake avahi-daemon build-essential git libasound2-dev libavahi-client-dev libconfig-dev libdaemon-dev libpopt-dev libssl-dev libtool xmltoman
```

Get the code  
`git clone https://github.com/mikebrady/shairport-sync.git`

Install the software
```
cd shairport-sync
autoreconf -i -f
./configure --with-alsa --with-avahi --with-ssl=openssl --with-systemd --with-metadata

make
sudo make install
```


####Test and enable
$ sudo service shairport-sync start
$ sudo systemctl enable shairport-sync