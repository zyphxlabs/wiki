# Lecture 6 Services and Daemons

Linux has daemons in the background. They are the reason the system runs. This lecture is about finding them and managing services. It covers how to start and stop and restart and enable things like ssh and web servers. I start the lab and open the Service and Process Management module and launch the terminal.

## Processes First

A process is an instance of a running program. I opened the Sublime text editor from the top bar. That started a process. I can see it.

```bash
ps aux | grep sublime
```

Two processes show up for it. When I closed Sublime and ran the same command they were gone. It is the same for nano. I opened a terminal and typed `nano daemons` and from another terminal ran `ps aux | grep nano`. It even shows the file I have open. When I close nano the process is gone. So whenever I open a program a process starts and closing it ends the process.

These are interactive processes. I had to start them. When I run `ps aux` alone there are a lot of processes I did not start. Many of them are daemons.

## Daemons

Daemons are processes I dont start and dont touch. They do their thing by themselves. They are like services. Windows users know services like the print spooler. Linux has them too and they are often called daemons. They run things like networking and printing and ssh. Without them I couldn't do anything on Linux. They hide in the background.

A daemon looks like a normal process. To identify one the name ends with a `d`. For example the ssh daemon is `sshd`.

```bash
ps aux | grep ssh
ps aux | grep ntp
```

I found `sshd`. It makes sure that when I connect remotely with ssh it works. It starts as soon as the system boots. I also found `ntpd`. NTP is network time protocol and it keeps the time in sync.

The word daemon comes from Greek mythology. In that view it is a supernatural being with no bias for good or evil. It just works in the background. It can be pronounced demon.

## systemd

I control the daemons through the master daemon. His name is `systemd`. He starts and stops and restarts them. He has two jobs. First he is the service manager. Second and more important he is the init system or initialization system. It is vital for booting. When I boot Linux it loads the kernel. Then the kernel hands over to systemd. He mounts the file systems and starts all the services.

He is the first daemon and the first process started. From him all other processes start by forking. Forking means a process starts child processes like a pitchfork. I can see it.

```bash
pstree
```

`pstree` means process tree. At the top is systemd and all the processes branch out from him. His process id or PID is 1. In `ps aux` each process has a PID and he is number 1 at the top.

systemd is not the only init system. Older ones were sysv init and upstart which Ubuntu had and openrc. But systemd is the main one on modern distros.

## systemctl

The tool I use to control the daemons is `systemctl`. systemd calls daemons units. So the word unit means a daemon or service. I played with `sshd`.

```bash
sudo systemctl stop sshd
sudo systemctl status sshd
sudo systemctl start sshd
sudo systemctl restart sshd
sudo systemctl reload sshd
sudo systemctl reload-or-restart sshd
```

- Careful. If I am connected over ssh then stopping sshd logs me out. In the lab it is safe
- `stop` doesnt give much feedback. `status` shows if it is active. It showed inactive after stop and active after start
- `restart` kills it and brings it back
- `reload` reloads the config without a restart. Not every daemon can do it. If I am not sure I use `reload-or-restart` which reloads if it can and restarts if it cant

## Start at Boot

What if I want a service to start by itself at boot or not start. I used ntp which starts by default.

```bash
sudo systemctl disable ntp
sudo systemctl status ntp
sudo systemctl enable ntp
sudo systemctl is-active ntp
sudo systemctl is-enabled ntp
```

After disable the status still said active because I did not stop it. But it said disabled so it will not come up on the next boot. `enable` makes it start on the next boot. It does not start it right now. `is-active` asks if it is running and `is-enabled` asks if it starts at boot. They are quick one liners.

## Listing Services

```bash
sudo systemctl list-units
sudo systemctl list-units -t service
```

This shows the active units that systemd knows. There are other types too. The full name of a unit is like `ntp.service`. ntp is the first name and service is the type. Other types are sockets and targets and timers and mounts and devices. systemctl is smart so `ntp` and `ntp.service` work the same. With `-t service` I only see services.

The columns are the unit name and load and active and a description. Load says it was loaded in memory. Active says if it is running. Under them was the DigitalOcean monitoring agent so the lab is hosted on DigitalOcean.

## The nginx Scenario

nginx is a web server. It is on the system but not enabled. First I try to find it.

```bash
sudo systemctl list-units | grep nginx
sudo systemctl list-units --all | grep nginx
```

Nothing. `list-units` only lists units that systemd has tried to parse and load into memory. nginx was not. So I use a different command.

```bash
sudo systemctl list-unit-files | grep nginx
sudo systemctl status nginx
```

It is disabled and not active. A web server is something I would want to start by itself after a reboot. So I enable it for the boot and start it right now.

```bash
sudo systemctl enable nginx
sudo systemctl is-enabled nginx
sudo systemctl start nginx
```

The start failed. systemd told me to check the status and also to use `journalctl`. `journalctl` shows the logs of systemd.

```bash
sudo journalctl -xe
```

There were no entries which is wrong because systemd is always doing something. So the logging daemon needed a restart. I first found its full name.

```bash
sudo systemctl list-units | grep journal
sudo systemctl restart systemd-journald
sudo systemctl start nginx
sudo journalctl -xe
```

Now there were logs. I scrolled up with the arrow keys and found the reason. nginx uses port 80 by default and it was already used by another service so it couldnt bind. The fix is covered later in the series.

This shows why I need to know how to manage daemons. journald itself is a daemon.

## Recap

- A process is a running program and a daemon is a background process that ends in `d`
- systemd is the master daemon with PID 1 and it is the init system and service manager
- `systemctl` has `start` and `stop` and `restart` and `reload` and `enable` and `disable` and `status` and `is-active` and `is-enabled` and `list-units` and `list-unit-files`
- `journalctl -xe` shows logs when a service fails
