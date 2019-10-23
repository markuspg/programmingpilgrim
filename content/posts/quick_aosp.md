---
title: "Quick AOSP Build"
date: 2019-10-19T20:46:54+02:00
draft: false
toc: false
images:
tags: 
  - android
  - debian
---
This is an attempt at trying out how fast AOSP can be built for a _Nexus 5X_ from scratch including setting up a virtual machine with _Debian Stretch_ in which the build will be done.
For virtualization _VMware Workstation 15 Player_ was used on a _Windows 10 v1809_ running on a _DELL_ laptop with an _Intel Core i9-8950HK_ and 32 GB RAM (eight virtual cores and 16 GB of RAM were allocated to the VM).

Start: ~19:30

First _Debian Stretch_ was set up as default network install including the _Gnome_ desktop environment.
As a baseline of development packages install the following ones:

    sudo apt install bison build-essential curl flex g++-multilib gcc-multilib git gnupg gperf lib32z-dev lib32ncurses5-dev libc6-dev-i386 libgl1-mesa-dev libx11-dev libxml2-utils m4 make openjdk-8-jdk open-vm-tools open-vm-tools-desktop unzip vim x11proto-core-dev xsltproc zip zlib1g-dev

Create a `bin` directory in your home directory and add it to your `PATH`.

    mkdir ~/bin
    PATH=~/bin:$PATH

Google developed an own tool for handling the multitude of _Git_ repositories which all combined make up the Android sources. This tool is called `repo` and can be installed to your `bin` directory with the following commands:

    curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    chmod a+x ~/bin/repo

Check if `sha256sum ~/bin/repo` equals `d06f33115aea44e583c8669375b35aad397176a411de3461897444d247b6c220` (as of _repo_ 1.25) and make the `repo` binary executable.

    chmod a+x ~/bin/repo

Create a working directory for all the _Android_ sources and change into it:

    mkdir ~/aosp
    cd ~/aosp

Configure your _Git_ user:

    git config --global user.name "Your Name"
    git config --global user.email "you@example.com"

Initialize the working directory (without the `-b android-8.1.0_r46` you get Android's current master):

    repo init -u https://android.googlesource.com/platform/manifest -b android-8.1.0_r46
    repo sync

For finding the correct device driver binaries the tag has to be mapped to the correct build through [this table](https://source.android.com/setup/start/build-numbers#source-code-tags-and-builds). `android-8.1.0_r46` is mapped to `OPM6.171019.030.K1`.

The driver binaries for Nexus and Pixel devices can be obtained [here](https://developers.google.com/android/drivers).
Download both files for the _Nexus 5X_ and the build `OPM6.171019.030.K1` and verify their checksums.

These self extracting scripts can be run as follows:

    cp -v ~/Downloads/extract-* .
    ./extract-lge-bullhead.sh
    ./extract-qcom-bullhead.sh

The actual build is started with these commands (the appended `aplay` command is only for an audible notification when the build is done):

    source build/envsetup.sh
    lunch aosp_bullhead-userdebug
    m -j12 && paplay /usr/share/sounds/freedesktop/stereo/complete.oga

To be able to access the phone copy [this](https://raw.githubusercontent.com/M0Rf30/android-udev-rules/master/51-android.rules) rules file to `/etc/udev/rules.d/`.

If your _Nexus 5X_ has not been unlocked yet, you have to do this step first (Google is your friend).
Last you can flash the built Android system by running:

    fastboot flashall -w

Finish: 00:10

4:40 for building a custom Android image (not my first try though).
I'm most impressed with the performance of my laptop, especially since I lost about 45 minutes fiddling with a problem caused by packages I had forgotten to install ;-)
