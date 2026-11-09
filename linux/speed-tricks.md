# Speed Tricks

## Terms

An alias is my own short name for a command. A log file is a file where a system or app writes what happens. `.bashrc` is a hidden file in my home directory. Bash reads it every time I open a terminal.

## Shortcuts

Little things add up when I use the terminal all day.

- Ctrl+L clears the screen
- Ctrl+A jumps to the start of the line
- Ctrl+E jumps to the end of the line
- Ctrl+U cuts everything before the cursor
- Ctrl+K cuts everything after the cursor
- Ctrl+Y pastes what I cut
- Alt+Backspace deletes one word back
- Ctrl+Shift+C copies and Ctrl+Shift+V pastes in the terminal
- Ctrl+Shift+Plus zooms in and Ctrl+Minus zooms out
- Ctrl+X then Ctrl+E opens the current command in my editor
- Up and Down arrows walk through old commands
- Ctrl+R searches old commands and pressing it again finds the next match

## Tab

One Tab completes a command or a path. Two Tabs show all the options.

## Repeat With sudo

When I forget sudo I dont retype. Two exclamation marks mean the last command.

```bash
sudo !!
```

## Aliases

```bash
alias ll='ls -l'
alias lumos='ls -la'
```

An alias is gone when I log out. To keep it I put the same line in `~/.bashrc`.

## Logs

Log files get big and `cat` is bad for that. `less` opens a file one page at a time. `tail` shows the last 10 lines. With `-f` it follows the file live and new lines show up as they happen. This is great to watch logins or to repeat an issue. Ctrl+C stops it.

```bash
sudo less /var/log/auth.log
sudo tail /var/log/auth.log
sudo tail -f /var/log/auth.log
```

`auth.log` records login attempts. If I add a user in another terminal I see it show up live.