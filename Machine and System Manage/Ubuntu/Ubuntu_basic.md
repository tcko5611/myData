# Ubuntu

# update package

```bash
sudo apt update

```

# upgrade system

```bash
sudo apt upgrade

```

# install package

```bash
sudo apt install emacs25

```

# search package
```bash
apt-cache search gcc

```
# install chrome
```bash
$ wget <https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb>
$ sudo apt install ./google-chrome-stable_current_amd64.deb
$ cat /etc/apt/sources.list.d/google-chrome.list

### THIS FILE IS AUTOMATICALLY CONFIGURED ###
# You may comment out this entry, but any other modifications may be lost.
deb [arch=amd64] <http://dl.google.com/linux/chrome/deb/> stable main
```