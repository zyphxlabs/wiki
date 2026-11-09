# Lecture 3 Getting Help

Linux can be a beast. There are so many commands and switches and ways to use them. This lecture covers how to get help right in the terminal. It also covers what the terminal and shell really are and some new commands. I start the lab like before and open the terminal.

## Terminal Emulator

The terminal I use is not a real terminal. It is a terminal emulator. It pretends to be one. A real terminal is an old physical device like the VT100. That was the keyboard and the screen and monitor used to talk to the computer. Now the terminal emulator is a nice GUI window that pretends to be that keyboard and monitor.

## The Shell

The terminal has one job and that is to give me access to the commands. But the commands are not the terminal. The shell is the user interface I use to talk to the operating system and the Linux kernel. The terminal emulator is what I use to talk to the shell. I picture the terminal as the physical keyboard and monitor and the shell as the UI.

There are different shells. The one I use is bash which stands for Bourne Again Shell. It is by far the most common. To see which shell I am using I use `ps`. `ps` stands for process status and it lists the processes running in my box.

```bash
ps
```

One of the processes is bash. Others I could see are zsh and fish. In the lab there is a blue icon for PowerShell. PowerShell is from Microsoft and it runs on Linux too. When I open it and run `ps` the process shows as `pwsh`.

## What to Call It

People say the terminal and bash and the shell and console. They use all these terms for the same thing. To be fancy I say I am using the bash shell in a terminal emulator.

## The Prompt

When I open the shell there is already text there.

```
user@hostname:~$
```

- The first part is my user account
- After the `@` is the host name of the machine
- Then comes where I am in the file system. It is the same thing `pwd` shows. `~` means home
- The Hack The Box prompt has extra stuff because it was customized
- The `$` means I am logged in as a normal user
- If I am the root user the prompt shows `#` instead. It is a handy reminder because root has godlike power so I must be careful. In the lab I cant log in as root but on another box I switched to root and the prompt changed to `#`

## Commands to Know My System

These commands tell me about the system. I went through them fast in the lecture.

- `id` tells me about my user
- `hostname` shows the host name
- `uname` tells me the system name. Alone it just says Linux but with options it tells a lot like `uname -a`
- `ifconfig` shows network stuff
- `ip` shows more network stuff with lots of options
- `netstat` shows the status of my network
- `ss` shows socket sessions
- `ps` shows processes
- `who` is like `whoami` but shows who else is logged in
- `env` shows environment variables
- `lsblk` lists block devices which means hard drive stuff
- `lsusb` lists USB devices
- `lsof` lists all the open files

I will forget them by next week so I need help.

## man

`man` is the manual. I type `man` and then the command.

```bash
man uname
```

It shows the detailed manual with the description and all the switches and how to use it. It is long. I press `q` to quit. It works for any command like `man ls` or `man ip`.

## Help Switch

Most commands have a short help. I use `-h` or if that doesnt work `--help`.

```bash
uname --help
```

It doesnt print a whole novel. It just shows the switches I can use and how to use the command. Sometimes I want the novel with `man` and sometimes the short list.

## apropos

What if I dont remember the command. There is a command for that called `apropos`. I give it what I want to do as a keyword. It searches through the commands and documentation.

```bash
apropos usb
apropos compress
```

The first one shows `lsusb` at the top. The second shows stuff for file compression.

## Homework From the Lecture

- Practice `man` and `-h` and `--help` and `apropos` on the commands I listed
- The most important thing to learn in Linux is how to learn for myself. I will always hit tools I dont know
- The Hack The Box module has a quiz at the bottom. It is tough and I have to use man. Getting it right earns cubes back
- There was also an extra quiz in the video description
