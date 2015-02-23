Title: Pi on Rails  
Subtitle: How to setup a Raspberry Pi to run Rails  
Comments: True  

Setting up your RaspberryPi so that it can run a rails app can be a time
consuming proposition, in this post I'll cover how to setup a
Raspberry Pi run a ruby on rails project that uses GPIO.

Segment 16's code is open source and avaiable here: https://github.com/bguest/blinky

**Notes**:

+ These instructions assume that you are using a Macintosh computer to do the setup.
+ This assumes that you are familiar with running things from the command line, or
  your really good at following directions.
+ '$' implies Terminal Command line (you shouldn't enter $ just what follows)
+ '\>\>' implies irb Command line (you shouldn't enter >> just what follows)
+ When asked if you want to install things, you will always answer yes.

## What you will need

### Hardware

1. A Macintosh computer, sorry that's what I have.
1. A Raspberry Pi (get one from [sparkfun](https://www.sparkfun.com/products/11546))
2. Micro USB to USB ([Amazon][micro_usb])
2. 8 GB SD Card ([Amazon][sd_card])
3. SD Card Reader ([Amazon][card_reader])
4. Router connected to your Mac (wireless or otherwise)
5. Ethernet Cable
6. Screen that accepts HDMI
7. HDMI Cable
8. USB Keyboard

### Software

Copy of Raspbian ([Raspbian][raspbian])

**Note** I used to recomend using [Occidentalis][occidentalis], but it
hasn't been updated in quite sometime and is not compatible with newer
versions of the RasbperryPi revB that use the HYNIX ram chips.

## Steps

### A. Format the SD Card

2. Insert SD Card into the SD Card Reader and plug that into a USB slot on your mac.
2. Open the Disk Utility application.
3. Select the SD Card.
4. Select the Erase Tab.
5. Set the format to MS-DOS (FAT)
6. Set the name to something recognizable like RASP_PI
7. Click Erase... (and confirm if necessary)

### B. Install Rasbian

* Decompress rasbian (usually by double clicking the zip you downloaded), you should have a file called something like 2014-01-07-weezy-rasbian.img
* Open up the Terminal.app (or whatever terminal you prefer)
* Type `df -h` then return and you should see a line that looks something like

``` text
/dev/disk4s1   7.4Gi  832Ki  7.4Gi     1%         0         0  100%   /Volumes/RASP_PI
```

* Unmount that partition with `diskutil unmount /dev/disk4s1` where the `4s1` part comes from
   whatever you saw in the previous step you should see something like

``` text
Volume RASP_PI on disk4s1 unmounted
```

If you see something different you may need to run the previous command with `sudo`

* Copy the rasbpian image to the SD card.

In the following command, substitute <path to> with the path to your copy of Raspbian and change <#> to the number of the disk from step 3

``` bash
sudo dd bs=1m if=<path to>/2014-01-07-wheezy-raspbian.img of=/dev/rdisk<#>
```

**WARNING** Be sure you know what you are doing here, if you screw up and enter the wrong disk number you could overwrite you whole hard drive.

* Wait. It will take a second, but eventually you will see something like

``` text
2479+1 records in
2479+1 records out
2600000000 bytes transferred in 323.754960 secs (8030765 bytes/sec)
```

### C. Boot up Raspberry Pi

1. Eject the Raspberry Pi SD Card.
2. Insert the SD Card in to the Pi.
3. Use the Micro USB to USB to connect the Raspberry Pi to a power source (your computer or USB power
   adapter)
3. Use the ethernet cable to connect your router to the Raspberry Pi
3. Plug your keyboard into one of the USB ports
3. Connect Raspberry Pi to Screen using HDMI Cable
4. Wait a few minutes, There are a bunch of LEDs on the Raspberry Pi they should all be lit and
   blinking happily, on the screen you should see your raspberry pi booting up.

### D. raspi-config

If you are not greated with a blue screen, but instead with a terminal type `sudo raspi-config`

**Note** to move around use the arrow and tab keys

Using raspi-config

* Expand Filesystem: select `Expand Filesystem` press <enter> twice
* Change User Password: select `Change User Password`, press enter, do the password change thing
* Change the locale to US: select `Internationalization Options`, press enter, then select `I1
  change locale`, use the <space> bar to unselect `en_GB.UTF-8 UTF-8`, and select `en_US.UTF-8
UTF-8`, use tab to select <Ok>, press enter then select `en_US.UTF-8`, select <Ok>, press enter
* Go to Advanced Optiong: select `Advanced Options`, press enter

In Advanced Options

* Change the hostname: select `A2 Hostname` press enter, change to something awesome, remember it or
  write that name down
* Enable SPI: select `A5 SPI`, press enter, select `<yes>`, press enter again
* While you are here, you may as well update raspi-config, select `A7 Update`, select `<OK>`

in the main raspi-config, select `<finish>`, press enter, when prompted, confirm that you would like
to reboot.

### E. Install Bonjour

When the raspberry pi boots backup, log in then

``` bash
sudo apt-get updtate
sudp apt-get upgrade
sudo apt-get install avahi-daemon
```

### F. Setup User

Ok you can now login through ssh, on your normal computer, ssh into the Raspberry Pi, In the terminal type

``` bash
ssh pi@raspberrypi.local
```

The default password is `raspberry`

Change the user pi's default password, in ssh session, make the prompt look like

``` bash
pi@raspberrypi ~ $ passwd
```

   Enter the old default `raspberry` password followed twice by your new password as requested. Remember your new password or better yet use an application like 1Password to remember it for you

* Change the root user password, similar to changing the pi user password except with `sudo passwd root`
* Exit out of the current ssh session using `exit` and log back in as the root user using
   `ssh root@raspberrypi.local`

### G. Setup correct locale

Setup the correct locale, log back into the RaspberryPi using `$ ssh root@raspberrypi.local`

   Use `vi` or `nano` to edit `/ect/default/locale` make it look like:

``` bash
LANG=en_GB.UTF-8
LANGUAGE=en_GB.UTF-8
```

### H. Add SSH Key to Pi (Optional but suggested)

At the command prompt enter the following two lines individually

``` bash
cat ~/.ssh/id_rsa.pub | ssh pi@raspberrypi.local 'mkdir .ssh; touch .ssh/authorized_keys; cat >> .ssh/authorized_keys'
```

``` bash
cat ~/.ssh/id_rsa.pub | ssh root@raspberrypi.local 'mkdir .ssh; touch .ssh/authorized_keys; cat >> .ssh/authorized_keys'
```

Follow prompts, the passwords requested are the passwords that you entered above

### I. Setup WiFi (optional)

ssh into raspberry pi (`ssh root@raspberrypi.local`) edit `/etc/network/interfaces` to look like

``` bash
auto lo

iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0
auto wlan0


iface wlan0 inet dhcp
        wpa-ssid "ssid"
        wpa-psk "password"
```

### J. Install Software: Ruby, RubyGems, Javascript, Thin, Nginx, PostgreSQL, Redis

Note: You can install ruby using rvm, but I don't recommend it as its kind of overkill
and you will likely run into problems later.

* ssh into your Pi as root `ssh root@raspberrypi.local`
* Install dependencies

``` bash
apt-get update # Note: you don't need to apt-get update if you already `sudo apt-get update` above
apt-get install libreadline-gplv2-dev libyaml-dev libssl-dev libffi-dev
```

* Install Ruby, I'm using the latest version at the time of writing (2.1.1), however you can
   choose, whatever version you feel like by going to the [ruby-lang ftp site][ruby-lang-ftp]
   and finding the version you want to install.

``` bash
cd ~
wget ftp://ftp.ruby-lang.org/pub/ruby/2.1/ruby-2.1.1.tar.gz
gunzip ruby-2.1.1.tar.gz
tar -xf ruby-2.1.1.tar
rm ruby-2.1.1.tar
cd ruby-2.1.1
./configure --prefix=/usr     # Takes long enough to grab a beer and maybe drink it.
make                          # Takes a long time (~2 hours), go for a hike.
make install                  # Drink another beer.
```

Check the ruby version and fire up irb to see if things are working

``` bash
$ ruby --version
$ irb
>> 2+2 #=> 4 ...
```

Delete the source code to save space (optional, but recommended)

``` bash
cd ..
rm -fR ruby-2.1.1
```

Install NodeJS (because we need a Javascript runtime for rails)

``` bash
apt-get install nodejs
```

Install PostgreSql

``` bash
apt-get install postgresql
pg_createcluster 9.1 main --start
apt-get install postgresql-server-dev-9.1
```

Setup the database with a root user

``` bash
su - postgres
psql template1
```

The following was not done on the Rubarian.img

``` postgres
CREATE USER root WITH PASSWORD 'your password';
CREATE DATABASE blinky_production;
GRANT ALL PRIVILEGES ON DATABASE blinky_production TO root;
ALTER USER root WITH SUPERUSER CREATEDB;
\q
```

``` bash
$ exit    # exit out of using postgres user
```

Install RubyGems and Bundler

``` bash
apt-get install rubygems
echo "gem: --no-ri --no-rdoc" >> ~/.gemrc  # Don't install documentation
gem install bundler
```

Install Redis

``` bash
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make
make install
cp redis.conf /etc/redis.conf
```

Edit the `/etc/redis.conf`, change `daemonize no` to

```
daemonize yes
```

Now start the redis server with

``` bash
redis-server /etc/redis.conf
```

Install Thin

``` bash
gem install thin
thin install
/usr/sbin/update-rc.d -f thin defaults    # Start thin on boot
thin config -C /etc/thin/blinky -c /var/www/blinky/current --servers 1 -e production
```

Install Nginx

``` bash
apt-get install nginx
```

   Edit the file at /etc/nginx/sites-available/blinky

``` bash
vi /etc/nginx/sites-available/blinky
```

   Or if you are uncomfortable with `vi` you can use `nano`

``` bash
nano /etc/nginx/sites-available/blinky
```

   Edit this file to look like

``` javascript
upstream blinky {
  server 127.0.0.1:3000;
}
server {
  listen   80;
  server_name .raspberrypi.local;

  access_log /var/www/blinky/current/log/access.log;
  error_log  /var/www/blinky/current/log/error.log;
  root       /var/www/blinky/current;
  index      index.html;

  location / {
    proxy_set_header  X-Real-IP  $remote_addr;
    proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header  Host $http_host;
    proxy_redirect  off;
    try_files /system/maintenance.html $uri $uri/index.html $uri.html @ruby;
  }

  location ~ ^/(assets)/  {
    root /var/www/blinky/current/public;
    gzip_static off;
    expires max;
    add_header Cache-Control public;
    # access_log /dev/null;
  }

  location @ruby {
    proxy_pass http://0.0.0.0:3000;
  }
}
```

   Enable this configuration

``` bash
ln -nfs /etc/nginx/sites-available/blinky /etc/nginx/sites-enabled/blinky
```

Setup for sidekiq

Install upstart

``` bash
apt-get install upstart
```

Edit `/etc/init/sidekiq.conf` to look similar to [this][https://github.com/mperham/sidekiq/blob/master/examples/upstart/manage-one/sidekiq.conf]
making modifications to the last block (see below)

Then you must restart.

``` bash
# /etc/init/sidekiq.conf - Sidekiq config

# This example config should work with Ubuntu 12.04+.  It
# allows you to manage multiple Sidekiq instances with
# Upstart, Ubuntu's native service management tool.
#
# See workers.conf for how to manage all Sidekiq instances at once.
#
# Save this config as /etc/init/sidekiq.conf then mange sidekiq with:
#   sudo start sidekiq index=0
#   sudo stop sidekiq index=0
#   sudo status sidekiq index=0
#
# or use the service command:
#   sudo service sidekiq {start,stop,restart,status}
#

description "Sidekiq Background Worker"

# no "start on", we don't want to automatically start
stop on (stopping workers or runlevel [06])

# change to match your deployment user
setuid root
setgid root

respawn
respawn limit 3 30

# TERM and USR1 are sent by sidekiqctl when stopping sidekiq.  Without declaring these as normal exit codes, it just respawns.
normal exit 0 TERM USR1

instance $index

script
# this script runs in /bin/sh by default
# respawn as bash so we can source in rbenv
exec /bin/bash <<EOT
  # use syslog for logging
  # exec &> /dev/kmsg

  # pull in system rbenv
  # export HOME=/home/deploy
  # source /etc/profile.d/rbenv.sh

  cd /var/www/blinky/current
  bundle exec sidekiq -i ${index} -e production
EOT
end script
```


7. Exit out of the RaspberryPi ssh session with `$ exit`

### K. Deploy Blinky

Use Capistrano 3 to deploy application **from your Mac's Terminal**

``` bash
$ cd <where ever you cloned this repo to>
$ bundle exec cap production deploy
```

Don't forget that you will need to `RAILS_ENV=production bundle exec rake db:migrate`


### Backup

Jesus christ, that was a lot of work, to make sure you don't have to do all that again, make a
backup.

* Shutdown your pi with `sudo shutdown -h now`
* Take the SD card out of your Pi and put it back in the card reader attached to your computer
* Open up terminal and type, again takeing care to replace <x> with the correct value from `df -h`

``` bash
dd if=/dev/rdisk<x> of=/path/to/image bs=1m
```

## Thanks / More Info

This guide wouldn't have been possible without the following resources

http://elinux.org/RPi_Ruby

http://creativepsyco.github.io/blog/2013/04/10/deploying-rails-on-nginx-and-thin/

[micro_usb]:http://www.amazon.com/Micro-USB-to-Cable/dp/B004GETLY2/
[sd_card]:http://www.amazon.com/Transcend-Class-Flash-Memory-TS8GSDHC10E/dp/B003VNKNEG/
[card_reader]:http://www.amazon.com/IOGEAR-MicroSD-Reader-Writer-GFR204SD/dp/B0046TJG1U/
[occidentalis]:http://learn.adafruit.com/adafruit-raspberry-pi-educational-linux-distro/occidentalis-v0-dot-2
[raspbian]:http://www.raspberrypi.org/downloads
[ruby-lang-ftp]:ftp://ftp.ruby-lang.org/pub/ruby/
