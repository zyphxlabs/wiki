# Lecture 4 User Management

This lecture covers everything about users and groups. I create users and delete them and change them. I create groups and delete them. Then I use sudo and the sudoers file. The video told it as a story where the Avengers stop Thanos and I use the same names. I start the lab and open the User Management module and launch the terminal.

## What Is a User

Every computer has users and Linux too. To log in and do anything I have to be a user. A user is an account on the system. `whoami` shows which one I am. A user with the right permissions can launch programs and create things and delete things. But not all users are the same.

## Adding a User With adduser

The scenario is that Thanos is attacking and I need the Avengers in the system. I start with Thor.

```bash
adduser thor
```

It fails with only root may add a user to the system. Root is another user on the system and he is the super user. I borrow his power for one command with sudo. I use the up arrow to bring the command back and put `sudo` at the beginning. Sudo is like saying please in Linux.

```bash
sudo adduser thor
```

It asks for a password and the password again and the full name and some other information. I can just press Enter for the other information. Then it asks if it is correct. Thor is now a user on the system.

## Where Users Are Stored

There is a file with every user on the system.

```bash
cat /etc/passwd
```

I see a lot of users. The last ones are me and Thor. The rest are system accounts that do jobs for the system. Many of them cant log in. The word nologin in their entry means that.

This is one line.

```
thor:x:1001:1001::/home/thor:/bin/bash
```

- The first part is the username
- The `x` means the password is stored in a separate file. It is the shadow file
- The first number is the user id or UID. It is 1001 here
- The second number is the group id or GID. It is also 1001
- Then there is room for comments like the full name
- Then the home directory which is `/home/thor`
- At the end the default shell. Adduser gave bash

The passwords live in `/etc/shadow` in the shadow where they are hidden.

```bash
sudo cat /etc/shadow
```

Next to thor there is a long string. It is not his password. It is the hashed version. A hash is a scrambled version so the real password is not stored. I need sudo because I dont have permission without it.

Every time I create a user Linux also creates a group with the same name. So there is a user thor and a group thor and the member of the group is thor.

## Home Directory

Thor got a home directory. I check it.

```bash
cd /home
ls
cd thor
ls
ls -al
```

I see Desktop and Templates and so on. With `-al` the hidden files show too.

## useradd

There is a second command that is confusingly almost the same name. `adduser` is friendly and does a lot. `useradd` is lazy. Next is Iron Man.

```bash
sudo useradd ironman
```

It just says nothing. It doesnt ask for anything. If I look at the passwd file he is there but his shell is `sh` and not bash. In the shadow file he has an entry but no real password. And he has no home directory. In `/home` I see only me and Thor.

I set his password with `passwd`. It is spelled without the o and r.

```bash
sudo passwd ironman
```

Now the shadow file has a real hash for him.

## Changing a User With usermod

`usermod` means user modification. It changes a user. `usermod -h` shows lots of options. First I fix his shell.

```bash
sudo usermod --shell /bin/bash ironman
```

Now the passwd file shows bash. I can also rename a user with `-l` and the new name and then the old name.

```bash
sudo usermod -l tonystark ironman
```

Then I changed it back.

```bash
sudo usermod -l ironman tonystark
```

## useradd Is Fast

`useradd` is good when I want to add users fast without a menu. With `-m` it creates the home directory.

```bash
sudo useradd -m hulk
sudo useradd -m spiderman
sudo useradd -m loki
sudo useradd -m captainamerica
sudo useradd -m doctorstrange
```

Then `cat /etc/passwd` shows all of them. The bad guy comes too.

```bash
sudo adduser thanos
```

## sudo and the Infinity Gauntlet

Sudo is the infinity gauntlet in Linux. It stands for super user do. When I use it I put on the gauntlet for one moment and I can do whatever I want. The sudo command lets me use the power of the super user which is the root user. Root is the boss. He can change or delete all the settings and delete half my stuff with `rm -rf`. If I can impersonate him for a moment that is powerful so not everyone should get it. By default it is restricted.

## su

`su` means switch user. It lets me become another user. The dash makes it a full login.

```bash
su - ironman
```

Without sudo I need the password of that user. If I use `su -` with no name it switches to root. With sudo I dont need a password.

```bash
sudo su -
```

That made me root. I should never log in as root and this is why we have sudo. To go back I use Ctrl+D or `exit` or `logout`.

As Iron Man I try to use sudo.

```bash
sudo useradd pepperpotts
```

It asks for a password and then says ironman is not in the sudoers file and this incident will be reported. He cant put on the gauntlet.

## The sudoers File

The sudoers file defines who can use sudo. It is very important. A mistake can ruin the whole system but there are checks that keep me safe. The only recommended way to edit it is with `visudo`. I log out of Iron Man and edit it as me.

```bash
sudo visudo
```

I scroll down and see this.

- The root line says root can use sudo
- A line with `%sudo` says the sudo group members can run any command and they dont need a password. A percent sign in front means it is a group

I add Thanos just under root.

```
thanos ALL=(ALL) ALL
```

The first ALL means all systems. It matters in multi system setups. The last ALL means all commands. To give him only one command I would write the full path of that command like `/sbin/useradd` instead. Thanos gets all power. Save and exit is Ctrl+X then Y then Enter.

Now I become Thanos and test it.

```bash
sudo su - thanos
sudo useradd nebula
cat /etc/passwd
```

It worked.

## Deleting Users

`userdel` deletes a user. This is the snap.

```bash
sudo userdel thor
sudo userdel spiderman
sudo userdel doctorstrange
```

Thor and Spider-Man and Doctor Strange are gone. Hulk and Loki and Captain America and Iron Man are left.

## Groups

To fix this I need a group with sudo access. `groupadd` creates a group. In the lecture the name had a space but group names cant have spaces so I write it with a hyphen.

```bash
sudo groupadd infinity-gauntlet
cat /etc/group
```

Just like passwd for users there is a file for groups. The group is in there and so is the group of every user. The `groups` command shows which groups I am in.

```bash
groups
```

I am in my own group and the sudo group. That is why I have the gauntlet.

I add the new group to the sudoers file.

```bash
sudo visudo
```

Under the sudo group line I add this.

```
%infinity-gauntlet ALL=(ALL) NOPASSWD: ALL
```

When I messed up the syntax and saved visudo told me there is a syntax error. It gave options. `e` edits it again. `x` exits without saving. `Q` saves anyway and that is dangerous. I pressed `e` and fixed it and saved again with no errors. A typo in this file can break the whole system.

Iron Man still cant use sudo because he is not in the group. I add him with `usermod`. The option is `-aG`. `-G` alone replaces the groups of the user so I use `-a` too which means append.

```bash
sudo usermod -aG infinity-gauntlet ironman
cat /etc/group
```

Iron Man is in the members of the group now. Thanos wrestled for the glove so I add him too and then remove him. `gpasswd -d` removes a user from a group.

```bash
sudo usermod -aG infinity-gauntlet thanos
sudo gpasswd -d thanos infinity-gauntlet
```

Now as Iron Man I can create the lost users again and delete Thanos.

```bash
su - ironman
sudo useradd captainamerica
sudo userdel thanos
```

## Destroying the Gauntlet

Using the principle of least privilege I remove the group when the job is done. That means giving only the power that is needed. `groupdel` deletes a group. It does not delete the users inside it.

```bash
sudo groupdel infinity-gauntlet
```

Iron Man lost his special privileges too. If he tries to create the group again he cant.

## Recap

- I can add and delete and change users and groups
- `adduser` is the friendly one and `useradd` is the lazy one
- `/etc/passwd` has users and `/etc/shadow` has hashed passwords and `/etc/group` has groups
- `usermod` and `passwd` and `su` and `sudo` and `visudo` and `userdel` and `groupadd` and `groupdel` and `gpasswd`
- The Hack The Box module has the commands in text and a quiz at the bottom that earns cubes back
