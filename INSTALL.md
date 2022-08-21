Installation is in three parts. bladeRF drivers from a specific git commit, yate from Nuand's modified source, yatebts from Nuand's modified source.

# Prerequisites

**These instructions were written with Ubuntu 22.04 LTS in mind**

## Repositories

`sudo add-apt-repository ppa:ondrej/php && sudo apt update`

## Packages

`sudo apt install -y build-essential git subversion unzip aiutoconf libusb-1.0-0-dev libgsm1 cmake pkg-config wget apache2 php5.6`

## Build environment

`mkdir ~/build`

## bladeRF firmware

1.9.1 works best with Yate

`cd ~/build/ && wget https://www.nuand.com/fx3/bladeRF_fw_v1.9.1.img`

# Fetch sources

## bladeRF

Fetch the repository and reset to this commit before building.

```
cd ~/build/
git clone https://github.com/Nuand/bladeRF.git ./bladeRF
cd ./bladeRF
git reset --hard 3a411c87c2416dc68030d5823d73ebf3f797a145
```

## yate and yatebts

```
cd ~/build/
wget https://nuand.com/downloads/yate-rc-3.tar.gz
tar xvr yate-rc-3.tar.gz
```

# Building/installing

## bladeRF

Create the build directories and prepare the build files.

```
cd ~/build/bladeRF/host
mkdir build
cd build/
$ cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local -DINSTALL_UDEV_RULES=ON ../
```

Build.

```
make
sudo make install
sudo ldconfig
```

## yate

```
cd ~/build/yate/
./autogen.sh
./configure --prefix=/usr/local
make
sudo make install-noapi
sudo ldconfig
```

## yatebts

```
cd ~/build/yatebts/
./autogen.sh
./configure --prefix=/usr/local
make
sudo make install
sudo ldconfig
```

# Post install configuration

## Give config group ownership to Yate NIB

Network In a Box will allow you to maniupluate the config through a web GUI. Prepare the files in advance and give correct ownership.

```
sudo touch /usr/local/etc/yate/snmp_data.conf /usr/local/etc/yate/tmsidata.conf
sudo chown root:www-data /usr/local/etc/yate/*.conf
sudo chmod g=u /usr/local/etc/yate/*.conf
```

## Configure apache to serve network in a box page

`ln -sfT /usr/local/share/yate/nipc_web/ /var/www/html/nib`

## Flash firmware 1.9.1 to bladeRF

```
cd ~/build/
bladeRF-cli -f bladeRF_fw_v1.9.1.img
```

Once this has completed, unplug and replug the bladeRF.

# Hashes and mirrors

All hashes are sha256

Nuand bladeRF Firmware 1.9.1

`ab32b890f3dc5d353c802dbcad0af78770ef77f2542792875e9177c2e5445124  bladeRF_fw_v1.9.1.img`

Mirror: https://com.xen.sh/mirror/bladeRF_fw_v1.9.1.img

Nuand modified Yate source release candidate 3

`daa876d15296b955cd90ade71fbb86789ffeaa2cc61801da93b6739ae8dc0f97  yate-rc-3.tar.gz`

Mirror: https://com.xen.sh/mirror/yate-rc-3.tar.gz

Personal zip of commit 3a411c87c2416dc68030d5823d73ebf3f797a145 of bladeRF git repo

`4796f711a04f0df296ab3839276d71b65337486a7cb753dae06566f77ae2a860  bladeRF-3a411c87c2416dc68030d5823d73ebf3f797a145.tar.gz`

Mirror: https://com.xen.sh/mirror/bladeRF-3a411c87c2416dc68030d5823d73ebf3f797a145.tar.gz
