# Processes

## Terms

A process is a running program. Every program I open starts a process and closing it ends it. Each process has a PID which is a process id number. A signal is a message I send to a process like stop or die. A foreground process holds my terminal and I cant type until it ends. A background process runs by itself. A job is a process started from my terminal that the shell keeps track of.

## Finding Processes

`ps` alone only shows what runs in my own terminal. I add options to see more. In `aux` the a means all users and the u shows the user and the x includes processes not started from this terminal.

```bash
ps
ps -u username
ps aux
ps aux | grep firefox     # find one program
pgrep firefox             # just the PID
```

A pipe `|` sends the output of one command into the next. `grep` filters lines.

## Monitoring

`top` shows live processes sorted by CPU use. Press q to leave. `htop` does the same and looks nicer.

## States

- R is running
- S is sleeping or waiting
- T is stopped
- Z is a zombie

## Killing

`kill` sends a signal to a PID. Without a number it sends 15 which asks the process nicely to stop. The process can refuse. Signal 9 is the forced kill and it cant be refused.

```bash
kill 1234
kill -9 1234
pkill -9 ping       # kill by name. All matches die
kill -l             # list all signals
```

- 15 SIGTERM asks to stop and is the default
- 9 SIGKILL kills for sure
- 2 SIGINT interrupts and it is the same as Ctrl+C
- 19 SIGSTOP stops a process and Ctrl+Z is a close cousin of it
- 18 SIGCONT continues a stopped process

## Foreground and Background

```bash
ping -c 100 example.com     # runs in the foreground
```

Ctrl+C stops it for good. Ctrl+Z pauses it and puts it to sleep. `jobs` lists the paused jobs with a job number. `bg 1` sends job 1 to the background and it keeps running. `fg 1` brings it back to the foreground.

A background process ignores Ctrl+C so I have to bring it back with `fg` first. To start a process straight in the background I put `&` at the end.

```bash
ping -c 100 example.com &
jobs
fg 1
```