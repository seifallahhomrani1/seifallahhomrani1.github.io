---
title: "Linux Bootloaders"  
author: Seif-Allah
layout: post
description : To Do
category : Linux
image: /assets/images/linux/todo.png
toc : true
---


### Linux Bootloaders

The bootloader program helps bridge the gap between the system firmware and the full Linux operating system kernel. In Linux, you have many choices of bootloaders. The most popular ones that you'll run across are these : 
* Linux Loader (LILO)
* Grand Unified Bootloader (GRUB) Legacy
* GRUB2

In the original versions of Linux, the Linux Loader (LILO) bootloader was the only one available. It was extremely limited in what it could do, but it accomplished its purpose, that is, loading the Linux kernel from the BIOS startup. LILO became the default bootloader used by Linux distributions in the 1990s. The LILO configuration file is stored in a single file, UEFI, */etc/lilo.conf*, which defines the systems to boot. Unfortunately, LILO doesn't work with UEFI systems, so it has limited use on modern systems and is quickly fading into history.

The first version of the GRUB bootloader (now called GRUB Legacy) was created in 1999 to provide a more robust and configurable bootloader to replace LILO. GRUB quickly became the default bootloader for all linux distributions, whether they run on BIOS or UEFI systems.

GRUB2 was created in 2005 as a total rewrite of the GRUB Legacy system. It supports advanced features, such as the ability to load hardware driver modules and use logic statements to alter the boot menu options dynamically, depending on conditions detected on the system (such as if an external hard drive is connected).



### GRUB Legacy 

GRUB Legacy allows you to select multiple kernels and /or operating systems using a menu interface as well as an interactive shell. You configure the menu interface to provide options for each kernel or operating system you wish to boot. The interactive shell provides a way for you to customize boot commands on the fly. 

Both the menu and the interactive shell utilize a set of commands that control features of the bootloader. 

### Configuring GRUB Legacy 

When you use the GRUB Legacy interactive menu, you need to tell it what options to show. You do that using special GRUB menu commands.
The GRUB Legacy system stores the menu commands in a standard text configuration file. The configuration file used by GRUB Legacy is *menu.lst*, and it is stored in the */boot/grub* folder. (While not a requirement, some Linux Distributions create a separate */boot* partition on the hard drive.) Red Hat-derived Linux distributions (such as CentOS and Fedora) use *grub.conf* for the configuration file.

The GRUB Legacy configuration file consists of two sections:

* Global definitions
* Operating system boot definitions

The global definitions section defines commands that control the overall operation of the GRUB Legacy boot menu. The global definitions must appear first in the configuration file. There are only handful of global settings. 

### GRUB 2
Since the GRUB2 system was intended as an improvement over GRUB Legacy, many of the features are the same, with just a few twists. For example, the GRUB2 system changes the configuration filename to *grub.cfg*, and it stores it in the */boot/grub/* folder. (This allows you to have both GRUB Legacy and GRUB2 installed at the same time.)

### Configuring GRUB2

There are also a few changes to the commands used in GRUB2. FOr example, instead of the *title* command, GRUB uses the *menuentry* command, and you must also enclose each individual boot section within braces immediately following the *menuentry* command.
Here's an example of a GRUB2 configuration file:

```bash
menuentry "CentOS Linux"{
    set root=(hd1,1)
    linux /boot/vmlinuz
    initrd /initrd
}

menuentry "Windows" {
    set root=(hd0,1)
}

```

The configuration process for GRUB2 is also somewhat different. While GRUB2 uses the */boot/grub/grub.cfg* file as the configuration file, you should never modify that file. Instead, there are seperate configuration files stored in the */etc/grub.d* folder. This allows you (or the system) to create individual configuration files for each boot option installed on your system (for example, one configurationo file for booting Linux and another for booting Windows).

### Installing GRUB2

Unlike GRUB Legacy, you don't need t install GRUB2. All you need to do is to rebuild the main installation file by running the *grub-mkconfig* program to the *grub.cfg* configuration file or use the *-o* option to specify the output file. By default, the *grub-mkconfig* program just outputs the new configuration file commands to standard output.


### Alternative Bootloaders 

While GRUB Legacy and GRUB2 are the most popular Linux bootloader programs, you may run into a few others, depending on which Linux distributions you are using.

The *Systemd-boot* bootloader program is starting to gain popularity in Linux Distributions that use the systemd init method. 