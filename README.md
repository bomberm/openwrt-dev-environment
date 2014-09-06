# PirateBox OpenWRT development environment scripts
This collection and documentation is for developing on scratch on PirateBox-OpenWRT packages, images and so on. All of those commands in the Makefile are customized or assume, that you don't rely on the current stable source.

## What this repository is:
This repository is intended to get you started with a development environment to build your own PirateBox images - no matter if you just want to enable an additional Kernel feature, remaster your PirateBox image or start developing for the PirateBox.

## What this repository is __not__:
* __NO__ newbie guide. You should know your way around in Makefiles. You should know how to configure a Kernel and you should also have some knowledge about OpenWRT. In doubt follow the reference Links.
* __SHOULD NOT__ be used if you only want to create a customized OpenWRT image. If you want to do that, use [openwrt-image-build](http://wiki.openwrt.org/doc/howto/obtain.firmware.generate) instead.

## Todo's
* Improve this README - WIP
* Complete walk-through for piratebox feed and local feed method
* Improve comments in the Makefile, better yet - get rid of them and move all needed information to this README.

## Prerequisites
* Make sure you have the loop kernel module loaded:

        modprobe loop

* Make sure you have at least __8BG__ free disk space

## Setting up the development enviroment
There are two methods to build the image:
* Using the piratebox feed
* Using a custom, local feed

Use the __local feed__ variant if you want to use __other__ branches __than__ the __master__ branch.


### PirateBox feed variant
#### For the impatient
If you just want to build the stable release run:

    make auto_build_stable

This will automated run the steps described below and may take from minutes, to hours, depending on your system. There are some points while making this target where you will be asked for your root password so you have to either sit it out or run it as root.

#### Step by step
To build your PirateBox image execute the following steps in order:
    
1. Clone and configure OpenWRT and clone the image build script
    
        make openwrt_env
Detailed information about the OpenWRT build system may be found in the OpenWRT Wiki:

  * [build system](http://wiki.openwrt.org/doc/howto/buildroot.exigence)
  * [obtaining the source](http://wiki.openwrt.org/doc/howto/buildroot.exigence#downloading.sources)

2. Apply the PirateBox OpenWRT feed 

        make apply_piratebox_feed

3. Update all feeds

        make update_all_feeds

4. Install the PirateBox OpenWRT feed

        make install_piratebox_feed

5. Create the piratebox script image

        make create_piratebox_script_image

6. Build OpenWRT
If you have more than four cores, do not forget to adjust the __CORES__ variable in the Makefile.
This will copy the default kernel config and start building OpenWRT.

        make build_openwrt
The __CORES__ variable in the Makefile needs to be adjusted to your system, a good rule of thumb for the value is to use the amount of cores you have available on your build machine.     
Building the OpenWRT image may take a long time, depending on your machine, up to a couple of hours.

7. Aquire missing packages    
There are a couple of packages that did not make it in the OpenWRT repo yet, so you need to acquire them manually::

        make acquire_packages

8. Start local repository    
After building OpenWRT you can start your local repository:

        make run_repository_all
Now surf to http://localhost:2342 and verify that the repository is up and running.
If you want to change the port of the local repository, set it in the __Makefile__.

9. Build the PirateBox image     
To build the PirateBox image and istall.zip run:

        make piratebox

10. Stop the local repository
After building the image you can stop your local repository:

       make stop_repository_all

11. Enjoy your build     
You should now have a directory called __target_piratebox__.
This directory contains all supported firmware images and the install_piratebox.zip

You can now continue with the [auto installation](http://piratebox.cc/openwrt:diy) step.

### Troubleshooting
#### Builing OpenWRT fails with errors
Run __make__ in the openwrt folder single threaded and with the __S=v__ flag to get detailed output:

    make S=v