---
title: "A praise of conservative dependency handling"
date: 2023-04-29T18:46:11+02:00
draft: false
toc: false
images:
tags: 
  - c
  - linux
  - python
---
Currently I work on porting [GnoTime](https://gttr.sourceforge.net) from deprecated dependencies on _Gnome 2_ into the modern era. The last "feature commit" before the refactoring was in 2013 - so the dependencies are quite ancient. Likewise the build system is good old _GNU Autotools_. So apart of upgrading the dependencies porting the build to a more modern build system would be a worthwhile effort. For my own projects I try to keep the dependencies compatible to current _Debian oldstable_, but during refactoring I prefer to use the most recent tooling available.

One major challenge in this process is that the code does not build on up-to-date Linux distributions. Out of the box it compiled only on _Ubuntu 12.04 (Precise Pangolin)_ and with a minor (linkage) fix on _Ubuntu 14.04 (Trusty Tahr)_ too. So the question was if _CMake_ and _Meson_ could be made working on these ancient distributions. I cloned their repositories and set off to try it out.

Current _Meson_ `1.1.0` requires at least _Python_ `3.8` and _Ninja_ `1.8.2` or newer. Amazingly it was possible to compile and install current _Python_ `3.11.3` and _Ninja_ `1.11.1` without issues on _Ubuntu 14.04 (Trusty Tahr)_. With these it was possible to build _GnoTime_ with _Meson_ without issues (the _Meson_ branch is not upstreamed to _GnoTime_ yet).

For convenient integration with modern IDEs a _CMake_ file has been added to the _GnoTime_ project already. _CMake_ is not intended as the future build system though, since _Meson_ seems to be more prevalent in the ecosystem of _Gnome_. _CMake_ `3.26.3` could be built and installed on _Ubuntu 14.04 (Trusty Tahr)_ without issues too. Both _CMake_ and _Meson_ succeeded to compile a working binary of _GnoTime_ in their current versions! So both can be executed in their current releases on a nearly ten year old system (although _Meson_ unfairly profits from the superb dependency handling of _Python_)!!

A different level will be _Ubuntu 12.04 (Precise Pangolin)_. Will both work there too? _Python_ `3.11.1` fails to compile due to `-std=c11` being an unknown command line option for `cc`on _Ubuntu 12.04 (Precise Pangolin)_. Trying the `3.10` branch with `3.10.11` was successful. This is still very impressive since _Python_ `3.10.0` was released in October of 2021, so more than nice years after _Ubuntu 12.04 (Precise Pangolin)_ got released! _Ninja_ `1.11.1` built without issues on _Ubuntu 12.04 (Precise Pangolin)_ - so big respect to its developers too! The last tool missing is _CMake_. All recent _CMake_ versions fail to build, due to insufficient support of modern C++ in _Ubuntu 12.04 (Precise Pangolin)_. The most recent buildable version of _CMake_ is `3.9.6`. On _Ubuntu 12.04 (Precise Pangolin)_ again both _CMake_ (`3.9.6`) and _Meson_ (`1.1.0`) succeeded to build _GnoTime_. _Meson_ produces a working binary, while _CMake_ fails during linking.

What's the bottom line? In my early C++ days I tried every new latest and greatest feature. Now after a few years of programming in a professional context and dealing with all kinds of legacy code and toolchains I came to cherish a more conservative approach. It's more friendly to users who are bound to older hardware and toolchains and like in the case of trying to move _GnoTime_ to a more recent build system while trying not to break compatibility with well seasoned _Ubuntu_ releases, it really is a game changer.

So thanks and respect again to the devs of _Python_ and _Ninja_!
