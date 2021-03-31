---
title: "Linux Boot Process "  
author: Seif-Allah
layout: post
description : To Do
category : Linux
image: /assets/images/linux/todo.png
toc: true
---


### Introduction

When you on the power to your linux system, it triggers a series of events that eventually leads to the login prompt. There may be times when your linux systel doesn't boot quite correctly, or perhaps an application that you expected to be running in background mode isn't running. In those situations, it helps to have a basic understanding of how Linux boots the operating system and starts programs so that you can **troubleshoot** the problem.

### Following the Boot Process

The linux boot process can be split into three main steps: 

1. The workstation firmware starts, performing a quick check of the hardware, called a *Power-On-Self Test (POST)*, and then it looks for a bootloader program to run from a bootable device.

2. The bootloader runs and determines what Linux kernel program to load.

3. The kernel program loads into memory and starts the necessary background programs required for the system to operate (such as a graphical desktop manager for desktops or web and database servers for servers.)

These 3 steps might seem so simple on the surface, but a somewhat complicated ballet of operations happens behind the scenes to keep the boot process working. Each step performs several actions as it prepares your system to run Linux.


### Viewing the Boot Process 

You can monitor the Linux boot process by watching the system console screen as the system boots. You'll see lots of informative messages scroll by as the system detects hardware and loads the software.

Usually the boot messages scroll by somewhat quickly, and it's hard to see just what's happening. If you need to troubleshoot boot problems, you can review the boot-time messages using the *dmesg* command. Most Linux distributions copy the boot kernel messages into a special ring buffer in memory called the *kernel ring buffer*. The buffer is circular and set to a predetrmined size. As new messages are logged into the buffer, older messages are rotated out.

The *dmesg* command displays the most recent boot messages that are currently stored in the kernel ring buffer : 

```text
[    0.000000] microcode: microcode updated early to revision 0x2f, date = 2019-11-12
[    0.000000] Linux version 5.4.0-66-generic (buildd@lgw01-amd64-016) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #74~18.04.2-Ubuntu SMP Fri Feb 5 11:17:31 UTC 2021 (Ubuntu 5.4.0-66.74~18.04.2-generic 5.4.86)
[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-5.4.0-66-generic root=UUID=735fb175-3ba7-44e8-9308-b1edab99ff24 ro quiet splash
[    0.000000] KERNEL supported cpus:
[    0.000000]   Intel GenuineIntel
[    0.000000]   AMD AuthenticAMD
[    0.000000]   Hygon HygonGenuine
[    0.000000]   Centaur CentaurHauls
[    0.000000]   zhaoxin   Shanghai  
[    0.000000] x86/fpu: Supporting XSAVE feature 0x001: 'x87 floating point registers'
[    0.000000] x86/fpu: Supporting XSAVE feature 0x002: 'SSE registers'
[    0.000000] x86/fpu: Supporting XSAVE feature 0x004: 'AVX registers'
[    0.000000] x86/fpu: xstate_offset[2]:  576, xstate_sizes[2]:  256
[    0.000000] x86/fpu: Enabled xstate features 0x7, context size is 832 bytes, using 'standard' format.
[    0.000000] BIOS-provided physical RAM map:
[    0.000000] BIOS-e820: [mem 0x0000000000000000-0x000000000009e7ff] usable
[    0.000000] BIOS-e820: [mem 0x000000000009e800-0x000000000009ffff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000000e0000-0x00000000000fffff] reserved
[    0.000000] BIOS-e820: [mem 0x0000000000100000-0x000000008ecb8fff] usable
[    0.000000] BIOS-e820: [mem 0x000000008ecb9000-0x000000008f5b8fff] reserved
[    0.000000] BIOS-e820: [mem 0x000000008f5b9000-0x000000009c9cdfff] usable[    0.000000] microcode: microcode updated early to revision 0x2f, date = 2019-11-12
[    0.000000] Linux version 5.4.0-66-generic (buildd@lgw01-amd64-016) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #74~18.04.2-Ubuntu SMP Fri Feb 5 11:17:31 UTC 2021 (Ubuntu 5.4.0-66.74~18.04.2-generic 5.4.86)
[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-5.4.0-66-generic root=UUID=735fb175-3ba7-44e8-9308-b1edab99ff24 ro quiet splash
[    0.000000] KERNEL supported cpus:
[    0.000000]   Intel GenuineIntel
[    0.000000]   AMD AuthenticAMD
[    0.000000]   Hygon HygonGenuine
[    0.000000]   Centaur CentaurHauls
[    0.000000]   zhaoxin   Shanghai  
[    0.000000] x86/fpu: Supporting XSAVE feature 0x001: 'x87 floating point registers'
[    0.000000] x86/fpu: Supporting XSAVE feature 0x002: 'SSE registers'
[    0.000000] x86/fpu: Supporting XSAVE feature 0x004: 'AVX registers'
[    0.000000] x86/fpu: xstate_offset[2]:  576, xstate_sizes[2]:  256
[    0.000000] x86/fpu: Enabled xstate features 0x7, context size is 832 bytes, using 'standard' format.
[    0.000000] BIOS-provided physical RAM map:
[    0.000000] BIOS-e820: [mem 0x0000000000000000-0x000000000009e7ff] usable
[    0.000000] BIOS-e820: [mem 0x000000000009e800-0x000000000009ffff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000000e0000-0x00000000000fffff] reserved
[    0.000000] BIOS-e820: [mem 0x0000000000100000-0x000000008ecb8fff] usable
[    0.000000] BIOS-e820: [mem 0x000000008ecb9000-0x000000008f5b8fff] reserved
[    0.000000] BIOS-e820: [mem 0x000000008f5b9000-0x000000009c9cdfff] usable
[    0.000000] BIOS-e820: [mem 0x000000009c9ce000-0x000000009cebdfff] reserved
[    0.000000] BIOS-e820: [mem 0x000000009cebe000-0x000000009cfbdfff] ACPI NVS
[    0.000000] BIOS-e820: [mem 0x000000009cfbe000-0x000000009cffdfff] ACPI data
[    0.000000] BIOS-e820: [mem 0x000000009cffe000-0x000000009cffefff] usable
e000-0x000000009cffdfff] ACPI data
[    0.000000] BIOS-e820: [mem 0x000000009cffe000-0x000000009cffefff] usable
```

Most linux distributions also store the boot messages in a log file, usually in the */var/log* folder. For debian-based systems, the file is usually */var/log/boot*.

Although it helps to be able to see the different messages generated during boot time, it is also helpful to know just what generates those messages.


### The Firmware Startup 

All IBM-compatible workstations and servers utilize some type of built-in firmware to control how the installed operating system starts. On older workstations and servers, this firmware was called the *Basic Input/Output System (BIOS)*. On newer workstations and servers, a new method, called the *Unified Extensible Firmware Interface (UEFI) * is responsible for maintaining the system hardware status and launching an installed operating system.

Both methods eventually launch the main operating system program, however, and each method uses different ways of doing that.


### The BIOS Startup

The BIOS firmware found in older workstations and servers was somewhat limited in what it could do. The BIOS firmware had a simplistic menu interface that allowed you to change some settings to control how the system found hardware and define what device the BIOS should use to start the operating system.

One of the limitations of the original BIOS firmware was that it could read only one sector's worth of data from a hard drive into memory in order to run. As you can probably guess, that's not enough space to load an entire operating system. To get around that limitation, most operating systems (including Linux and Microsoft Windows) split the boot process into two parts.

First, the BIOS runs a bootloader program, The *bootloader* is a small program that initializes the necessary hardware to find and run the full operating system, which is usually found at another location on the same hard drive but sometimes situated on  a separate internal or external storage device.

The bootloader program usually has a configuration file, so you can tell it where to find tha actual operating system file to run or even to produce a small menu allowing the user to boot between multiple operating systems.

To get things started, the BIOS must know where to find the bootloader program on 
ab installed storage device. Most BIOS setups allow you yo load the bootloader program from several locations (internal/external hard drive, CD/DVD, etc.. )

When booting from a hard drive, you must designate which hard drive, and which partition on the hard drive, the BIOS should load the bootloader program from. This is done by defining a *Master Boot Record (MBR)*. 

The MBR is the first sector on the first hard drive partition on the system. There is only one MBR for the computer system. The BIOS looks for the MBR and reads the program stored there into memory. Since the bootloader program must fit in one sector, it must be very small, so it can't do much. The bootloader program mainly points to the location of the actual operating system kernel file, which is stored in a boot sector of a separate partition on the system. There are no size limitations on the kernel boot file.

The bootloader program isn't required to point directly to an operating system kernel file, it can point to any type of program including another bootloader program. You can create a primary bootloader program that points to a secondary bootloader program, which provides options to load multiple operating systems. This process is called chainloading.


### The UEFI Startup 

While there were plenty of limitations with BIOS, computer manufactures learned to live with them, and BIOS was the default standard for IBM compatible systems for many years. However, as operating systems became more complicated, it eventually became clear that a new boot method needed to be developed. 

Intel created the *Extensible Firmware Interface (EFI)* in 1998 to address some of the limitations of  BIOS. The adoptions of EFI was somewhat of a slow process, but by 2005 the idea caught on with other vendors, and the Universal EFI (UEFI) specification was adopted as a standard. These days just about all IBM-compatible desktop and server systems utilize the UEFI firmware standard.

Instead of relying on a single boot sector on a hard drive to hold the bootloader program, UEFI specifies a special disk partition, called the EFI system partition (ESP) to store bootloader programs. This allows for any size of bootloader program, plus the ability to store multiple bootloader programs for multiple operating systems.

The ESP setup utilizes the old Microsoft File Allocation Table (FAT) filesystem to store the bootloader programs. On Linux Systems, the ESP is typically mounted in the */boot/efi* folder, and the bootloader files are typically stored using the *.efi* filename extension.

The UEFI firmware utilizes a built-in mini bootloader (sometimes referred to as a boot manager), which allows you to configure just which bootloader program file to launch. 

> Not all linux distributions support the UEFI firmware, it you're using a UEFI system, make sure that the linux distribution you select supports it.

With UEFI, you need to register each individual bootloader file that you want to appear at boot time in the boot manager interface menu. The *efibootmgr* Linux application allows you to create and remove boot entries or change the boot order. The UEFI interface includes a shell environment, allowing you to enter commands to alter boot settings, or select the bootloader to run each time you boot the system. 

Once the firmware finds and runs the bootloader, its job is done.

