# Services

## Terms

A process is a running program. A daemon is a process that runs in the background and I never touch it. It is the same idea as a service on Windows. Daemons run things like networking and printing and ssh. The name usually ends with a d like `sshd` and `ntpd`. A port is a numbered door on a machine that a service listens on. Web servers use port 80 by default.

```bash
ps aux | grep ssh      # finds the ssh daemon
```

## systemd

systemd is the master daemon. It is the first process that starts after the kernel and its process id is 1. It starts all the other daemons. This job is called the init system. It is also the service manager. It calls daemons units and a unit has a name like `ssh.service`. The command `pstree` shows all the processes branching out from systemd.

Other init systems exist like SysV init and Upstart but systemd is the main one now.

## systemctl

`systemctl` is the tool I use to control the units.

```bash
sudo systemctl start sshd
sudo systemctl stop sshd
sudo systemctl restart sshd
sudo systemctl reload sshd               # reload the config. Not every service can
sudo systemctl reload-or-restart sshd    # reload if it can and restart if not
systemctl status sshd
```

If I am connected by ssh then stopping sshd logs me out.

To control the start at boot I use enable and disable. Enable does not start the service right now. It only makes it start on the next boot.

```bash
sudo systemctl enable ntp
sudo systemctl disable ntp
systemctl is-active ntp       # is it running now
systemctl is-enabled ntp      # will it start on boot
```

## Listing Services

```bash
systemctl list-units                      # active units only
systemctl list-units -t service           # only services
systemctl list-units --all                # also inactive
systemctl list-unit-files | grep nginx    # every installed unit
```

A gotcha is that `list-units` only shows units that systemd already loaded into memory. A service that was never started can be missing. `list-unit-files` shows it.

## When a Service Fails

If a start fails systemd tells me to check the logs. `journalctl` shows the logs of systemd.

```bash
sudo systemctl start nginx
sudo journalctl -xe
```

A common reason is that the port is already in use. Nginx wants port 80 and something else holds it. If journalctl shows nothing I restart its own daemon.

```bash
sudo systemctl restart systemd-journald
```