# Workflow compiling Swampcoin on raspberry pi 3 (stretch)

Distributor ID:	Raspbian
Description:	Raspbian GNU/Linux 9.11 (stretch)
Release:	9.11
Codename:	stretch

Linux raspberrypi 4.19.66-v7+ #1253 SMP Thu Aug 15 11:49:46 BST 2019 armv7l GNU/Linux

 * `$ gcc -dumpmachine`
 * `arm-linux-gnueabihf`

## Install deps
```sh
sudo apt-get install autoconf libevent-dev libtool libssl-dev libboost-all-dev libminiupnpc-dev

git clone https://github.com/swampcoin/swamp.git
```

## Make install berkley-db-4.8.30 and signs
```sh
mkdir bin
cd bin
wget http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz
tar -xzvf db-4.8.30.NC.tar.gz
cd db-4.8.30.NC/build_unix/
../dist/configure --enable-cxx
make -j2
sudo make install
cd
wget https://github.com/codablock/bls-signatures/archive/v20181101.zip
unzip v20181101.zip
cd bls-signatures-20181101
ls
cmake .
make install
sudo make install
```

## Make swamp
```sh
cd
cd swamp
./autogen.sh
./configure --prefix `pwd`/depends/arm-linux-gnueabihf CPPFLAGS="-I/usr/local/BerkeleyDB.4.8/include -O2" LDFLAGS="-L/usr/local/BerkeleyDB.4.8/lib" --enable-upnp-default
make -j2
```
