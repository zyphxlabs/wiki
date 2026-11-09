# File System

## Big Idea

In Linux everything is a file. Settings are files. Devices like hard drives and printers are files. Even commands are files. The layout of all these files is called the File System Hierarchy or FHS.

## Terms

A binary is a compiled program file that the computer can run. A library is shared code that many programs use. Mounting means attaching a drive so it shows up as a folder.

## Commands Are Files

A command like `ls` is a binary file. I can look inside it with `cat` but it is unreadable. I can also copy it with `sudo cp` and the copy works as the same command. This showed me that commands are just files. Deleting them would break the system.

`which` tells me where a command lives.

```bash
which ls      # /usr/bin/ls
```

## Main Directories

- `/` is the root and the start of everything
- `/bin` holds the essential command binaries like ls and cp and rm and cat
- `/sbin` holds admin commands that only administrators use like adduser
- `/usr/bin` and `/usr/sbin` hold the same kind of commands as bin and sbin and mostly overlap. There is a history about hard drive space. Both are real
- `/usr/local` is where I put binaries I make myself
- `/usr` also has shared libraries and games
- `/boot` has the files the system needs to boot
- `/var` has log files and web application files
- `/tmp` has temporary files that go away after a reboot
- `/lib` has shared library files the system needs
- `/home` is where every user lives
- `/root` is the home of the root user. I need `sudo` to look inside
- `/dev` has devices. `vda` or `sda` is the hard drive. It stands for virtual disk
- `/etc` has configuration files of the system and apps
- `/mnt` is for drives I mount by hand
- `/media` is for drives mounted automatically like a usb stick

## Examples

Network settings on some systems live in `/etc/network/interfaces`. I can read it with `sudo cat`. Running `cat` on the disk in `/dev` dumps unreadable data nonstop. Ctrl+C stops it.
