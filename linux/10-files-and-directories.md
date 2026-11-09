# Lecture 10 Creating Files and Directories

In this lecture I learn how to create files and directories and lots of them. At the end I break Linux for fun. The video did it on a temporary Hack The Box lab machine and not on a real computer. I start the lab and open the Linux Fundamentals module and launch the terminal.

## Creating Files With touch

`touch` creates a file. The file is empty.

```bash
touch cant-touch-this.txt
cat cant-touch-this.txt
```

The `cat` shows nothing inside. I can create many files at once.

```bash
touch cant-touch-this.txt one.txt two.txt three.txt
ls
```

## Creating Files With cat

I can also create files with stuff inside. First way is `cat` with a right arrow and the file name. It waits for me to type text. When I am done I press Ctrl+D to save.

```bash
cat > file.txt
```

Then I check it.

```bash
cat file.txt
```

A second way is useful in bash scripts. I use two left arrows and the word EOF. EOF stands for end of file. It can be any word. I add a right arrow and the file name.

```bash
cat << EOF > file.txt
this is line one
this is line two
EOF
```

It lets me write many lines and it ends when I type EOF and press Enter. I could use another word like `cat` as the end word.

A third way is the one liner with `echo`. I put the text and a right arrow and the file name.

```bash
echo "some text" > stuff.txt
cat stuff.txt
```

So I have `touch` and `cat` and `echo`. I will see these in scripts or use them myself.

## Directories

A directory is basically a folder. Windows people say folder but in Linux say directory. I made a directory to put all these files in so my home directory is not cluttered.

```bash
mkdir cool-stuff
ls -l
```

`mkdir` means make directory. In `ls -l` the first letter shows what it is. A `d` is a directory and a dash is a file. This is better than trusting colors because I am colorblind and colors can trick me. I can make many directories at once like touch.

```bash
mkdir one two three four
ls -l
```

## Moving Files

`mv` means move. First the file and then where to go. The period and slash at the start means the directory is right here in my current working directory.

```bash
mv file.txt ./cool-stuff
ls -l
ls cool-stuff
```

If I forget the period then Linux thinks `cool-stuff` is at the root of the file system and I get permission denied and not found. Just like `cd /` shows the root has no folder called cool-stuff. So I need the period.

I can also rename while I move. It is like the witness protection program.

```bash
mv stuff.txt ./cool-stuff/new-identity.txt
```

I can move many things at once. The files first and then the location.

```bash
mv one.txt two.txt three.txt ./cool-stuff
ls cool-stuff
```

## Copying Files

`cp` copies. It leaves the original where it is. It is good for a backup before changing a file. It works like `mv`. I give the file and the location.

```bash
cp cant-touch-this.txt ./cool-stuff
cp cant-touch-this.txt ./cool-stuff/you-can-touch-this.txt
cp cant-touch-this.txt cant-touch-this.txt.bk
```

- The first copy keeps the name in the new place
- The second copy gives a new identity
- The third makes a backup in the same directory. People normally use `.bk` for backup. It must be a different name in the same directory or it wont work

To clean up I moved all the loose files into `cool-stuff` with `mv`.

## Parent and Child Directories

`mkdir -p` creates a parent directory with child directories under it. The p stands for parent.

```bash
mkdir -p directory/another-one/another-one/another-one
ls -l
tree
```

`ls -l` only shows `directory`. `tree` shows the whole directory tree. It is a really cool command. If I scroll up I see the chain of directories.

## Moving and Copying Directories

Moving a directory is the same as moving a file.

```bash
mv cool-stuff ./directory/another-one
ls -l
tree
```

`cool-stuff` is no longer in my home directory. The tree shows it as a leaf inside another-one with all its files. I can also copy a directory but it needs `-r`.

```bash
cp cool-stuff ./directory/another-one/another-one
```

It said I am not using `-r` because the directory has stuff in it. `-r` means recursive. In this case it means copy all the files inside it too. Then I ran it again with `-r`.

```bash
cp -r ./directory/another-one/cool-stuff ./directory/another-one/another-one
tree
```

It worked and the tree showed `cool-stuff` inside there too like inception.

## Removing

`rm` means remove. I made a file called please-dont-delete-me and removed it.

```bash
touch please-dont-delete-me
rm please-dont-delete-me
```

It can take many files too. To remove a directory I can use `rmdir` which means remove directory.

```bash
mkdir actually
rmdir actually
```

But it only works on an empty directory. If I try it on `directory` which has stuff inside it fails with directory not empty. Linux is protecting me from myself. The way to remove a directory that has stuff inside is the plain `rm` with `-r`. It means recursive. Before it copied everything inside. This time it removes everything inside along with the directory.

```bash
rm -r directory
ls -l
tree
```

It is gone with everything in it. It is not reversible.

## The Dangerous One

`rm --help` explains the switches. `-r` is recursive. `-f` means force. Force ignores all warnings. Linux says you should not do this and force goes through anyway. The other one is `--no-preserve-root`. Normally `rm` protects `/` and this switch removes that safety.

```bash
sudo rm -rf --no-preserve-root /
```

This deletes everything starting at the root of the file system. It breaks Linux a hundred percent. Do not run this on a real computer. It is only for a temporary or ephemeral machine like the Hack The Box lab. I must also be done for the day because the free version gives only one instance per day.

When I ran it a lot of things said they cant be removed but it removed many others. Then even `pwd` didnt work and `echo hi` didnt work. Everything in Linux is a file and commands are files. I deleted the commands. That is how to break Linux. I reset the machine after.

## Making a Million Directories

I wanted to see how many directories it takes to break Linux. I used `mkdir -p` to nest many directories. There is a limit on how deep I can go in one go. I could only make about 600 or so at once. So I used a bash script with a for loop. It creates a new parent with about 600 children and does it over and over about 1500 times. It is called with `bash`. It took a minute. Then I ran `tree` and it went on for a long time. The count at the end was 1,116,017 directories all named another-one. I know it was stupid but it was fun.

## Recap

- `touch` and `cat` and `echo` create files
- `mkdir` and `mkdir -p` create directories and `tree` shows them
- `mv` moves and renames and `cp` copies and `cp -r` copies directories
- `rm` and `rmdir` and `rm -r` remove
- `rm -rf --no-preserve-root /` breaks the system and I only do it in a lab I can reset
