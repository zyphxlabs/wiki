# Package Management

## Terms

A package is the software I want to install like Discord or Firefox. A package manager installs and updates and removes packages. A dependency is another package that a package needs to work. A repository is a server that stores a big collection of packages.

A low level manager does the basic job only. A high level manager does more of the work for me.

## dpkg and apt

These are the two main ones on Debian based systems like Parrot and Ubuntu and Kali. Debian based systems use `.deb` files. Red Hat and CentOS and openSUSE use `.rpm`.

`dpkg` is low level. I have to download the `.deb` file myself and it does not install dependencies. If something is missing it fails.

```bash
sudo dpkg -i package.deb
dpkg -l                       # list installed packages
```

`apt` stands for advanced package tool and it is high level. I only give the package name. It finds the package in a repository and installs the dependencies too.

```bash
sudo apt update               # refresh the list from the repositories
sudo apt install name
sudo apt remove name          # removes the app and keeps my settings
sudo apt purge name           # removes the app and the settings
sudo apt upgrade              # update installed packages
sudo apt full-upgrade         # also removes old packages that are not needed
sudo apt update && sudo apt upgrade
apt list --installed
apt search keyword
apt show name                 # details of a package
```

I always run `apt update` before installing because repositories are updated all the time.

If a dpkg install leaves broken dependencies this fixes it.

```bash
sudo apt --fix-broken install
```

## Repositories

When I run apt install it asks the repository for the package. The list of repositories is in the sources. `sudo apt edit-sources` opens it. On Parrot the real list is in a file under `/etc/apt/sources.list.d/`. If a tool is not in my repositories the tool often has its own repository. I add it to the sources then run update and install.

## aptitude

Aptitude is another high level manager like apt with more features. `sudo aptitude` opens a menu inside the terminal.

## snap

Snap is a package manager with its own store instead of a repository. Developers can put their app in the store and it is available right away. It needs `snapd` and works like apt.

```bash
sudo snap install code --classic
```

## Language Package Managers

Some managers belong to a programming language. `pip3` is for Python 3 and `gem` is for Ruby.

```bash
pip3 install -r requirements.txt
gem install rails
```

## Installing a Tool From GitHub

Git is a command line tool to pull code from places like GitHub. The steps are clone the repo and install the requirements and run the script.

```bash
sudo apt install git
git clone <repo url>
cd repo-name
pip3 install -r requirements.txt
python3 script.py -h
```

The file `requirements.txt` lists the libraries the script needs and pip reads it.
