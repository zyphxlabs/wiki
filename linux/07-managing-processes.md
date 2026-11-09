# Lecture 7 Managing Processes

This lecture is how to deal with processes on Linux. It is like opening the task manager on Windows with Ctrl+Alt+Delete and stopping something that is acting up. I do it all from the command line. I start the lab and open the Service and Process Management module and launch the terminal. I also opened Firefox to have a process to play with.

## Finding Processes

Every time I launch an application a process starts. I run `ps` first.

```bash
ps
```

It only shows what is running in my own terminal which is bash and the `ps` command itself. Firefox is not there. I have to be specific. I ask for all processes of my user. `-u` is for user.

```bash
ps -u user89786
ps -u user89786 | grep firefox
```

The pipe `|` sends the output into `grep`. grep stands for global regular expression print. It filters and finds stuff. Now Firefox shows with its process ID or PID.

`ps` has a lot of options. `ps --help simple` gives a simple rundown. The three I always use are `a` and `u` and `x`.

```bash
ps aux
ps aux | grep firefox
```

- `a` is all users
- `u` shows the user the process belongs to
- `x` shows the processes that were not started from this terminal

The first lot of processes belong to root. Further down other users show up. I use `grep` a lot with `ps aux`.

## pgrep

There is an easier way to find a process by name. `pgrep` is `ps` and `grep` in one command. It returns only the PID.

```bash
pgrep firefox
```

## Killing a Process

If Firefox freezes I must stop it. The command is `kill`. It needs the PID.

```bash
kill 2376
```

Firefox closed. I will explain what happened soon.

## Monitoring With top and htop

```bash
top
htop
```

`top` shows running processes sorted by CPU use. It shows the PID and the user and the CPU and memory. The one on top was VNC which is how I access the lab in the browser. I press `q` to leave. `htop` does the same thing and looks nicer. It is a fun way to monitor my Linux box.

## Foreground and Background

A foreground process holds the terminal. I cant do anything until it ends or I stop it. A background process runs without holding it.

`ping` is a tool network people use to check things are up. It says are you there and the other side replies yes. `-c` means count.

```bash
ping -c 100 networkchuck.com
```

It sends 100 pings and I cant use the terminal. That is a foreground process. Ctrl+C stops it. Another example is `sleep`.

```bash
sleep 30
```

It sleeps for 30 seconds and I am stuck. Ctrl+C kills it.

## Moving a Process to the Background

I start a ping again. While it runs I press Ctrl+Z. That is Z and not C. It puts the process to sleep and stops it. Then I list my jobs.

```bash
ping -c 1000 networkchuck.com
jobs
```

`jobs` shows the jobs table. The ping has the label `1`. It is still a foreground process and it is asleep. To make it a background process and continue I use `bg`.

```bash
bg 1
```

If there is only one job `bg` alone is enough. If there are more I give the job number. Now it runs but I cant stop it with Ctrl+C or Ctrl+Z. It is in the background and I cant interact. `jobs` says it is running. To bring it back I use `fg`.

```bash
fg 1
```

Now Ctrl+C stops it. I can do this many times to practice.

I also started a ping to networkchuck.com and put it to sleep with Ctrl+Z. Then `sleep 60` and put it to sleep. Then a ping to google.com. `jobs` showed three stopped jobs. I sent job 3 to the background with `bg 3`. Then I brought it back with `fg 3` and pressed Ctrl+C. It was gone from `jobs`.

To start a process straight in the background I put an ampersand `&` at the end.

```bash
ping -c 300 hackthebox.eu &
jobs
fg 3
```

The `jobs` command showed its number. Then `fg` and Ctrl+C stopped it.

## Process States

A process can have different statuses. The main ones are running and waiting and stopped and zombie. I ran `ping -c 1 networkchuck.com` and pressed Ctrl+Z so it stopped. Then I checked `ps ax`.

```bash
ps ax
```

In the status column the ping showed `T`. It stands for traced but it means stopped.

- R is running
- S is sleeping or waiting
- T is stopped
- Z is a zombie

## Signals

When I use `kill` it sends a kill signal. There are many signals. `kill -l` lists them all.

```bash
kill -l
```

The ones I care about are these.

- 15 SIGTERM is the default signal when I dont give one. It is more like a suggestion. It asks the process nicely to die when it wants to. The process can refuse. So `kill` without a signal may not kill a process that is going crazy
- 9 SIGKILL is the real kill. It forcibly terminates a process and it has no other option than to die
- 2 SIGINT is interrupt. This is what Ctrl+C does
- 19 SIGSTOP stops a process. Ctrl+Z works like a stop signal. Technically Ctrl+Z sends SIGTSTP which is a close cousin but it acts the same way
- 18 SIGCONT continues a process that was stopped

So Ctrl+C and Ctrl+Z are shortcuts for the kill command with a different signal.

## Practicing the Signals

I started a sleep in the background and checked its PID.

```bash
sleep 900 &
jobs
ps
```

Then I sent signal 19 to stop it.

```bash
kill -19 21374
jobs
```

That is the same as Ctrl+Z. Then another sleep and signal 2.

```bash
sleep 700 &
ps
kill -2 21887
jobs
```

That is the same as Ctrl+C and the sleep was interrupted and gone. Then the real kill with signal 9 on the ping and the sleep.

```bash
kill -9 18517
kill -9 21374
jobs
```

They were killed.

## pkill

`pkill` is to `kill` like `pgrep` is to `ps`. I dont give a process ID. I give a name. I started a bunch of pings in the background.

```bash
pkill -9 ping
```

All the pings were killed at the same time.

## Recap

- I can find processes with `ps` and `pgrep` and watch them with `top` and `htop`
- I can kill them gracefully with a plain `kill` or by force with `kill -9`
- I can move them between foreground and background with Ctrl+Z and `bg` and `fg` and `&`
- `pkill` kills by name
