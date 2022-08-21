Installation is in three parts. bladeRF drivers from a specific git commit, yate from Nuand's modified source, yatebts from Nuand's modified source.

# Prerequisites

**Ubuntu 22.04 LTS**

## Repositories

`sudo add-apt-repository ppa:ondrej/php && sudo apt update`

## Packages

`sudo apt install -y build-essential git subversion unzip aiutoconf libusb-1.0-0-dev libgsm1 cmake pkg-config wget apache2 php5.6`

## Build environment

`mkdir ~/build`

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

```
sudo touch /usr/local/etc/yate/snmp_data.conf /usr/local/etc/yate/tmsidata.conf
sudo chown root:www-data /usr/local/etc/yate/*.conf
sudo chmod g=u /usr/local/etc/yate/*.conf
```
