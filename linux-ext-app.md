# Linux applications

## useful links:
* [gnu applications](https://www.gnu.org/software/)
* [how to install ubuntu on USB stick](https://ubuntuhandbook.org/index.php/2014/11/install-real-ubuntu-os-usb-drive/)
* [rust command line tools](https://gist.github.com/sts10/daadbc2f403bdffad1b6d33aff016c0a)
* [vnc alternative - connect to existing session](http://www.karlrunge.com/x11vnc/)
* [web page like a screensaver](https://github.com/lmartinking/webscreensaver)
* [jira editing shortcuts](https://jira.atlassian.com/secure/WikiRendererHelpAction.jspa?section=all)
* [i3 window manager shortcuts](https://i3wm.org/docs/refcard.html)
* [xmind settings](https://www.xmind.net/m/PuDC/)
 > XMind.ini: ```-vm  /home/user/.sdkman/candidates/java/8.0.222-zulu/bin/java ```
* [awesome windows manager, battery widget](https://github.com/deficient/battery-widget)
```bash
echo $XDG_CONFIG_DIRS
locate rc.lua
```
* [rainbow cursor](https://www.gnome-look.org/p/1300587/)
```sh
# place for mouse pointer, cursor, theme
/usr/share/icons
```


### gnome settings 
#### gnome settings configuration customization adjuster
```sh
sudo apt install dconf-editor
```
examples of suppressing: monitor mode switch, show desktop
```sh
dconf-editor
# /org/gnome/mutter/keybindings/switch-monitor
# /org/gnome/desktop/wm/keybindings/show-desktop
```
manually can be achieved via
```
~/.local/share/gnome-shell/extensions/<extension-identifier>/prefs.js
~/.local/share/gnome-shell/extensions/<extension-identifier>/settings.js
```

remove HP default display mode switching
```sh
dconf-editor
# /org/gnome/mutter/keybindings/switch-monitor
# ['<Super>p', 'XF86Display']
# replace to
# []
```

#### gnome list of settings
```sh
# all gnome settings
gsettings list-recursively 
# one settings
org.gnome.desktop.background picture-uri
```

### reset Gnome to default
```
rm -rf .gnome .gnome2 .gconf .gconfd .metacity .cache .dbus .dmrc .mission-control .thumbnails ~/.config/dconf/user ~.compiz*
```

### restart Gnome shell
```sh
alt-F2 r
```
### gnome application icon
```sh 
ls /usr/share/applications/*.desktop
cat /usr/share/applications/usb-creator-gtk.desktop
```
### adjust Gnome desktop shortcuts, gnome shortcuts
```sh
dconf-editor
```
gnome keybinding
```sh
/org/gnome/desktop/wm/keybindings
```
save/restore
```sh
# dconf dump /org/gnome/desktop/wm/keybindings/ > org_gnome_desktop_wm_keybindings
dconf dump /org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ > org_gnome_settings-daemon_plugins_media-keys_custom-keybindings_custom0

# dconf load /org/gnome/desktop/wm/keybindings/ < org_gnome_desktop_wm_keybindings
dconf load /org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/custom0/ < org_gnome_settings-daemon_plugins_media-keys_custom-keybindings_custom0
```
alternative way to get/set settings
```sh
gsettings list-schemas
gsettings list-keys org.gnome.desktop.wm.keybindings
gsettings get org.gnome.desktop.wm.keybindings close
gsettings set org.gnome.desktop.wm.keybindings close "['<Super>w']"
```
find shortcuts in the settings
```sh
## for suppressing Super + P 
gsettings list-recursively | grep "<Super>p"

# UI tool: dconf-editor
gsettings set org.gnome.mutter.keybindings switch-monitor "['XF86Display']"
gsettings set org.gnome.mutter.keybindings switch-monitor "['']"

gsettings reset org.gnome.mutter.keybindings switch-monitor
```

### windows information, window control
https://www.freedesktop.org/wiki/Software/wmctrl/
http://mirrors.kernel.org/ubuntu/pool/universe/w/wmctrl/wmctrl_1.07-7build1_amd64.deb
```sh
# get current window id
CURRENT_WINDOW_ID=$(xprop -root | grep "_NET_ACTIVE_WINDOW(WINDOW)" | cut -d ' ' -f 5)
echo "Window ID: $CURRENT_WINDOW_ID, Border Width: $BORDER_WIDTH"
# get window properties 
xprop -id $CURRENT_WINDOW_ID _NET_FRAME_EXTENTS
```

### custom shortcuts script for finding and activating window, simulate actions by human 
> for windows customization(change position, size, title ... ): https://www.nongnu.org/devilspie2/ (https://github.com/dsalt/devilspie2)
```sh
#!/bin/bash
values=`xdotool search --name 'Visual Studio Code'`
# xdotool getwindowname 75497476
if [[ $? > 0 ]]; then
		  /usr/bin/code &> /dev/null &
	else
    xdotool windowactivate `echo $values | grep '' | awk '{print $1}' | head -n 1`
fi
```

### gnome extension manual installation, gnome ext folder
#### install gnome extension 
```sh
gnome-shell --version
path_to_extension=~/Downloads/switcherlandau.fi.v28.shell-extension.zip

plugin_uuid=`unzip -c $path_to_extension metadata.json | grep uuid | cut -d \" -f4`
plugin_dir="$HOME/.local/share/gnome-shell/extensions/$plugin_uuid"
mkdir -p $plugin_dir
unzip -q $path_to_extension -d $plugin_dir/
sudo systemctl restart gdm
```

#### delete gnome extension
```sh
path_to_extension=~/Downloads/gsconnectandyholmes.github.io.v53.shell-extension.zip

plugin_uuid=`unzip -c $path_to_extension metadata.json | grep uuid | cut -d \" -f4`
if [[ -n $plugin_uuid ]]; then
    plugin_dir="$HOME/.local/share/gnome-shell/extensions/$plugin_uuid"
    rm -rf $plugin_dir
    sudo systemctl restart gdm
else
    echo "plugin folder was not found"
fi
```

## gnome keyring
```text
raise InitError("Failed to unlock the collection!")
```

```sh
# kill all "keyring-daemon" sessions
# clean up all previous runs
rm ~/.local/share/keyrings/*
ls -la ~/.local/share/keyrings/

dbus-run-session -- bash
gnome-keyring-daemon --unlock
# type your password, <enter> <Ctrl-D>
keyring set cc.user cherkavi
keyring get cc.user cherkavi
```
### keyring reset password
```sh
PATH_TO_KEYRING_STORAGE=~/.local/share/keyrings/login.keyring 
mv $PATH_TO_KEYRING_STORAGE "${PATH_TO_KEYRING_STORAGE}-original"
# go to applications->passwords and keys-> "menu:back" -> "menu:passwords"
```

### gnome launch via ssh 
```sh
ssh -Y remoteuser@remotehost dbus-launch -f gedit
ssh -X remoteuser@remotehost dbus-launch gnome-terminal
```

## certification 
Generating a RSA private key
```bash
openssl req -new \
-newkey rsa:2048 \
-nodes -out cherkavideveloper.csr \
-keyout cherkavideveloper.key \
-subj "/C=DE/ST=Bavaria/L=München/O=cherkavi/CN=cherkavi developer" \
# scp -i $AWS_KEY_PAIR cherkavideveloper.csr ubuntu@ec2-52-29-176-00.eu-central-1.compute.amazonaws.com:~/
# scp -i $AWS_KEY_PAIR cherkavideveloper.key ubuntu@ec2-52-29-176-00.eu-central-1.compute.amazonaws.com:~/
```
```bash
openssl req -x509 \
-days 365 \
-newkey rsa:2048 \
-nodes -out cherkavideveloper.pem \
-keyout cherkavideveloper.pem \
-subj "/C=DE/ST=Bavaria/L=München/O=cherkavi/CN=cherkavi developer"
```
## console browsers
* `sudo apt install w3m`
* `sudo apt install linx`
* https://brow.sh/downloads
* elinks  
* `sudo apt install links`
* `sudo apt install links2`

## online http test
* https://webhook.site/  
* https://httpbin.org/  

## local http server http test server
```sh
nc -kdl localhost 8000
# Sample request maker on another shell:
wget http://localhost:8000
```
```sh
npm -g install http-server
http-server
```

## Utilities 
### [web-based terminal](https://github.com/butlerx/wetty)
terminal window in browser

### automation for browsers, 
automate repited actions: iMacros

### md2html, markdown to html, markdown tool 
- [Sphinx](https://www.sphinx-doc.org/en/master/usage/markdown.html)
- [docsify](https://docsify.js.org/)
- [hugo](https://gohugo.io/getting-started/quick-start/)
- [jekyll](https://jekyllrb.com/)
- [eleventy](https://www.11ty.dev/)
- [pelican](https://blog.getpelican.com/)
- [hexo](https://hexo.io/)
- [markdown to html realtime renderer](https://github.com/markserv/markserv)
  ```sh
  npm i -g markserv
  markserv .
  ```
    
```sh
sudo apt-get update
sudo apt-get install -y python3-sphinx
pip3 install recommonmark sphinx-markdown-tables --user
sphinx-build "/path/to/source" "/path/to/build" .
```
```sh
pandoc README.md | lynx -stdin
```
```sh
markdown-it README.md
```
```sh
glow README.md
```

### keepass
```sh
sudo add-apt-repository ppa:jtaylor/keepass
sudo apt-get update && sudo apt-get install keepass2
```
keepassxc-cli
* https://www.mankier.com/1/keepassxc-cli
* https://wiki.gentoo.org/wiki/KeePassXC/cli
```sh
## set key file instead of password
KEEPASS_KEY=/home/projects/keepass.keyx
# create key file: openssl rand -out $KEEPASS_KEY 256
ll $KEEPASS_KEY
# set key file 
keepassxc-cli db-edit --set-key-file  $KEEPASS_KEY  $KEEPASS_FILE 
# check key file with entering password
keepassxc-cli ls --key-file $KEEPASS_KEY $KEEPASS_FILE 
# unset password 
keepassxc-cli db-edit --key-file $KEEPASS_KEY  $KEEPASS_FILE  --unset-password
keepassxc-cli ls --key-file $KEEPASS_KEY $KEEPASS_FILE --no-password

# unset key file 
# keepassxc-cli db-edit --unset-key-file  $KEEPASS_KEY  $KEEPASS_FILE 


# get password
keepassxc-cli show -s -k $KEEPASS_KEY $KEEPASS_FILE  'Client1/Order Value'  --no-password
```

### vnc
#### vnc installation
```sh
sudo apt install tightvncserver
sudo apt install x11vnc
```

#### ~/.vnc/xstartup, file for starting vncserver
[for starting Docker container with UI, vnc with docker](https://github.com/cherkavi/docker-images/blob/master/vnc/docker-virtual-display-vnc.md)

```sh
chmod +x ~/.vnc/xstartup
```

```sh
#!/bin/sh

# sudo apt install xfce4

# Fix to make GNOME and GTK stuff work
export XKL_XMODMAP_DISABLE=1
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
startxfce4 &

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
```

#### vnc start local server
```sh
# check your active display 
DISPLAY=:1 xrandr  --current
DISPLAY=:2 xrandr  --current
DISPLAY=:3 xrandr  --current
```

```sh
# run
x11vnc -display :1 -rfbport 5902
# stop
x11vnc -R stop
```

```sh
# run 
tightvncserver :1 -geometry 1920x1080 -rfbport 5902
ps aux | grep tightvnc

# stop 
tightvncserver -kill :1
tightvncserver -kill :2
```

#### vnc server local start
```sh
# vnc server 
sudo apt install tigervnc-standalone-server
# tigervncserver

## issue on Ubuntu 22.04
# sudo apt install tightvncserver
# tightvncserver

# vncserver -passwordfile ~/.vnc/passwd -rfbport 5900 -display :0
vncserver
# for changing password
vncpasswd
# list of vnc servers 
vncserver -list
# stop vnc server
vncserver -kill :1
# configuration

vim ~/.vnc/xstartup
# xrdb $HOME/.Xresources
# startxfce4 &
```

#### vnc server with connecting to existing X session
```sh
# https://github.com/sebestyenistvan/runvncserver
sudo apt install tigervnc-scraping-server

## password for VNC server
vncpasswd

## start vnc server 
X0tigervnc -PasswordFile ~/.vnc/passwd
# the same as: `x0vncserver -display :0`
x0vncserver -passwordfile ~/.vnc/passwd -rfbport 5900 -display :0

## list of the servers
x0vncserver -list

## log files 
ls $HOME/.vnc/*.log

x0vncserver -kill :1
```

#### vnc start, x11vnc start, connect to existing display, vnc for existing display
```sh
# export DISPLAY=:0
# Xvfb $DISPLAY -screen 0 1920x1080x16 &
# Xvfb $DISPLAY -screen 0 1920x1080x24 # not more that 24 bit for color

# startxfce4 --display=$DISPLAY &

# sleep 1
x11vnc -quiet -localhost -viewonly -nopw -bg -noxdamage -display $DISPLAY &

# just show current desktop 
x11vnc
```

#### vnc commands
```sh
# start server
vncserver -geometry 1920x1080
# full command, $DISPLAY can be ommited in favoud to use "next free screen"
vncserver $DISPLAY -rfbport 5903 -desktop X -auth /home/qqtavt1/.Xauthority -geometry 1920x1080 -depth 24 -rfbwait 120000 -rfbauth /home/qqtavt1/.vnc/passwd  -fp /usr/share/fonts/X11/misc,/usr/share/fonts/X11/Type1 -co /etc/X11/rgb

## Couldn't start Xtightvnc; trying default font path.
## Please set correct fontPath in the vncserver script.
## Couldn't start Xtightvnc process.

# start server with new monitor
vncserver -geometry 1920x1080 -fp "/usr/share/fonts/X11/misc,/usr/share/fonts/X11/Type1,built-ins"

# check started
ps aux | grep vnc
# kill server
vncserver -kill :1
```

#### vnc client, vnc viewer, vnc player
```sh
# !!! don't use Remmina !!!
sudo apt install xvnc4viewer
```


###  timer, terminal timer, console timer
```
sudo apt install sox libsox-fmt-mp3
https://github.com/rlue/timer
sudo curl -o /usr/bin/timer https://raw.githubusercontent.com/rlue/timer/master/bin/timer
sudo chmod +x /usr/bin/timer
# set timer for 5 min 
timer 5
```

## vim
[vim cheat sheet](http://najomi.org/vim)

### vim pipe
```sh
echo "hello vim " | vim - -c "set number"
```

### copy-paste
* v - *visual* selection ( start selection )
* y - *yank* ( end selection )
* p - *paste* into position
* u - *undo* last changes
* ctrl-r - *redo* last changes

### read output of command 
```
:read !ls -la
```

### vim execute selection  
```
1) select text with v-visual mode
2) semicolon
3) w !sh
:'<,'>w !sh
```

### [vim plugin](https://github.com/junegunn/vim-plug)
#### vim plugin managers
* [Vundle](https://github.com/VundleVim/Vundle.vim)
* [pathogen](https://github.com/tpope/vim-pathogen)

file ```~/.vimrc``` should have next content: 
```
if empty(glob('~/.vim/autoload/plug.vim'))
  silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
    \ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
  autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif

call plug#begin('~/.vim/plugged')
Plug 'junegunn/seoul256.vim'
Plug 'junegunn/goyo.vim'
Plug 'junegunn/limelight.vim'
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
" Plug 'andreshazard/vim-logreview'
" Plug 'dstein64/vim-win'

call plug#end()

set laststatus=2
set ignorecase
set smartcase
set number
set nocompatible
filetype on
set incsearch
set hlsearch
```  

or 
```sh
git clone --depth=1 https://github.com/vim-airline/vim-airline ~/.vim/plugged/vim-airline
git clone --depth=1 https://github.com/dstein64/vim-win ~/.vim/plugged/vim-win
```
```sh
vim anyfile.txt
:PlugInstall
```

### .vim folder example
```
.vim
├── autoload
│   └── plug.vim
├── colors
│   └── wombat.vim
├── pack
│   └── plugins
└── plugged
    ├── goyo.vim
    ├── lightline.vim
    ├── limelight.vim
    ├── seoul256.vim
    ├── vim-airline
    └── vim-airline-themes
```


## vifm
### colorschema
copy to ```~/.config/vifm/colors``` [color scheme](https://vifm.info/colorschemes.shtml)  
```:colorscheme <tab>```

## visual code extensions
[create custom](https://code.visualstudio.com/api/get-started/your-first-extension)
```sh
npx --package yo --package generator-code -- yo code
```
select option "open in code"  
run in "debug" mode - will open another 'code' with your extension

```sh
## common
codium --install-extension nick-rudenko.back-n-forth
codium --install-extension alefragnani.numbered-bookmarks
codium --install-extension rockingskier.copy-copy-paste
codium --install-extension mksafi.find-jump
codium --install-extension jacobdufault.fuzzy-search
codium --install-extension qcz.text-power-tools
codium --install-extension redhat.vscode-commons
codium --install-extension visualstudioexptteam.vscodeintellicode
codium --install-extension foam.foam-vscode
codium --install-extension devwright.vscode-terminal-capture
# markdown
codium --install-extension tchayen.markdown-links
codium --install-extension kortina.vscode-markdown-notes
codium --install-extension yzhang.markdown-all-in-one
codium --install-extension gera2ld.markmap-vscode
# json
codium --install-extension mohsen1.prettify-json
codium --install-extension vthiery.prettify-selected-json
codium --install-extension richie5um2.vscode-statusbar-json-path

## common-ext
codium --install-extension GitHub.copilot # don't install for pytest
codium --install-extension atlassian.atlascode
codium --install-extension ms-vscode-remote.remote-containers
codium --install-extension ms-vscode-remote.remote-ssh
codium --install-extension ms-vscode-remote.remote-ssh-edit
codium --install-extension liximomo.remotefs

## git
codium --install-extension donjayamanne.githistory
codium --install-extension qezhu.gitlink
codium --install-extension TeamHub.teamhub

## containers
codium --install-extension peterjausovec.vscode-docker
codium --install-extension ms-azuretools.vscode-docker

## shell 
codium --install-extension inu1255.easy-shell
codium --install-extension ryu1kn.edit-with-shell
codium --install-extension ms-toolsai.jupyter-renderers
codium --install-extension devwright.vscode-terminal-capture
codium --install-extension miguel-savignano.terminal-runner
codium --install-extension tyriar.terminal-tabs

## jupyter
codium --install-extension ms-toolsai.jupyter
codium --install-extension ms-toolsai.jupyter-keymap

## java
codium --install-extension vscjava.vscode-java-dependency
codium --install-extension vscjava.vscode-java-pack
codium --install-extension vscjava.vscode-java-test
codium --install-extension redhat.java
codium --install-extension vscjava.vscode-maven
codium --install-extension vscjava.vscode-java-debug

## python
codium --install-extension ms-python.python
codium --install-extension ms-python.vscode-pylance
codium --install-extension ms-pyright.pyright

## scala 
codium --install-extension scala-lang.scala

## sql
codium --install-extension mtxr.sqltools
codium --install-extension mtxr.sqltools-driver-mysql
```

## taskwarrior
```sh
task add what I need to do
task add wait:2min  finish call
task waiting
task 25 modify wait:2min
task 25 edit
task 25 delete
task 25 done
task project:'BMW'
task priority:high 
task next
```
doc:
* https://taskwarrior.org/docs/using_dates.html
* https://taskwarrior.org/docs/durations.html

extension:
* https://github.com/ValiValpas/taskopen
  installation issue: 
```sh
sudo cpan JSON
```
  commands:
```sh 
  task 13 annotate -- ~/checklist.txt
  task 13 annotate https://translate.google.com
  task 13 denotate
  taskopen 1

  # add notes
  task 1 annotate Notes
  taskopen 1
```

## Terminator
### plugins
* https://askubuntu.com/questions/700015/set-path-for-terminator-to-lookup-for-plugins
* https://github.com/gstavrinos/terminator-jump-up
* https://github.com/mikeadkison/terminator-google/blob/master/google.py

## file manager 
### ranger
#### ranger rifle
```sh
ls -la ~/.config/ranger/rifle.conf
## create if, if not exist
# ranger --copy-config=rifle

## example of making default pdf reader
ext pdf, has okular,   X, flag f = okular -- "$@"
```

## bluejeans installation ubuntu 18+
```sh
# retrieve all html anchors from url, html tags from url
curl -X GET https://www.bluejeans.com/downloads | grep -o '<a .*href=.*>' | sed -e 's/<a /\n<a /g' | sed -e 's/<a .*href=['"'"'"]//' -e 's/["'"'"'].*$//' -e '/^$/ d' | grep rp

sudo alien --to-deb bluejeans-1.37.22.x86_64.rpm 
sudo dpkg -i bluejeans_1.37.22-2_amd64.deb 

sudo apt install libgconf-2-4 
sudo ln -s /lib/x86_64-linux-gnu/libudev.so.1 /lib/x86_64-linux-gnu/libudev.so.0

sudo ln -s /opt/bluejeans/bluejeans-bin /usr/bin/bluejeans
```

## smb client, samba client
```
smbclient -U $SAMBA_CLIENT_GROUP//$SAMBA_CLIENT_USER \
//europe.ubs.corp/win_drive/xchange/Zurich/some/folder
```

## tiling window manager i3wm, i3 desktop
Alternatives:
* [miracle](https://wiki.miracle-wm.org/latest/)
* [Awesome](https://awesomewm.org/) 
* [Bspwm](https://github.com/baskerville/bspwm) 
* [dwm](https://dwm.suckless.org/) 
* [EXWM](https://github.com/ch11ng/exwm) 
* [Herbstluftwm](https://herbstluftwm.org/) 
* [NotionWM](https://notionwm.net/) 
* [Qtile](https://github.com/qtile/qtile) 
* [spectrwm](https://github.com/conformal/spectrwm) 
* [StumpWM](https://stumpwm.github.io/) 
* [Sway](https://swaywm.org/) 
* [wmii](https://github.com/0intro/wmii) 
* [Hyprland](https://hyprland.org/) 
* [River](https://github.com/riverwm/river)
* [xmonad](https://xmonad.org/)
* 
### [custom status bar](https://py3status.readthedocs.io/en/latest/intro.html#installation)
### config file
```sh
vim ~/.config/i3/config
```
### exit from i3 window manager
```
i3-msg exit
#bindsym $mod+Shift+e exec i3-msg exit
```
### keyboard layout add to config file
```sh
exec "setxkbmap -layout us,de"
exec "setxkbmap -option 'grp:alt_shift_toggle'"
```

## icaclient citrix 
### [download receiver](https://www.citrix.de/downloads/citrix-receiver/)
### [download for linxu](https://www.citrix.com/downloads/workspace-app/linux/workspace-app-for-linux-latest.html)
### sudo apt remove icaclient
```sh
sudo dpkg --add-architecture i386
```
### install dependencies
```sh
#sudo apt-get install ia32-libs ia32-libs-i386 libglib2.0-0:i386 libgtk2.0-0:i386
sudo apt-get install libglib2.0-0:i386 libgtk2.0-0:i386
sudo apt-get install gcc-multilib
sudo apt-get install libwebkit-1.0-2:i386 libwebkitgtk-1.0-0:i386
sudo dpkg --install icaclient_13.10.0.20_amd64.deb
```

### mc color, midnight commander
file:~/.mc/ini
```
[Colors]
base_color=normal=brightgray,black:marked=brightcyan,black:selected=black,lightgray:directory=white,black:errors=red,black:executable=brightgreen,black:link=brightblue,black:stalelink=red,black:device=brightmagenta,black:special=brightcyan,black:core=lightgray,black:menu=white,black:menuhot=brightgreen,black:menusel=black,white:editnormal=brightgray,black:editmarked=black,brightgreen:editbold=brightred,cyan
```
```
mc --nocolor
```


### install ssh server, start ssh server, server ssh
```
# sudo apt install openssh-server
sudo apt install ssh

sudo service ssh start

# sudo systemsctl status ssh
sudo service ssh status

# firewall ubuntu
sudo ufw allow ssh

# configuration
sudo vim /etc/ssh/sshd_config
```
for enabling/disabling password using
```text
PasswordAuthentication yes
```
ssh server without password ssh with rsa
1. copy public key 
```sh
# ssh-keygen -b 4096
cat $USER/.ssh/id_rsa.pub secret_user_rsa.pub
```
2. to ssh server
```sh
# touch ~/.ssh/authorized_keys; chmod 600 ~/.ssh/authorized_keys
cat secret_user_rsa.pub >> $USER/.ssh/authorized_keys
```
3. change config
```properties
# sudo vim /etc/ssh/sshd_config
PubkeyAuthentication yes
PasswordAuthentication no
AuthorizedKeysFile .ssh/authorized_keys
```
4. restart service
5. connect with existing user on remote server ( you can also can specify -i ~/.ssh/id_rsa )

### nfs server
#### nfs install
```sh
sudo apt install nfs-kernel-server
systemctl status nfs-server
nfsstat
```
#### nfs create mount point
```sh
# create point 
sudo mkdir /mnt/disks/k8s-local-storage1
# mount 
sudo mount /dev/sdc /mnt/disks/k8s-local-storage1
sudo chmod 755 /mnt/disks/k8s-local-storage1
# createlink 
sudo ln -s /mnt/disks/k8s-local-storage1/nfs nfs1

# update storage
sudo cat /etc/exports
# /mnt/disks/k8s-local-storage1/nfs       10.55.0.0/16(rw,sync,no_subtree_check)

# restart 
sudo exportfs -a
sudo exportfs -v
```

#### nfs parameters
```sh
ll /sys/module/nfs/parameters/
ll /sys/module/nfsd/parameters/
```

#### remote client for nfs mapping
```sh
sudo vim /etc/fstab
# 10.55.0.3:/mnt/disks/k8s-local-storage/nfs /mnt/nfs nfs rw,noauto,x-systemd.automount,x-systemd.device-timeout=10,timeo=14 0 0
# 10.55.0.3:/mnt/disks/k8s-local-storage1/nfs /mnt/nfs1 nfs defaults 0 0

# refresh mapping
sudo mount -av
```

### youtube
#### youtube installation
* [installation](https://ytdl-org.github.io/youtube-dl/download.html)  
* yt-dlp
  ```sh
  ## installation
  # https://github.com/yt-dlp/yt-dlp/release
  sudo wget https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -O yt-dlp; chmod a+rx yt-dlp
  curl -fsSL https://deno.land/install.sh | sh

  # usage
  URL_TO_VIDEO=https://www.youtube.com/shorts/KP4cUVKQ8Jo

  ./yt-dlp https://www.youtube.com/playlist?list=example
  ./yt-dlp $URL_TO_VIDEO
  ./yt-dlp -x --audio-format mp3 $URL_TO_VIDEO
  ./yt-dlp -x --js-runtimes deno --list-subs $URL_TO_VIDEO
  ```
* [alternative installation](https://github.com/ytdl-org/youtube-dl/releases)
  ```sh
  chmod +x youtube-dl
  # check your /usr/bin/pyton and fix header in the file otherwise
  sudo mv youtube-dl /usr/bin/
  ```
```
youtube-dl --list-formats https://www.youtube.com/watch?v=nhq8e9eE_L8
youtube-dl --format 22 https://www.youtube.com/watch?v=nhq8e9eE_L8
```
#### youtube subtitles
```sh
YT_URL=...
youtube-dl --list-subs $YT_URL
YT_LANG=de
# original subtitles
youtube-dl --write-sub --sub-lang $YT_LANG $YT_URL
# autogenerated subtitles
youtube-dl --write-auto-sub --sub-lang $YT_LANG --skip-download $YT_URL
```
or direct from browser find:
https://www.youtube.com/api/timedtext...

#### youtube view counter
```sh
VIDEO_URL="https://www.youtube.com/watch?v=Rppjx10EeQo"
curl $VIDEO_URL | hq . | grep interactionCount | awk '{print $2}' | awk -F '=' '{print $2}'
```

### screen video recording, screen recording
```sh
# start recording
# add-apt-repository ppa:savoury1/ffmpeg4 && apt update && apt install -y ffmpeg
ffmpeg -y -video_size 1280x1024 -framerate 20 -f x11grab -i :0.0 /output/out.mp4

# stop recording
ps aux | grep ffmpeg | head -n 1 | awk '{print $2}' | xargs kill --signal INT 
```
### video metadata
```sh
sudo apt install mediainfo
mediainfo video.mp4
mediainfo -f video.mp4
```

### image format, image size, image information, image metadata
```sh
# sudo apt-get install imagemagick
identify -verbose image.png

# https://imagemagick.org/script/escape.php
identify -format "%m" image.png     # format type 
identify -format "%wx%h" image.png  # width x height
```

### image resize, image size, image rotation, image scale 
```sh
# sudo apt-get install imagemagick
# without distortion
convert marketing.png -resize 100x100 marketing-100-100.png
# mandatory size, image will be distorted
convert marketing.png -resize 100x100 marketing-100-100.png
# rotate and change quality
convert marketing.png -rotate 90 -charcoal 4 -quality 50 marketing.png
```

```sh
# merge pdf files
convert 1.pdf 2.pdf 3.pdf result.pdf
# Error: no image defined
# /etc/ImageMagick-6/policy.xml
# <policy domain="coder" rights="read|write| pattern="PDF" />
```

### image cut image crop
```sh
WIDTH=200
HEIGHT=200
X=10
Y=20
convert input.jpg -crop $WIDTHx$HEIGHT+$X+$Y output.jpg
```

### image change color image black and white image monochrome imagemagic 
```sh
convert $IMAGE_ORIGINAL -monochrome $IMAGE_CONVERTED
convert $IMAGE_ORIGINAL -remap pattern:gray50 $IMAGE_CONVERTED
convert $IMAGE_ORIGINAL -colorspace Gray $IMAGE_CONVERTED
convert $IMAGE_ORIGINAL -channel RGB -negate $IMAGE_CONVERTED
```

### image text recognition ocr, text from image, text recognition
```sh
gocr $IMAGE_POST_CONVERTED
# for color image play with parameter 0%-100% beforehand
convert $IMAGE_ORIGINAL -threshold 75% $IMAGE_CONVERTED
```

### get image info image metadata
```
exiftool my_image.jpg
exif my_image.jpg
identify -verbose my_image.jpg
```

### image remove gps remove metadata cleanup
```sh
exiftool -gps:all= *.jpg
```

### image remove all metadata
```sh
exiftool -all= *.jpg
```

### image tags
```sh
# tags list: https://exiftool.org/TagNames
# sub-elements: https://exiftool.org/TagNames/GPS.html
exiftool -GPS:GPSLongitude *.jpg

exiftool -filename  -gpslatitude -gpslongitude  *.jpg
exiftool -filename  -exif:gpslongitude  *.jpg
```

### top
top hot keys:
* t - change graphical representation
* e - change scale
* z - color
* c - full command
* d - delay 
* o - filter ( COMMAND=java )

## ngrok
```sh
# ngrok install
sudo snap install ngrok
# ngrok setup 
x-www-browser https://dashboard.ngrok.com/get-started/setup
ngrok config add-authtoken aabbccddeeffgg

ngrok config check

x-www-browser https://dashboard.ngrok.com/tunnels/agents

# how to start as a service
# https://github.com/cherkavi/cheat-sheet/blob/master/linux.md#ngrok
```

## stress test memory test
```sh
apt update; apt install -y stress
```
start process with occupying certainly 100 Mb
```sh
stress --vm 1 --vm-bytes 100M
```

## boot loader efi 
```sh
sudo apt install efibootmgr
efibootmgr -v

# boot order, boot descriptions
efibootmgr

## set bootorder
# read
efibootmgr | grep BootOrder
# write 
sudo efibootmgr --bootorder 0002,0000,0004,0007,0003,0006

```
if you have lost ability to start bootloader from current drive or you can't see anymore normal boot loader
```sh
sudo apt install grub-efi
sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

```sh
lsblk
# sda           8:0    0   477G  0 disk 
# ├─sda1        8:1    0   512M  0 part 
# └─sda2        8:2    0 476,4G  0 part /media/qxxxxxx/26d89655-ea1a-4922-9724-9a7a25


DISK_ID=26d89655-ea1a-4922-9724-9a7a25
ls /media/${USER}/${DISK_ID}/boot/efi
sudo mount /dev/sda1 /media/${USER}/${DISK_ID}/boot/efi

sudo grub-install /dev/sda --target=x86_64-efi --efi-directory=/media/${USER}/${DISK_ID}/boot/efi

ls -la /media/${USER}/${DISK_ID}/boot/efi/EFI
# ls -la /media/${USER}/${DISK_ID}/boot/efi/EFI/BOOT
# ls -la /media/${USER}/${DISK_ID}/boot/efi/EFI/ubuntu

mkdir /tmp/bootloader-efi
cp -r /media/${USER}/${DISK_ID}/boot/efi/EFI /tmp/bootloader-efi
ls /tmp/bootloader-efi
```

```sh
DISK_ID=26d89655-ea1a-4922-9724-9a7a25
ls /media/${USER}/${DISK_ID}/boot/efi
# re-attach drive 
sudo cp -r /tmp/bootloader-efi/* /media/${USER}/${DISK_ID}/boot/efi
sudo ls -la /media/${USER}/${DISK_ID}/boot/efi/
# sudo rm -rf /media/${USER}/${DISK_ID}/boot/efi/bootloader-efi
```

## [mail url rules mailto](https://yoast.com/developer-blog/guide-mailto-links/)
| mailto:  |to set the recipient, or recipients, separate with comma                                                                         |
| &cc=     |to set the CC recipient(s)                                                                                                       |
| &bcc=    |to set the BCC recipient(s)                                                                                                      |
| &subject=|to set the email subject, URL encode for longer sentences, so replace spaces with %20, etc.                                      |
| &body=   |to set the body of the message, you can add entire sentences here, including line breaks. Line breaks should be converted to %0A.|

## mail console client
### aerc
```sh
## create application password: https://myaccount.google.com/u/1/apppasswords
## vim ~/.config/aerc/accounts.conf

## vim ~/.config/aerc/aerc.conf
# [filters]
# text/html = "w3m -T text/html"
sudo apt install aerc 
```

### mutt
```sh
# aerc; alpine, neomutt
# sudo apt-get install alpine
sudo apt-get install mutt  # For Debian/Ubuntu
mkdir -p ~/.mutt/cache/
mkdir -p "~/.mutt/cache/headers"
mkdir -p "~/.mutt/cache/bodies"

```

#### mutt setup pop3
```sh
echo "
set pop_host = $POP3_HOST
set pop_user = $POP3_USER
set pop_pass = $POP3_PASS
set pop_port = $POP3_PORT
set from = $POP3_EMAIL
set realname = $POP3_USER_TITLE
" > ~/.muttrc
```

### mutt setup imap
```sh
echo "
set imap_user = $IMAP_USER
set imap_pass = $IMAP_PASS
# set imap_port = $IMAP_PORT
set folder = $IMAP_FOLDER # "imaps://imap.example.com/"
set spoolfile = "+INBOX"
set postponed = "+[Gmail]/Drafts"
set header_cache = "~/.mutt/cache/headers"
set message_cachedir = "~/.mutt/cache/bodies"

# Other settings
set from = $IMAP_EMAIL
set realname = $IMAP_TITLE
" > ~/.muttrc
# cat ~/.muttrc
```

```sh
mutt
```
