---
layout: page
title: Linux Kernel Programming - Character device driver 
description: A simple character device driver implemented for Linux System Programming Lab project
img: /assets/img/linux_kernel.png
importance: 3
github: https://github.com/mushfek/system-programming/blob/main/chardev.c
---

#### Theory
In UNIX, hardware devices are accessed by the user through special device files. These files are grouped into the /dev directory, and system calls open, read, write, close, lseek, mmap etc. are redirected by the operating system to the device driver associated with the physical device. The device driver is a kernel component (usually a module) that interacts with a hardware device.

In the UNIX world there are two categories of device files and thus device drivers: character and block. This division is done by the speed, volume and way of organizing the data to be transferred from the device to the system and vice versa. In the first category, there are slow devices, which manage a small amount of data, and access to data does not require frequent seek queries. Examples are devices such as keyboard, mouse, serial ports, sound card, joystick. In general, operations with these devices (read, write) are performed sequentially byte by byte.

This program doesn't implement driver for an actual device rather reads data from a simulated buffer for conceptual purpose.

Source code is available on [Github](https://github.com/mushfek/system-programming/blob/main/chardev.c).