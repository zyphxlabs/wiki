# Lecture 2 The File System

This lecture goes deeper. I explore the Linux file system and learn many new commands. I use the same free lab from lecture 1. I log in to Hack The Box Academy and open the Linux Fundamentals module and start the instance at the bottom and click Interact. Then I open the terminal.

## Starting Point

First I check who I am.

```bash
whoami
```

It prints the user I am logged in as. Then I go to the root of the file system with `cd ..` over and over. Then I clear the screen.

```bash
clear
```

`clear` clears the terminal like a dry erase board. The shortcut is Ctrl+L. Then I list the root with `ls` and see a bunch of folders.

## Everything Is a File

This is the big idea to understand. Everything in Linux is a file.

- Configuration like network settings and the IP address is a file
- Devices like hard drives and printers and CD drives are files
- The commands I use are also files. `ls` and all the commands I learned are files

## Commands Live in bin

Let me prove it. The `/bin` directory holds the command binaries. A binary is a compiled program file. bin stands for binaries. These are the essential command binaries.

```bash
cd /bin
ls
```

A lot shows up. I scroll up and find `ls` in the list. The command I use to list files is itself a file. If it is a file I can look inside it. The command for that is `cat`. It stands for concatenate. It takes a file and puts it all on the terminal.

```bash
cat ls
```

It is unreadable junk because it is the command binary. This is what tells the computer what `ls` does.

## Copying and Deleting a Command

To prove it more I copy the command and delete it and bring it back. `cp` means copy.

```bash
cp ls networkchuck
```

It says permission denied because I dont have permission to write in this directory. I need to say please. For one command I put `sudo` before it. sudo means super user do. It lets me pretend to be the root or admin user for that one command.

```bash
sudo cp ls networkchuck
networkchuck
```

It worked. I copied the binary of `ls` into a new file and the new file does the same thing as `ls`. Next `rm` which means remove.

```bash
sudo rm ls
ls
```

`ls` is gone. The new `networkchuck` command still works the same. To put it back I copy it again.

```bash
sudo cp networkchuck ls
ls
```

`ls` is back. Since `cat` is also a command file I can even run `cat cat`.

## sbin

Next to `bin` there is `/sbin`. I call it super bin. It holds special commands that only administrators use to administer the system. One of them is `adduser`.

```bash
cd /sbin
ls
sudo adduser superbin
```

It asks for a password and a full name. I skip the rest. The user superbin is now created. Users are covered in lecture 4.

## usr

Inside `/usr` there is also a `bin` and an `sbin`.

```bash
cd /usr
ls
cd bin
ls
cd ..
cd sbin
ls
```

The same kind of commands are in both. Both locations are real. There is a history for why there are two and it comes down to hard drive space. The bin and sbin in `/usr` are the same idea as the ones in the root. The ones in `/usr` usually have more commands and there is a lot of overlap.

To see which one is used I use `which`.

```bash
which ls
which cat
which networkchuck
```

`ls` and `cat` are in `/usr/bin`. `which` finds where a command binary lives. It also found the command I made earlier.

In `/usr` there is also a `local` directory. It works like bin and stores command binaries but this is where I put the command binaries I create myself. `/usr` also has libraries that the binaries share and apparently games.

## Speed Round of the Other Directories

- `/boot` has the files my system needs to boot
- `/var` has log files and also web application files
- `/tmp` has temporary files that go away after a reboot
- `/lib` has more shared library files. Specifically things the system needs to boot

## home

`/home` is where I live. Every user lives here.

```bash
cd /home
ls
```

I see two homes. One is mine and one is superbin because I created him. I can go inside his folder and `ls` to see his stuff.

## root

Every Linux system has a root user. He is not in `/home`. He lives alone in `/root`. To look inside I need sudo.

```bash
sudo ls /root
```

## dev

Devices are files too. They live in `/dev` which stands for devices.

```bash
cd /dev
ls
```

A lot of weird stuff is there. I look at `vda` and `vda1`. These are the hard drives. They stand for virtual disks. On other systems I might see `sda` and `sda1`. It is a file so I can cat it.

```bash
sudo cat vda
```

It blows up and keeps going with binary gibberish. I press Ctrl+C and it stops. That is all the data of the hard drive and it is just a file.

## etc

Settings of the system are files too. They are in `/etc`. It is officially etcetera and some people say it etc but most Linux people say etsy. I go here a lot because configuration files of the system and some applications are here.

```bash
cd /etc
ls
cd network
ls
sudo cat interfaces
```

The `interfaces` file has my network interfaces and the IP addresses assigned to them. This is where they are configured. To change network settings I often change this file. Some Linux systems do it differently nowadays but this is still the way on some.

## mnt and media

Both mount drives. A USB flash drive plugged into the Linux box is mounted automatically into `/media` as a file. Mounting means attaching a drive so it shows up as a folder. `/media` is mainly for the system doing it for me. `/mnt` also has drives mounted to it but they are the ones I mount manually with commands. They are shown as a file in `/mnt` too.

## FHS

The way all of this is arranged is called the FHS. It stands for the File System Hierarchy. When I mess with apps or configure things or hack it is important to know where stuff is and where stuff should go.

## Recap

- Everything in Linux is a file
- New commands this lecture were `whoami` and `cp` and `rm` and `cat` and `adduser` and `which`
- I also used `clear` and `sudo`

## Homework From the Lecture

- Make flashcards of all these commands and commit them to memory
- Practice them. If I forget who I am I type `whoami`. Copy things and remove things. Be careful but in the lab I can just restart it
- Practice `which cat` and `cat cat` and all kinds of stuff. The more I practice the faster I learn
- There was also a quiz in the video description
