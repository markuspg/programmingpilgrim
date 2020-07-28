---
title: "Flashing with uuu from within a QEMU Hosted Linux VM"
date: 2020-07-28T22:19:37+02:00
draft: false
toc: false
images:
tags: 
  - debian
  - i.mx8
  - nxp
  - proxmox
  - qemu
  - ubuntu
  - uuu
---
For my current work project I have to integrate flashing a _NXP i.MX8_ from a virtual machine running _Ubuntu Focal Fossa (20.04)_ on top of a _QEMU_ hypervisor on a _Proxmox 6.1_ host. The flash process is done with the [uuu tool](https://github.com/NXPmicro/mfgtools) from _NXPmicro_.

Unfortunately the initial flashing attempts were not successful. After quite a few tries a pattern crystallized. The very first flashing attempt after rebooting the VM and resetting the i.MX8 worked, on any further attempt the Proxmox host reset its USB and this then seemed to mess up the virtualized Ubuntu's USB. I tried another Linux (_Fedora_) as host and guest operating system for variety and both constellations did not improve the situation.

As a last resort a [PCIe USB expansion card](https://www.amazon.de/BEYIMEI-Superspeed-3-0-Erweiterungskarte-5-15-poligem-SATA-Stromanschluss/dp/B0855QY46F/ref=sr_1_4?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=beyimei+usb+pci+express&qid=1595968071&sr=8-4) was installed into the test rig. Now instead of forwarding the individual USB devices the PCIe USB expansion card was forwarded to the virtualized Ubuntu to let it handle its USB without interference of the host operating system.

This has been working reliably for now and greatly helps our CI.
