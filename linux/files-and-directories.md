# Files and Directories

## Terms

A directory is what Windows calls a folder. The working directory is the one I am in right now. My home directory is the personal folder of my user and it is written as `~`. The root of the file system is `/` and it is the top of everything. A single dot `.` means the current directory and two dots `..` mean one directory back.

## Moving Around

`pwd` means print working directory and it shows where I am. `ls` lists what is in the current directory. `cd` means change directory and it moves me somewhere else.

```bash
pwd
ls
cd folder      # go into a folder
cd ..          # back one directory
cd ../..       # back two directories
cd             # straight home
cd -           # back to the previous directory
cd /           # the root of the file system
```

The `cd -` trick works because bash saves my last directory in a variable called `OLDPWD`.

## Listing Better

```bash
ls -l      # long list with details
ls -a      # also show hidden files
ls -la     # both
```

Hidden files start with a dot like `.bashrc`. In the long list the first letter tells me the type. A `d` is a directory and a `-` is a file. This is better than trusting colors.

## Making Files

`touch` makes an empty file. It can make many at once.

```bash
touch file.txt
touch a.txt b.txt c.txt
```

To make a file with text in it I can use `cat`. I type my text and press Ctrl+D to save.

```bash
cat > file.txt
```

For many lines I use EOF. EOF means end of file and it can be any word. The text ends when I type that word again.

```bash
cat << EOF > file.txt
line one
line two
EOF
```

For one line I use echo.

```bash
echo "some text" > file.txt
```

To read a file I use `cat file.txt`. The name cat comes from concatenate.

## Making Directories

`mkdir` means make directory. With `-p` it makes the parent and the children together.

```bash
mkdir stuff
mkdir one two three
mkdir -p parent/child/deeper
tree                      # shows the whole directory tree
```

## Moving and Copying

`mv` moves a file. It also renames because moving to a new name is the same thing. `cp` copies. To copy a directory I need `-r` which means recursive. Recursive means do it for everything inside too.

```bash
mv file.txt ./stuff              # move into stuff
mv file.txt ./stuff/new.txt      # move and rename
mv a.txt b.txt ./stuff           # move many
mv stuff ./other                 # move a directory
cp file.txt ./stuff              # copy
cp file.txt file.txt.bk          # quick backup in the same place
cp -r stuff ./other              # copy a directory
```

The `./` part matters. It says the folder is right here. Without the dot Linux looks for it at the root of the file system.

A copy in the same directory needs a different name from the original.

## Removing

```bash
rm file.txt        # remove a file
rmdir folder       # remove an empty directory
rm -r folder       # remove a directory and everything inside
rm -rf folder      # force it and ignore all warnings
```

Remove can not be undone. Linux refuses to remove a folder that has stuff in it with `rmdir` because it protects me from myself. Running `rm -rf` on `/` deletes the whole system. I should never do that on a real machine. It only makes sense on a temporary lab.