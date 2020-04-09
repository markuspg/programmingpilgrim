---
title: "Building Android Pie on Debian Buster"
date: 2020-04-09T17:24:56+02:00
draft: false
toc: false
images:
tags: 
  - android
  - aosp
  - debian
---
Until now I was falling back to _Debian 9 "Stretch"_ for compiling the _Android Open Source Project_.
This was due to the fact that compiling _Android 8.1 "Oreo"_ failed to compile on _Debian 10 "Buster"_ for me.

My most recent project is based on _Android 9 "Pie"_ and for this reason I wanted to give _Debian Buster_ a new shot.
And indeed - it worked!
I conducted a network install with the netinst installation medium installing only the base utilities as additional components in the task selector.
After the installation I manually added the packages `build-essential`, `git`, `gnupg`, `libncurses5`, `libtinfo5`, `m4`, `rsync` and `zip` (my methodology was rather low-tech: waiting for build errors to occur and installing the package responsible for the build error).
This seems to be the baseline package selection required to sucessfully build _Andoid 9 "Pie"_.
