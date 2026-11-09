# Lecture 5 Package Management

This lecture is about how to install and remove software on Linux. It covers everything from common tools to custom hacking tools from GitHub. I start the lab and open the Package Management module and launch the terminal.

## Packages and Package Managers

The things I want to install like Discord or Firefox or Minecraft are contained in packages. A package manager installs them. There are two main ones. They are `dpkg` and `apt`. I will show the hard way first.

## dpkg

`dpkg` is called a low level package manager. It installs and uninstalls and removes packages but it has two flaws. One is that I have to find and download the package myself.

I installed Discord. I opened the browser and went to discord.com and clicked download and Download for Linux and chose deb. Then I chose Save file. A `.deb` is the file type for packages on Debian based systems. Parrot OS is Debian based. On Windows the same idea is exe and on Mac it is dmg. On other systems like CentOS or openSUSE or Red Hat the file type is `.rpm`. I only cover Debian based here.

```bash
cd Downloads
ls
```

There is the discord deb file. I install it with `dpkg`. The `-i` means install.

```bash
sudo dpkg -i discord.deb
```

It fails with a dependency error. This is the second flaw. Most packages depend on other packages to work. Discord depends on `libappindicator1` and `libc++1` and they were not installed. dpkg just tells me to go get those myself.

## apt

`apt` stands for advanced package tool. It is called a high level package manager which means it is easier to use. First I update.

```bash
sudo apt update
```

Then I install by name only. I tried pidgin which is an open source instant messaging app.

```bash
sudo apt install pidgin
```

It didnt work at first because of the broken Discord install and that was dpkgs fault. But apt gives me the fix.

```bash
sudo apt --fix-broken install
```

It installed the two missing packages. Then I ran the pidgin install again. It shows a list of new packages that will also be installed. That is the dependencies and apt does that for me. I confirm and wait. Then I launch it by typing the name.

```bash
sudo apt install pidgin
pidgin
```

## Repositories

apt does not need the deb file. It uses a repository. A repository is a storage location or a server that holds a collection of software. When I ran install apt went to the repository and asked for pidgin and downloaded it.

Running `sudo apt update` makes apt go to the repositories and ask for the list of everything they have. I do this before every install because repositories are owned by someone else and updated often.

To see which repositories I use.

```bash
sudo apt edit-sources
```

It asks which editor to use. Parrot does it differently. It points to another location so I copy that path and open it. The list is the file `parrot.list` and I can open it with `sudo nano`. It shows lines starting with `deb` and then the server address. I can click the link in the browser and browse the repository. Inside the pool and main directory and then the letter n I find nmap. When I run `sudo apt install nmap` apt looks there.

If a tool is not in these repositories it often has another way to install it or its own repository. I add that repository to the sources list then run `sudo apt update` and `sudo apt install`.

## More apt Commands

```bash
apt -h
sudo apt list
apt list --installed
apt list --installed | grep nmap
apt show nmap
apt search nmap
```

- `apt list` lists all packages available in the repositories
- `--installed` shows what my system has
- The pipe and `grep` filters the long list and searches for something like nmap. grep is covered in another lecture
- `apt show` tells me what a package is and what it does
- `apt search` searches through the descriptions

## Removing

```bash
sudo apt remove pidgin
sudo apt purge pidgin
```

`remove` removes the app but not my user data like config. If I removed it by accident or want to reinstall that stuff is still there. `purge` removes everything. I checked with `apt list --installed | grep pidgin`. The main package was gone but a second one named pidgin-data was left. I removed it with `sudo apt purge pidgin-data`.

## Upgrading

```bash
sudo apt upgrade
sudo apt update && sudo apt upgrade
sudo apt full-upgrade
```

`upgrade` updates the packages that have updates. Often I join it with update using `&&` which runs two commands in one line. `full-upgrade` also removes previously installed packages that are not required for the upgrade like old versions.

## dpkg Can List Too

```bash
dpkg -l
dpkg -l | grep nmap
```

## aptitude

`aptitude` is also a high level package manager. It is like apt on steroids. It does more. `sudo aptitude` gives an interactive GUI like thing in the command line. I quit it with q.

## snap

`snap` is another package manager. If I dont have it installed the package is `snapd`.

```bash
sudo apt install snapd
```

Snap is not a repository. It is more like a store called the Snap store. A developer can add their app there and it is available right away without waiting to get into a repository like Ubuntus or Parrots. Using the command line works like apt. I installed VS Code with it. It needs the `--classic` switch.

```bash
sudo snap install code --classic
code
```

## Language Specific Package Managers

Some manage packages for a programming language. A Python script needs libraries and packages. Python has `pip`. For Python 3 the command is `pip3`. Ruby is another language and its package manager is RubyGems and the command is `gem`.

```bash
gem install rails
```

## Installing Tools From GitHub

Hackers create tools and put them on GitHub. To get them I need `git`. I install it with the package manager I know.

```bash
sudo apt install git
git --version
```

`git --version` is how to check if it is installed. The example tool was turbo lister. It is a Python tool that looks at a website and lists the subdomains passively. Passive means it only looks and does not attack. First I copy the repository link from GitHub. Then I clone it. Clone copies their code to my Linux machine in the directory I am in.

```bash
git clone <repository link>
ls
cd turbo-lister
ls
```

The project is a Python project so I run the Python script. But first like every application it has requirements. The libraries have to be installed. `pip3` can do it with `-r` and the file `requirements.txt`. It reads the file and installs all the dependencies.

```bash
pip3 install -r requirements.txt
python3 turbolister.py -h
python3 turbolister.py -d hackthebox.eu
```

`-h` gives help. `-d` is for domain. It listed many subdomains of hackthebox.eu. It is passive recon and not an attack. GitHub is also a repository and git is the tool. They are different.

## Recap

- The two main ways are `dpkg` which is low level and needs the deb file and `apt` which is high level and gets dependencies and uses repositories
- `aptitude` is a fancier version
- `snap` pulls from a store
- `pip3` is for Python and `gem` is for Ruby
- `git clone` pulls tools from GitHub. Then I install requirements with pip3 and run the script
