---
title: "Regularly used QEMU invocations"
date: 2020-07-18T14:34:54+02:00
draft: false
toc: false
images:
tags: 
  - linux
  - qemu
---
This post is nothing more than a quick reference of QEMU commands for different scenarios.

## Virtual Machine Image Creation

A command to create an image:

    qemu-img create -f qcow2 -o nocow=on TestClient.img 64G

The `-o nocow=on` option is valid on Btrfs only and shall avoid degraded performance.

## Installing and Running the Guest Operating System

The image can then be installed and run with:

    qemu-system-x86_64 -cdrom Downloads/debian-10.4.0-amd64-netinst.iso -enable-kvm -hda TestClient.img -k de -m 4096 -name 'Test Server'

The `-cdrom` part is only passed on installation. `-enable-kvm` greatly improves performance, `-k de` set the keyboard layout, `-m 4096` the memory size and the `-name 'Test Server'` adds a window title which helps telling apart multiple virtual machines.

## Sharing a Directory between the Host and the Guest Operating System

To share a directory between the host and the guest operating systems the following argument has to be added to the QEMU invocation (the user should be in the `qemu` group to run the command with elevated privileges):

    -virtfs local,path=/home/maprasser/Downloads,mount_tag=VMTransfer,security_model=none

The shared directory can be mounted within the virtual machine with the following command (the `/mnt/vm_transfer` mount point has to exist already):

    mount -t 9p -o trans=virtio VMTransfer /mnt/vm_transfer
