---
title: "Compiling A More Recent Python Version On Debian Stretch"
date: 2020-03-27T13:26:29+02:00
draft: false
toc: false
images:
tags: 
  - android
  - aosp
  - debian
  - python
---
For a current work project I'm again dealing with building and modifying the _Android Open Source Project_.
As usual my operating system of choice for that task is _Debian Stretch_ (9.12).

Since my last Android builds Google's `repo` tool for organizing the multitude of repositories that are needed for an Android build got finally updated from _Python 2_ to _Python 3_ (and that seemingly straight to _Python 3.6_).

Therefore on every invocation of `repo init` I got a complaint about an incompatible Python version being used (it still worked, but who does not like resolving error messages?).
Fortunately Python makes it really easy to have multiple versions installed in parallel as explained in the following.

First the _cpython_ repository has to be cloned and to be changed into.

    git clone https://github.com/python/cpython.git
    cd cpython

In this example version `3.7.7` of the _cpython_ Python implementation shall be build (this is the most recent one as of writing).
_cpython_ can be checked out like this:

    git checkout v3.7.7

All configuration options for the build can be shown by the `--help` argument for _configure_.

    ./configure --help | less

_cpython_ offers the possibility to rely on the operating system's libraries instead of those being included with the sources of _cpython_.
These can be installed on Debian through `apt`.

    apt install libexpat1-dev libffi-dev libmpdec-dev

Afterwards the build can be configured, executed and installed with these commands:

    ./configure --enable-optimizations --with-system-expat  --with-system-ffi --with-system-libmpdec
    make
    sudo make altinstall

`make altinstall` installs the _cpython_ Python interpreter with a version suffix to not collide with the default system one.
To use specifically the newly installed _cpython_ interpreter a virtual environment is created and then activated before running the commands that require a newer Python version.
