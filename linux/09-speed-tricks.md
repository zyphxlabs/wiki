# Lecture 9 Command Line Speed Tricks

To be a good hacker I need to be fast in Linux. This lecture turns me into a command line ninja. I put the mouse away and only open the terminal with it. I start the lab and open the Linux Fundamentals module and launch the terminal.

## cd Tricks

`cd` means change directory. `cd desktop` jumps into the Desktop directory. `cd /` goes to the root of the file system. Here comes the first hack. Typing only `cd` and nothing else takes me home. The `~` in the prompt tells me I am home. It is like the Dorothy slippers.

```bash
cd Desktop
cd /
cd
pwd
```

Next I practiced going to a deep path under `/usr` with `cd` and the full path. That is just how `cd` works. Then I wanted to go back one directory. I use `cd ..`. Two dots take me back in time one directory. To go back two I put a forward slash and another two dots.

```bash
cd ..
cd ../..
cd ../../..
pwd
```

Going back three directories gets me to `/usr`.

## Clear the Screen

I can type `clear` or just press Ctrl+L. It is the faster way and the terminal gets nice and clean.

## Command History

I can press the up arrow to get the previous commands. This is my history. The down arrow goes the other way. I use this all the time.

## Jump Back to the Previous Directory

I went home with `cd` and then wanted to go back to the directory I was in before. For this I use `cd` with a dash.

```bash
cd -
```

It jumps from home to the last directory. Again and I am home. Again and I am back. It is handy when working between two directories which happens all the time.

What `cd -` does is look at a built in variable named `OLDPWD`. I can see it with echo and a dollar sign.

```bash
echo $OLDPWD
echo $PWD
```

`$OLDPWD` is the previous directory and `$PWD` is where I am now. The value changes when I move.

## ls Tricks

`ls` lists the contents but it is not in a list. For a real top down list I use `-l`.

```bash
ls -l
```

It shows a list with extra details. There are hidden files too. The `-a` shows them. Files that start with a dot are hidden. For example `.bashrc` is hidden.

```bash
ls -la
```

I always use `ls -la` but there is a faster way. There are two aliases that already exist.

```bash
ll
la
```

`ll` gives the long list and `la` gives the long list with hidden files. It saves keystrokes and that adds up.

## Aliases

An alias lets me create my own commands. The syntax is `alias` and the name and an equals sign and the command. I made one named lumos.

```bash
alias lumos='ls -la'
lumos
```

It works but it is not permanent. It disappears when I log out. To make it permanent I put it in the `.bashrc` file. That is the hidden file I saw with `ls -la` and it is where `ll` and `la` live. Bash reads it when I open a terminal.

```bash
nano .bashrc
```

I scroll down and see the other `ls` aliases and I add mine under them. Then I save and exit. It stays there forever.

## Tab Completion

When I start typing a command and press Tab Linux finishes it for me if there is enough to go on and nothing else is closer. It works for commands and for directories. If I press Tab twice I see all the available options. For example in `tail /var/lo` pressing Tab twice shows the possible paths. I can start typing and let Tab finish. It is a lifesaver.

## Zoom

- Ctrl+Shift+Plus or Ctrl+Plus zooms in
- Ctrl+Minus zooms out. It might vary by terminal

## Copy and Paste

In most terminals I select text and press Ctrl+Shift+C to copy and Ctrl+Shift+V to paste. It comes in handy a lot.

## Editing a Long Command

I typed a long command and made a typo. Using the arrows to walk back is slow. A ninja uses these.

- Ctrl+A jumps to the beginning of the line
- Ctrl+E jumps to the end of the line
- Ctrl+U deletes everything before the cursor. It is like a cut
- Ctrl+Y pastes back what was deleted. It brings back what I cut
- Ctrl+K deletes everything after the cursor. It is the opposite of Ctrl+U
- Alt+Backspace deletes the last word

I can combine them. For example I cut the end of a command with Ctrl+K then jump to the start with Ctrl+A and paste with Ctrl+Y. My hands never leave the keyboard. If I undo by cutting with Ctrl+U and jump to the end with Ctrl+E and paste I can move parts around fast. It takes practice but it makes me faster.

## Edit a Command in an Editor

If I paste a long command from a tutorial and it is messy I can press Ctrl+X and then E. It opens my default editor with that command. In the lab it is nano. I edit it and save and exit and it runs the command. I tested it with `echo`.

## Log Files

Log files are used a lot. This one is `/var/log/auth.log`. It shows things trying to log in to my Linux computer. I used `cat` in the series so far. But log files are big because they keep getting added to and `cat` doesnt do well with big things. A better command is `less`. It doesnt load the whole file at once. It loads part of it and I scroll. I press `q` to leave.

```bash
cat /var/log/auth.log
less /var/log/auth.log
```

The first time I got permission denied. Normally it means I forgot `sudo`. I can press the up arrow and Ctrl+A and add it. But a faster way is two exclamation points. That runs the last command.

```bash
sudo !!
```

Each trick saves a little but added up they give me years of my life back.

## tail

Often I only care about the most recent logs. `tail` shows the last 10 lines of a file.

```bash
sudo tail /var/log/auth.log
```

With `-f` I look at the log in real time.

```bash
sudo tail -f /var/log/auth.log
```

To test it I opened another terminal and ran `sudo useradd bernard`. The new line appeared live in the first terminal. I can monitor my sessions live like that. Ctrl+C stops it.

## Search the History

I forgot a command. Ctrl+R lets me reverse search the commands I did before. I clear the screen and press Ctrl+R and start typing like `l` or `les`.

```
Ctrl+R then less
```

It shows the matching command. Pressing Ctrl+R again goes to the next older match. The right arrow brings the command to my line so I can run it or edit it. I tried it with `echo` too.

## Recap

- `cd` alone goes home and `cd ..` goes back and `cd -` goes to the previous directory
- `ll` and `la` and my own aliases saved in `.bashrc`
- Tab and Tab Tab and Ctrl+L and Ctrl+A and Ctrl+E and Ctrl+U and Ctrl+K and Ctrl+Y and Alt+Backspace and Ctrl+X then E and Ctrl+R and `sudo !!`
- `less` for big files and `tail` and `tail -f` for logs
