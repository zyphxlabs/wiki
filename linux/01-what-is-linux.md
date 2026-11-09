# Lecture 1 What Is Linux

To be a hacker I need to know Linux. It is a required foundation and a prerequisite for doing anything in hacking. Most of this series is Linux for everyone and not only hackers. In this first lecture I get a free Linux lab and learn what Linux is and run my first commands.

## Getting a Free Linux Lab

The whole series uses a free lab from Hack The Box Academy. It runs right in the browser.

- Sign up on Hack The Box Academy and skip the intro
- Under my username there are 30 cubes. Cubes unlock courses
- Click Modules then All Modules and scroll to Linux Fundamentals
- It costs 10 cubes and it shows a +10. If I finish the module I get 10 cubes back so it is basically free
- Click Unlock then Start
- Scroll to the very bottom to Your Workstation and click Start Instance
- When it is ready click Interact and it opens in the browser
- Hack The Box calls this machine the Pwnbox. It is my own Linux playground

## What Is This Machine

The machine in my browser is Linux. To be technical it is a distribution of Linux called Parrot OS and it was built for hacking.

An operating system or OS is the software I use to make the computer do things. Windows and macOS and iOS and Android are operating systems. If someone asks what OS I am running the answer here is Parrot OS.

Linux itself is not an operating system. Linux is a kernel and operating systems are built on top of it. These are called distributions or distros. Parrot OS and Kali Linux and Ubuntu and CentOS and Debian are all distros and they all use a version of the Linux kernel.

The kernel has one important job. It talks to the hardware like the CPU and RAM and hard drive. Say I want to run my favorite hacking tool nmap. I click it and Parrot OS asks the Linux kernel for hardware resources. The kernel sets them up. So the kernel is the middle man between the application and the OS and the hardware. To keep it simple people just say Linux and name the flavor like Parrot OS.

## Why Linux

- It is open source which means it is open and free to use and redistribute. That is why people can build distros like Parrot OS
- It is traditionally faster and more secure
- Most websites and servers in the world run on Linux
- Most hacking tools are Linux based so hackers use it

## GUI and Terminal

The desktop with icons and a dock and menus is a GUI which means graphical user interface. Linux has a nice GUI and I can use it but it was not made to be used like that. The real power is in the terminal which is also called the command line. I open it with the terminal icon in the dock.

## Your First Commands

I opened my home directory in the GUI and the terminal at the same time. In the GUI I always see where I am. In the terminal I dont so I use commands to find out.

### pwd

`pwd` means print working directory. It shows where I am.

```bash
pwd
```

It printed the full path of my home directory something like `/home/user89786`. The home directory then my user directory. That is the same place the GUI shows.

### ls

`ls` means list. It lists the contents of the current working directory. The same stuff I see in the GUI shows up like Desktop and Downloads and Templates and Documents.

```bash
ls
```

### cd

`cd` means change directory. After it I put a space and then where I want to go.

```bash
cd Desktop
pwd
ls
```

`pwd` proves I moved into Desktop and `ls` shows what is on my desktop.

### Going Back With cd ..

In the GUI there is a back button. In the terminal I use `cd` and a space and two dots.

```bash
cd ..
pwd
```

That took me from the Desktop back to my home directory. If I keep going back with `cd ..` I move up further. First I reached `/home` and then one more time I reached `/`.

### The Root

A single forward slash `/` is the root of the file system. It is the end of the line and I cant go back further. I can list it too.

```bash
cd ..
cd ..
pwd
ls
```

There are a lot of interesting folders at the root. They are covered in the next lecture.

## Quiz From the Lecture

I am at the root `/`. Which command or commands take me back to my home directory. There are two ways. One is a single command with the full path like `cd /home/user89786`. The other is two commands like `cd home` and then `cd user89786`.

## Recap

- Linux is a kernel and also used as a word for the whole OS. It is faster and more secure and more popular than Windows and macOS
- The terminal is where I use Linux
- `pwd` shows where I am. `ls` shows what is around me. `cd` changes where I am
