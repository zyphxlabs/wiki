# Basics

## What Linux Is

Linux is a kernel and not a full operating system. The kernel is the core part that talks to the hardware like the CPU and RAM and hard drive. An operating system or OS is the software I use to make the computer do things. Windows and macOS and Android are all operating systems.

A distribution or distro is an OS built on top of the Linux kernel. Parrot OS and Kali and Ubuntu and Debian and CentOS are all distros. People just say Linux for all of it.

When I run a tool like nmap the OS asks the kernel for hardware resources and the kernel sets them up. So the kernel is the middle man between the app and the hardware.

- App then OS then kernel then hardware

## Why Linux

Linux is open source. That means the code is free to use and change and share and thats why anyone can make a distro. It is also faster and more secure in general. Most websites and servers run on Linux and most hacking tools are made for it.

## GUI and Command Line

A GUI is a graphical user interface with icons and menus. Linux has one but it was not made to be used like that. The real power is in the terminal which is also called the command line.

## Terminal and Shell

The terminal I use is really a terminal emulator. It pretends to be the old physical terminal which was just a keyboard and a screen. The old one was called the VT100.

The shell is the program that actually reads my commands and talks to the operating system. The terminal is the window and the shell is the interface inside it. The most common shell is bash which stands for Bourne Again Shell. Others are zsh and fish and PowerShell also runs on Linux.

To see which shell I am using I run `ps`.

## The Prompt

```
user@hostname:/current/directory$
```

It shows my username and the machine name and where I am. The `$` at the end means I am a normal user. If I am root it shows `#` instead. That is a reminder to be careful because root has full power.

## Getting Help

Nobody remembers every command and every option. Linux has help built in.

```bash
man ls          # the manual. Press q to quit
ls --help       # short list of options. Some commands use -h
apropos usb     # search commands by keyword when I forgot the name
```

## Handy Info Commands

- `whoami` shows my username
- `id` shows my user id and groups
- `hostname` shows the name of the machine
- `uname` shows system info
- `ifconfig` and `ip` show network interfaces and ip addresses
- `netstat` and `ss` show network connections
- `ps` shows running processes
- `who` shows who is logged in
- `env` shows environment variables
- `lsblk` lists disks and blocks
- `lsusb` lists usb devices
- `lsof` lists open files