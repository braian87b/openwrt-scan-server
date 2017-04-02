Install instructions on OpenWrt
================================

```bash
opkg update
opkg install usbutils
opkg install libsane libusb-compat libpng libjpeg libc

# Add proper repository based on your architecture

# for Atheros ar71xx based routers (Usually TP-Link and others)
cat<<'EOF' >> /etc/opkg/distfeeds.conf
src/gz chaos_calmer_packages_sane http://luizluca.github.io/openwrt/15.05/ar71xx/packages
EOF

# for PogoPlug
cat<<'EOF' >> /etc/opkg/distfeeds.conf
src/gz chaos_calmer_packages_sane http://luizluca.github.io/openwrt/15.05/kirkwood/packages
EOF

# mvebu (some shitty and crappy routers): 
cat<<'EOF' >> /etc/opkg/distfeeds.conf
src/gz chaos_calmer_packages_sane http://luizluca.github.io/openwrt/15.05/mvebu/packages
EOF

# x86 (a never obsolete computer)
cat<<'EOF' >> /etc/opkg/distfeeds.conf
src/gz chaos_calmer_packages_sane http://luizluca.github.io/openwrt/15.05/x86/packages
EOF

cd /tmp
wget http://luizluca.github.io/openwrt/fe8e1e6731dc3f26
opkg-key add fe8e1e6731dc3f26
rm fe8e1e6731dc3f26

opkg update
opkg install hplip-sane # hplip-common libsane libc libusb-compat
opkg install sane-frontends # libsane libjpeg libpng libusb-compat libc

lsusb # Bus 001 Device 002: ID 03f0:7e04 Hewlett-Packard DeskJet F4100 Printer series
sane-find-scanner # found USB scanner (vendor=0x03f0 [HP], product=0x7e04 [Deskjet F4100 series]) at libusb:001:002
scanimage --test # scanimage: scanning image of size 638x877 pixels at 24 bits/pixel
```

### lazy install instructions (you actually just need the scan script in cgi-bin and scan.html)

```bash
# test
opkg install wget unzip
opkg install jpeg-tools # run this only if you need to use cjpeg
cd /tmp

# original repo from zed-0xff
wget --no-check-certificate https://github.com/zed-0xff/openwrt-scan-server/archive/master.zip
# this repo, (improved version)
wget --no-check-certificate https://github.com/braian87b/openwrt-scan-server/archive/master.zip

unzip master.zip; rm master.zip
cd openwrt-scan-server-master
mv index.html scan.html

# In this repo I use `file` over `identify`, actually it says the same information (size of the image in pixels)
# so you will not need to copy this file
cp bin/ar71xx/identify /bin
```

```bash
rm -rf bin
cp -r * /www/
cd /www
rm -rf /tmp/openwrt-scan-server-master
mkdir /www/data

# run this if you dont have `file` preinstalled.
opkg install file # libmagic
```
#### navigate to http://ipaddr/scan.html and you are done!

Many Thanks to Luiz Luca for his contribution! on http://luizluca.github.io/openwrt/
Many Thanks to zed-0xff for his original repository on: https://github.com/zed-0xff/openwrt-scan-server/

