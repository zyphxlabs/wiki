# Users and Groups

## Terms

A user is an account that can log in and use the system. Root is the main admin user and it has full power. It is also called the super user. A group is a set of users that share permissions. A UID is a user id and a GID is a group id. A hash is a scrambled version of a password so the real one is not stored.

## Where It Is Stored

- `/etc/passwd` lists all users
- `/etc/shadow` has the hashed passwords and needs sudo to read
- `/etc/group` lists all groups and their members

One line in passwd looks like this.

```
username:x:1001:1001:comment:/home/username:/bin/bash
```

The fields in order are the username and an x and the UID and the GID and a comment like the full name and the home directory and the default shell. The x means the password is kept in the shadow file. Every new user also gets a group with the same name.

Many users in passwd are system accounts. They do jobs for the system and cant log in.

## sudo

sudo means super user do. It borrows root power for one command only. I use it as a please. Only root can add users and groups so I need sudo for that.

## Adding Users

There are two commands and they are not the same. `adduser` asks questions and makes the home directory and sets bash as the shell. `useradd` is lazy. It makes the user and nothing else. There is no home and no password and the shell is sh. With `-m` it makes the home.

```bash
sudo adduser name
sudo useradd -m name
sudo passwd name        # set or change a password
```

## Changing Users

```bash
sudo usermod --shell /bin/bash name     # change the shell
sudo usermod -l newname oldname         # rename a user
sudo usermod -aG groupname name         # add a user to a group
```

In `-aG` the a means append. Without it the user loses all other groups. This is a big one.

## Groups

```bash
sudo groupadd groupname        # make a group
groups                         # groups I am in
cat /etc/group                 # all groups
sudo gpasswd -d name group     # take a user out of a group
sudo groupdel groupname        # delete a group. The users stay
sudo userdel name              # delete a user
```

## Switching Users

`su` means switch user. With the dash it gives me a full login of that user. I need their password unless I use sudo.

```bash
su - name
sudo su -       # becomes root
exit            # go back. Ctrl+D works too
```

I should never log in as root. I use sudo instead.

## The sudoers File

This file decides who can use sudo. If a user is not in it sudo says the incident will be reported. I must edit it only with `sudo visudo`. A typo here can break the whole system and visudo checks the syntax before saving.

```
root         ALL=(ALL:ALL) ALL
username     ALL=(ALL) ALL
%sudo        ALL=(ALL:ALL) ALL
%groupname   ALL=(ALL) NOPASSWD: ALL
```

A name alone is a user. A name with a percent sign is a group. The members of the sudo group already have full power. To limit a user to one command I write the full path of that command at the end instead of ALL.

If visudo finds an error it gives options. `e` edits again and `x` exits without saving and `Q` saves anyway. I should never choose Q.

The principle of least privilege means give only the power that is needed and take it away when the job is done.
