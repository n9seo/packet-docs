# packet-docs
N9SEO's Packet Docs

[EasyTerm](easyterm/README.md)

[BPQ AGW KISSTNC Emulator](BPQ-AGW-KISS-Emulator/README.md)


##Linux(URONODE) - Incomplete

###Background
Todo

####Linux dist os notes here
Install 32 bit Devuan GNU/Linux 2.1 (ascii)

####Required Apt Packages

* config-package-dev
* dh-exec
* dh-autoreconf
* devscripts
* dpkg-dev
* git
* zlib1g-dev
* libncursesw5-dev

#### Clone Repo
```test
$ mkdir build 
$ cd build
$ git clone https://github.com/ve7fet/linuxax25.git
```

#### Build 

We have to build the libax25 library dependancy first then install it.
Then we can build the ax25apps and ax25tools.

```text
cd linuxax25/libax25
debuild -us -uc
cd ..
sudo dpkg -i libax25_1.1.3_i386.deb
sudo dpkg -i libax25-dev_1.1.3_i386.deb
cd ../ax25tools
debuild -us -uc
cd ../ax25tools/
debuild -us -uc
```

#### Install

```shell script
cd ..
sudo dpkg -i ax25apps_2.0.1_i386.deb ax25tools_1.0.5_i386.deb
```

#### Setup

* todo