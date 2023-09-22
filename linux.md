# Linux Debian cheat sheet

* cheat sheets
    * [cheat sheet cmd](http://cheat.sh/)
    ```sh
    curl cheat.sh/ls
    ```
    * TooLongDontRead     
    ```sh
    # npm install -g tldr
    tldr ls
    ```
    * how to 
    ```sh
    # npm install -g how-2
    how2 ls
    ``` 
    
* [Security cheat sheet](https://www.jaiminton.com/cheatsheet/DFIR)

### commands gnu commands
```sh
time 
# run GNU version of the command:
/etc/bin/time
# or 
\time 
```
### retrieve human readable information from binary files
```sh
strings /usr/share/teams/libEGL.so | grep git
```

### place for scripts, permanent script
```
### system wide for all users
# System-wide .bashrc file for interactive bash(1) shells.
/etc/bash.bashrc
# system-wide .profile file for the Bourne shell sh(1)
/etc/profile
/etc/environment
/etc/profile.d/my_new_update.sh

### during user login
~/.bash_profile
# executed by Bourne-compatible login shells.
~/.profile 

# during open the terminal, executed by bash for non-login shells.
~/.bashrc
```

### socket proxy, proxy to remote machine
```
ssh -D <localport> <user>@<remote host>
```
and checking if it is working for 'ssh -D 7772 cherkavi@15pos1.190.211.1'
```
ssh -o "ProxyCommand nc -x 127.0.0.1:7772 %h %p" cherkavi@151.190.211.47
```

### jumpserver proxy jump host bastion 
```sh
# ssh root@35.35.13.49 -o "proxycommand ssh -W %h:%p -i /home/admin/.ssh/identity-bastionhost ubuntu@10.0.12.10"  -t "su - admin" 
# ssh root@35.35.13.49 -t "su admin"  ssh -i /home/admin/.ssh/identity-bastionhost ubuntu@10.0.12.10
# ssh root@35.35.13.49 "su admin && ssh -i /home/admin/.ssh/identity-bastionhost ubuntu@10.0.12.10"
# ssh root@35.35.13.49 -t "su admin" "ssh -i /home/admin/.ssh/identity-bastionhost ubuntu@10.0.12.10"
# ssh root@35.35.13.49 -t "su admin"  ubuntu@10.0.12.10

# localhost ---->  35.35.13.49 ---->   10.0.12.10
# all identity files must be placed on localhost !!!
ssh -Ao ProxyCommand="ssh -W %h:%p -p 22 $PROD_BASTION_USER@$PROD_BASTION_HOST" -i $EC2_INVENTORY -p EC2_AIRFLOW_PORT $EC2_AIRFLOW_USER@$EC2_AIRFLOW_HOST $*
```
### scp bastion scp proxy scp copy via bastion
```sh
scp -o "ProxyCommand ssh -W %h:%p -p 22 $PROD_BASTION_USER@$PROD_BASTION_HOST" -i $EC2_KEY 1.md $EC2_USER@$EC2_HOST:~/
```

### scp error 
```
bash: scp: command not found
```
solution: scp should be installed on both!!! hosts

### tunnel, port forwarding from local machine to outside
```sh
ssh -L <local_port>:<remote_host from ssh_host>:<remote_port> <username>@<ssh_host>
# ssh -L 28010:remote_host:8010 user_name@remote_host
ssh -L <local_port>:<remote_host from ssh_host>:<remote_port> <ssh_host>
# ssh -L 28010:vldn337:8010 localhost

# destination service on the same machine as ssh_host
# localport!=remote_port (28010!=8010)
ssh -L 28010:127.0.0.1:8010 user_name@remote_host
```

from local port 7000 to remote 5005
```
ssh -L 7000:127.0.0.1:5005 cherkavi@134.190.200.201
```

browser(ext_host) -> 134.190.2.5 -> 134.190.200.201
```
user@134.190.2.5:~$ ssh -L 134.190.2.5:8091:134.190.200.201:8091 cherkavi@134.190.200.201
user@ext_host:~$ wget 134.190.2.5:8091/echo 
```

### tunnel, port forwarding from outside to localmachine
```sh
# ssh -R <remoteport>:<local host name>:<local port> <hostname>
# localy service on port 9092 should be started
# and remotelly you can reach it out just using 127.0.0.1:7777
ssh -R 7777:127.0.0.1:9092 localhost
```

### tunnel for remote machine with proxy, local proxy for remote machine, remote proxy access
//TODO
local=======>remote   
after that, remote can use local as proxy

first of all start local proxy (proxychains or redsock)
```sh
sudo apt install privoxy
sudo vim /etc/privoxy/config
# listen-address  127.0.0.1:9999
# forward-socks5t / http://my-login:passw@proxy.zur:8080 .
# forward-socks4a / http://my-login:passw@proxy.zur:8080 .
# or 
# forward   /      http://my-login:passw@proxy.zur:8080
systemctl start privoxy
```

```sh
# locally proxy server on port 9999 should be started
ssh -D 9999 127.0.0.1 -t ssh -R 7777:127.0.0.1:9999 username@192.118.112.13

# from remote machine you can execute 
wget -e use_proxy=yes -e http_proxy=127.0.0.1:7777 https://google.com
```

### ssh suppress banner, ssh no invitation
```sh
ssh -q my_server.org
```

### ssh verbosive, ssh log, debug ssh
```sh
ssh -vv my_server.org
```

### ssh variable ssh envvar ssh send variable
#### ssh variable in command line
```sh
ssh -t user@host VAR1="Petya" bash -l
```
#### sshd config
locally: ~/.ssh/config 
```sh
SendEnv MY_LOCAL_VAR
```
remotely: /etc/ssh/sshd_config
```sh
AcceptEnv MY_LOCAL_VAR
```
#### ssh environment
```sh
echo "VAR1=Hello" > sshenv
echo "VAR2=43" >> sshenv
scp sshenv user@server:~/.ssh/environment
ssh user@server myscript
```

### ssh run command
```sh
ssh -t user@host 'bash -s' < my-script.sh
# with arguments
ssh -t user@host 'bash -s' -- < my-script.sh arg1 arg2 arg3
# ssh document here 
ssh -T user@host << _dochere_marker
cd /tmp
echo $(date) >> visit-marker.txt
_dochere_marker
```

### local proxy cntlm, cntlm proxy
```text
  app_1 --.
           \
  app_2 --- ---> local proxy <---> External Proxy <---> WWW
   ...     /
  app_n --'
```
> install cntlm
```sh
# temporarily set proxy variables for curl and brew to work in this session
$ export http_proxy=http://<user>:<password>@proxy-url:proxy-port
$ export https_proxy=$http_proxy

# update & upgrade apt
$ sudo --preserve-env=http_proxy,https_proxy apt-get update
$ sudo --preserve-env=http_proxy,https_proxy apt-get upgrade

# finally, install cntlm
sudo --preserve-env=http_proxy,https_proxy apt-get install cntlm
```
> edit configuration
```sh
vim ~/.config/cntlm/cntlm.conf
```
```
Username user-name
Domain  domain-name
Proxy   proxy-url:proxy-port
NoProxy localhost, 127.0.0.*, 10.*, 192.168.*, *.zur
Listen  3128
```
or globally
```
sudo vim /etc/cntlm.conf
```
> ~/bin/proxy-start.sh
```sh
#!/bin/sh

pidfile=~/.config/cntlm/cntlm.pid

if [ -f $pidfile ]; then
kill "$(cat $pidfile)"
sleep 2
fi

cntlm -c ~/.config/cntlm/cntlm.conf -P $pidfile -I
```
> source ~/bin/proxy-settings.sh
```
proxy_url="http://127.0.0.1:3128"
export http_proxy=$proxy_url
export https_proxy=$http_proxy
export HTTP_PROXY=$http_proxy
export HTTPS_PROXY=$http_proxy

export _JAVA_OPTIONS="-Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=3128 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=3128 -Dhttps.nonProxyHosts=localhost|*.ubsgroup.net|*.muc -Dhttp.nonProxyHosts=localhost|*.ubsgroup.net|*.zur"
```
> check status
```sh
sudo invoke-rc.d cntlm status
ss -lt | grep 3128
```

### possible solution to detect remote client to your machine
```
# open access
ping -s 120 -c 1 146.255.193.66
ping -s 121 -c 1 146.255.193.66
ping -s 122 -c 1 146.255.193.66

# close access
ping -s 123 -c 1 146.255.193.66
```

### open ports, open connections, listening ports, application by port, application port, process port, pid port
```sh
# list of open files
sudo lsof -i -P -n | grep LISTEN
# list of files for specific user
lsof -u my_own_user 

# limit of files for user
ulimit -a


# list of open connections
sudo netstat -tulpan | grep LISTEN
sudo ss -tulwn | grep LISTEN

# list of open ports
sudo nmap -sT -O 127.0.0.1

# print pid of process that occupying 9999 port
sudo ss -tulpan 'sport = :9999'

# open input output
iotop

# list of services mapping service to port mapping port to service
less /etc/services
```

### mount drive to path mount
```
# <drive> <path>
sudo mount /dev/sdd /tin
```

### mount remote filesystem via ssh, map folder via ssh, ssh remote folder
```
sudo mkdir /mnt/vendor-cluster-prod
sudo sshfs -o allow_other,IdentityFile=~/.ssh/id_rsa vcherkashyn@190.17.19.11:/remote/path/folder /mnt/vendor-cluster-prod
# sudo fusermount -u /remote/path/folder
# sudo umount /remote/path/folder
```

### mount remote filesystem via ftp
```sh
sudo apt install curlftpfs
sudo mkdir /mnt/samsung-note
curlftpfs testuser:testpassword@192.168.178.20:2221 /mnt/samsung-note/
```

### samsung phone android phone folder
```sh
cd /run/user/1000/gvfs
# phone samsung
cd /run/user/1000/gvfs/mtp:host=SAMSUNG_SAMSUNG_Android_RFxxxxxxxxx
```

### mount windows folder, mount windows shared folder
```sh
sudo apt install nfs-common
sudo apt install cifs-utils

sudo mkdir -p /mnt/windows-computer
USER_NAME='my-username'
USER_DOMAIN='ZUR'
USER_SERVER='//u015029.ubsbank.net/home$/x453337/'
sudo mount -t cifs -o auto,gid=$(id -g),uid=$(id -u),username=$USER_NAME,domain=$USER_DOMAIN,vers=2.1 $USER_SERVER /mnt/windows-computer
```
#### mount issue
```
bad option;  for several filesystems (e.g. nfs, cifs) you might need a /sbin/mount.<type> helper program.
```
```
sudo apt-get install nfs-common
sudo apt-get install cifs-utils
```

### mount usb drive temporary mount disk
```sh
sudo mount /dev/sdd    /media/tina-team
```
### unmount usb detach usb
```sh
umount /dev/sdd
```

### mount usb drive permanently mount, map drive
```sh
sudo mkdir /mnt/disks/k8s-local-storage1
sudo chmod 755 /mnt/disks/k8s-local-storage1
sudo ln -s /mnt/disks/k8s-local-storage1/nfs nfs1
ls -la /mnt/disks
ls -la /mnt

sudo blkid
sudo vim /etc/fstab
# add record
# UUID=42665716-1f89-44d4-881c-37b207aecb71 /mnt/disks/k8s-local-storage1 ext4 defaults 0 0

# refresg fstab reload
sudo mount -av
ls /mnt/disks/k8s-local-storage1
```
option 2
```sh
sudo vim /etc/fstab
# add line
# /dev/disk/by-uuid/8765-4321    /media/usb-drive         vfat   0   0

# copy everything from ```mount```
# /dev/sdd5 on /media/user1/e91bd98f-7a13-43ef-9dce-60d3a2f15558 type ext4 (rw,nosuid,nodev,relatime,uhelper=udisks2)
# /dev/sda1 on /media/kali/usbdata type fuseblk (rw,nosuid,nodev,relatime,user_id=0,group_id=0,default_permissions,allow_other,blksize=4096,uhelper=udisks2)

# systemctl daemon-reload

sudo mount -av
```

mount remote drive via network 
```
10.55.0.3:/mnt/disks/k8s-local-storage/nfs /mnt/nfs nfs rw,noauto,x-systemd.automount,x-systemd.device-timeout=10,timeo=14 0 0
```

### drive uuid hdd uuid
```
blkid
```

### list drives, drive list, attached drives
```sh
lsblk
fdisk -l
```

### create filesystem, format drive
```sh
sudo mkfs -t xfs /dev/xvdb
```
```sh
sudo mke2fs /dev/xvdb
```

### gpg signature check, asc signature check, crt signature check
```sh
kgpg --keyserver keyserver.ubuntu.com --recv-keys 9032CAE4CBFA933A5A2145D5FF97C53F183C045D
gpg --import john-brooks.asc
```
```sh
gpg --verify ricochet-1.1.4-src.tar.bz2.asc
```
in case of error like:
```
gpg: Can't check signature: No public key
```
```sh
gpg --import gpg-pubkey.txt
gpg --verify openldap-2.5.13.tgz.asc
```

### [authenticator 2fa](https://www.nongnu.org/oath-toolkit/man-oathtool.html)
```sh
sudo apt install oathtool
oathtool -b --totp $CODE_2FA
```
> oathtool: base32 decoding failed: Base32 string is invalid

### qr code generator
```sh
# install 
sudo apt install qrencode
# generate qr code
qrencode --size 6 --level H --output="test-text.png" "test text"
echo "output from pipe" | qrencode --size 6 --level H --output="test-text.png" 
```

### bar code scanner qr code scanner
```sh
# bar code scanner QR code scanner
sudo apt install zbar-tools
zbarimg ~/path-to-screenshot-of-barcode.png
```

### connect to remote machine via ssh without credentials
```
# generate new RSA keys, create RSA, generate keys
ssh-keygen -t rsa
```
( check created file /home/{user}/.ssh/id_rsa )
```sh
# if you have copied it, check permissions
chmod 700 ~/.ssh
chmod 700 ~/.ssh/*
```
#### print public ssh keys, ssh public keys
```sh
cat ~/.ssh/id_rsa.pub
```

### passphrase skip typing ssh-keygen without passphrase, avoid Enter passphrase for key
```sh
eval `ssh-agent -s` 
ssh-add $HOME/.ssh/id_rsa
```

#### login without typing password
```sh
# ssh
sshpass -p my_password ssh my_user@192.178.192.10
# ftp 
sshpass -p $CHINA_PASS sftp -P $CHINA_JUMP_SERVER_PORT $CHINA_USER@$CHINA_JUMP_SERVER
```
#### login without typing password
```sh
echo $my_password | ssh my_user@192.178.192.10
```

#### copy ssh key to remote machine, 
```sh
ssh-copy-id {username}@{machine ip}:{port}
ssh-copy-id -i ~/.ssh/id_rsa.pub -o StrictHostKeyChecking=no vcherkashyn@bmw000013.adv.org
# manual execution
cat ~/.ssh/id_rsa.pub | ssh vcherkashyn@bmw000013.adv.org 'cat >> ~/.ssh/authorized_keys'

# output nothing when ssh key exists, ssh check
ssh-copy-id user@ubssp000013.vantagedp.com 2>/dev/null
```
after copying you can use ssh connection with inventory file
```sh
# id_rsa - your private key 
ssh -i ~/.ssh/id_rsa vcherkashyn@bmw000013.adv.org
```

#### automate copying password
```sh
./ssh-copy.expect my_user ubsad00015.vantage.org "my_passw" 
```
```sh
#!/usr/bin/expect -f
set user [lindex $argv 0];
set host [lindex $argv 1];
set password [lindex $argv 2];

spawn ssh-copy-id $user@$host
expect "])?"
send "yes\n"
expect "password: "
send "$password\n"
expect eof
```

sometimes need to add next
```
ssh-agent bash
ssh-add ~/.ssh/id_dsa or id_rsa
```

remove credentials ( undo previous command )
```
ssh-keygen -f "/home/{user}/.ssh/known_hosts" -R "10.140.240.105"
```

copy ssh key to remote machine, but manually:
```
cat .ssh/id_rsa.pub | ssh {username}@{ip}:{port} "cat >> ~/.ssh/authorized_keys"
chmod 700 ~/.ssh ;
chmod 600 ~/.ssh/authorized_keys
```

issue broken pipe ssh
> vim ~/.ssh/config
```
Host *
    ServerAliveInterval 30
    ServerAliveCountMax 5
```

### ssh fingerprint checking
```
ssh -o StrictHostKeyChecking=no user@ubsp00013.vantage.org
sshpass -p my_password ssh -o StrictHostKeyChecking=no my_user@ubsp00013.vantage.org

# check ssh-copy-id, check fingerprint
ssh-keygen -F bmw000013.adv.org
# return 0 ( and info line ), return 1 when not aware about the host
```

### manage multiply keys
```sh
$ ls ~/.ssh
-rw-------  id_rsa
-rw-------  id_rsa_bmw
-rw-r--r--  id_rsa_bmw.pub
-rw-r--r--  id_rsa.pub
```
```sh
$ cat ~/.ssh/config 
IdentityFile ~/.ssh/id_rsa_bmw
IdentityFile ~/.ssh/id_rsa
```

### copy from local machine to remote one, remote copy
```
scp filename.txt cherkavi@129.191.200.15:~/temp/filename-from-local.txt

sshpass -p $CHINA_PASS scp -P $CHINA_JUMP_SERVER_PORT 1.txt $CHINA_USER@$CHINA_JUMP_SERVER:
sshpass -p $CHINA_PASS scp -P $CHINA_JUMP_SERVER_PORT 1.txt $CHINA_USER@$CHINA_JUMP_SERVER:~/
sshpass -p $CHINA_PASS scp -P $CHINA_JUMP_SERVER_PORT 1.txt $CHINA_USER@$CHINA_JUMP_SERVER:~/1.txt
```
### copy from remote machine to local
```
scp -r cherkavi@129.191.200.15:~/temp/filename-from-local.txt filename.txt 
scp -i $EC2_KEY -r ubuntu@35.175.255.10:~/airflow/logs/shopify_product_create/product_create/2021-07-17T02:31:19.880705+00:00/1.log 1.log
```

### copy directory to remote machine, copy folder to remote machine
```
scp -pr /source/directory user@host:the/target/directory
```
the same as local copy folder
```
cp -var /path/to/folder /another/path/to/folder
```

### copy file with saving all attributes, copy attributes, copy file with attributes
```bash
cp -r --preserve=mode,ownership,timestamps /path/to/src /path/to/dest
cp -r --preserve=all /path/to/src /path/to/dest
```

### copy only when changed
```bash
cp --checksum /path/to/src /path/to/dest
```

### change owner
```sh
# change owner recursively for current folder and subfolders
sudo chown -R $USER .
```

### rsync singe file copy from remote
```sh
#!/bin/bash

if [[ $FILE_PATH == "" ]]; then
    echo "file to copy: $FILE_PATH"
else
    if [[ $1 == "" ]]; then
    	echo "provide path to file or env.FILE_PATH"
    else
        FILE_PATH=$1
    fi
fi

USER_CHINA=cherkavi
HOST=10.10.10.1

scp -r $USER_CHINA@$HOST:$FILE_PATH .
# rsync -avz --progress  $USER_CHINA@$HOST:$FILE_PATH $FILE_PATH
```

### sync folders synchronize folders, copy everything between folders, diff folder
!!! rsync has direction from <first folder> to <second folder>
```bash
# print diff 
diff -qr /tmp/first-folder/ /tmp/second-folder

# local sync
rsync -r /tmp/first-folder/ /tmp/second-folder
## Attributes Verbosive Unew-modification-time
rsync -avu /tmp/first-folder/ /tmp/second-folder

# sync remote folder to local ( copy FROM remote )
rsync -avz user@ubspdesp013.vantage.org:~/test-2020-02-28  /home/projects/temp/test-2020-02-28
# sync remote folder to local ( copy FROM remote ) with specific port with compression
rsync -avz -e 'ssh -p 2233' user@ubspdesp013.vantage.org:~/test-2020-02-28  /home/projects/temp/test-2020-02-28

# sync local folder to remote ( copy TO remote )
rsync -avz /home/projects/temp/test-2020-02-28  user@ubspdesp013.vantage.org:~/test-2020-02-28  
# sync local folder to remote ( copy TO remote ) include exclude
rsync -avz --include "*.txt" --exclude "*.bin" /home/projects/temp/test-2020-02-28  user@ubspdesp013.vantage.org:~/test-2020-02-28  

# sync via bastion
rsync -avz -e 'ssh -Ao ProxyCommand="ssh -W %h:%p -p 22 $PROD_BASTION_USER@$PROD_BASTION_HOST" -i '$EC2_KEY $SOURCE_FOLDER/requirements.txt $EC2_LIST_COMPARATOR_USER@$EC2_LIST_COMPARATOR_HOST:~/list-comparator/requirements.txt
```
```bash
function cluster-prod-generation-sync-to(){
  if [[ $1 == "" ]]; then
      return 1
  fi
  rsync -avz . $USER_GT_LOGIN@ubsdpd00013.vantage.org:~/$1
}
```

### create directory on remote machine, create folder remotely, ssh execute command, ssh remote execution
```
ssh user@host "mkdir -p /target/path/"
```

### ssh execute command and detach ssh execute detached ssh command SIGHUP Signal Hang UP
```sh
each_node="bpde00013.ubsbank.org"
REMOTE_SCRIPT="/opt/app/1.sh"
REMOTE_OUTPUT_LOG="/var/log/1.output"

ssh $REMOTE_USER"@"$each_node "nohup $REMOTE_SCRIPT </dev/null > $REMOTE_OUTPUT_LOG 2>&1 &"
```

### ssh xserver, ssh graphical
#### option1
deamon settings
```sh
vim /etc/ssh/sshd_config
sudo systemctl restart sshd
```
```
X11Forwarding yes
```

connect with X11 forwarding
```sh
ssh -X username@server.com
```

#### option 2
```sh
export DISPLAY=:0.0
xterm
```

### here document, sftp batch command with bash
```
sftp -P 2222 my_user@localhost << END_FILE_MARKER
ls
exit
END_FILE_MARKER
```

env variable enviroment variables replace
```sh
echo "${VAR_1}" | envsubst
envsubst < path/to/file/with/variables > path/to/output/file
```

### map folder to another path, mount dir to another location
```
# map local /tmp folder to another path/drive
sudo mount -B /tmp /mapped_drive/path/to/tmp
```

### mount cdrom ( for virtual machine )
```
sudo mount /dev/cdrom /mnt
```

### create ram disc
```
mkdir -p /mnt/my-ram
mount -t tmpfs tmpfs /mnt/my-ram -o size=1024M
```

### repeat command with predefined interval, execute command repeatedly, watch multiple command watch pipe
```
watch -n 60 'ls -la | grep archive'
```

### execute command in case of changes watch file
```
ls *.txt | entr firefox 
```

### repeat last command
```
!!
```
### repeat last command with substring "flow" included into whole command line
```
!?flow
```

### execute in current dir, inline shell execution
```
. goto-command.sh
```

### directories into stack
```
pushd
popd
dirs
```

### to previous folder
```
cd -
```

### sudo reboot
```
shutdown -r now
```

### sort, order
```sh
# simple sort
sort <filename>

#  sort by column ( space delimiter )
sort -k 3 <filename>

# sort by column number, with delimiter, with digital value ( 01, 02....10,11 )
sort -g -k 11 -t "/" session.list

# sort with reverse order
sort -r <filename>
```

### print file with line numbers, output linenumbers
```
cat -n <filename>
```

### split and join big files split and merge, make parts from big file copy parts
```
split --bytes=1M /path/to/image/image.jpg /path/to/image/prefixForNewImagePieces
# --bytes=1G

cat prefixFiles* > newimage.jpg
```

### cut big file, split big file, cat after threshold
```
cat --lines=17000 big_text_file.txt
```

### unique lines (duplications) into file
#### add counter and print result
```
uniq --count
```

#### duplicates
print only duplicates ( distinct )
```
uniq --repeated
```
print all duplications
```
uniq -D
```

#### unique
```
uniq --unique
```

### output to columns format output to column
```sh
ls | column -t -c
```

### print column from file, split string with separator
```
cut --delimiter "," --fields 2,3,4 test1.csv
cut --delimiter "," -f2,3,4 test1.csv
```
substring with fixed number of chars:  from 1.to(15) and 1.to(15) && 20.to(50) 
```
cut -c1-15
cut -c1-15,20-50
```

### output to file without echo on screen, echo without typing on screen
```sh
echo "text file" | grep "" > $random_script_filename
```

### system log information, logging
```sh
# read log
tail -f /var/log/syslog
# write to system log
echo "test" | /usr/bin/logger -t cronjob
# write log message to another system
logger --server 192.168.1.10 --tcp "This is just a simple log line"

/var/log/messages
```

### commands execution logging session logging
```sh
# write output of command to out.txt and execution time to out-timing.txt
script out.txt --timing=out-timing.txt
```

### repository list of all repositories
```
sudo cat /etc/apt/sources.list*
```

### add repository
```
add-apt-repository ppa:inkscape.dev/stable
```
you can find additional file into
```
/etc/apt/sources.list.d
```
or manually add repository
```sh
# https://packages.debian.org/bullseye/amd64/skopeo/download
# The following signatures couldn't be verified because the public key is not available
# deb [trusted=yes] http://ftp.at.debian.org/debian/ bullseye main contrib non-free
```

search after adding
```
apt-cache search inkscape
```
update from one repo, single update
```
sudo apt-get update -o Dir::Etc::sourcelist="sources.list.d/cc-ros-mirror.list" -o Dir::Etc::sourceparts="-" -o APT::Get::List-Cleanup="0" 
```
### remove repository
```
sudo rm /etc/apt/sources.list.d/inkscape.dev*
```

### avoid to put command into history, hide password into history, avoid history
add space before command

### history settings history ignore duplicates history datetime
```sh
HISTTIMEFORMAT="%Y-%m-%d %T "
HISTCONTROL=ignoreboth
history
```

### bash settings, history lookup with arrows, tab autocomplete
~/.inputrc
```
"\e[A": history-search-backward
"\e[B": history-search-forward
set show-all-if-ambiguous on
set completion-ignore-case on
TAB: menu-complete
"\e[Z": menu-complete-backward
set show-all-if-unmodified on
set show-all-if-ambiguous on
```

### script settings
```
# stop execution when non-zero exit
set -e

# stop execution when error happend even inside pipeline 
set -eo pipeline

# stop when access to unknown variable 
set -u

# print each command before execution
set -x

# export source export variables
set -a
source file-with-variables.env
```

### execute command via default editor
```
ctrl+x+e
```

### edit last command via editor
```
fc
```

### folder into bash script
working folder
```sh
pwd
```

### process directory process working dir
```sh
pwdx <process id>
```

### bash reading content of the file to command-line parameter
```
--extra-vars 'rpm_version=$(cat version.txt)'
--extra-vars 'rpm_version=`cat version.txt`'
```
### all command line arguments to another program
```
original.sh $*
```

### ubuntu install python
```
# ubuntu 18 python 3.8
sudo apt install python3.8
sudo rm /usr/bin/python3
sudo ln -s /usr/bin/python3.8 /usr/bin/python3
python3 --version
python3 -m pip install --upgrade pip
```

### auto execute during startup, run during restart, autoexec.bat, startup script run
#### cron startup run
```
@reboot
```
####  rc0...rc1 - runlevels of linux
| ID  | Name                                    | Description                                                                  |
| --- | --------------------------------------- | ---------------------------------------------------------------------------- |
| 0.  | Halt                                    | Shuts down the system.                                                       |
| 1.  | Single-user Mode                        | Mode for administrative tasks.                                               |
| 2.  | Multi-user Mode                         | Does not configure network interfaces and does not export networks services. |
| 3.  | Multi-user Mode with Networking         | Starts the system normally.                                                  |
| 4.  | Not used/User-definable                 | For special purposes.                                                        |
| 5.  | Start the system normally with with GUI | As runlevel 3 + display manager.                                             |
| 6.  | Reboot                                  | Reboots the system.                                                          |

one of folder: /etc/rc1.d ( rc2.d ... )  
contains links to /etc/init.d/S10nameofscript ( for start and K10nameofscript for shutdown ) 
**can** understand next options: start, stop, restart  

/etc/init.d/apple-keyboard
```sh
#!/bin/sh
# Apple keyboard init
#
### BEGIN INIT INFO
# Provides:        cherkashyn
# Required-Start:  $local_fs $remote_fs
# Required-Stop:   $local_fs $remote_fs
# Default-Start:   4 5
# Default-Stop:
# Short-Description: apple keyboard Fn activating
### END INIT INFO

# Carry out specific functions when asked to by the system
case "$1" in
  start)
    echo "Starting script blah "
    ;;
  stop)
    echo "Stopping script blah"
    ;;
  *)
    echo "Usage: /etc/init.d/blah {start|stop}"
    exit 1
    ;;
esac
exit 0
```
```sh
sudo update-rc.d apple-keyboard defaults
# sudo update-rc.d apple-keyboard remove
find /etc/rc?.d/ | grep apple | xargs ls -l
```

#### custom service, service destination
```bash
sudo vim /etc/systemd/system/YOUR_SERVICE_NAME.service
```
```text
Description=GIVE_YOUR_SERVICE_A_DESCRIPTION

Wants=network.target
After=syslog.target network-online.target

[Service]
Type=simple
ExecStart=YOUR_COMMAND_HERE
Restart=on-failure
RestartSec=10
KillMode=process

[Install]
WantedBy=multi-user.target
```

#### ngrok
```
Description=GIVE_YOUR_SERVICE_A_DESCRIPTION

Wants=network.target
After=syslog.target network-online.target

[Service]
User=my_own_user
Group=my_own_user
Type=simple
ExecStart=/snap/ngrok/53/ngrok --authtoken aaabbbcccddd  tcp 22
Restart=on-failure
RestartSec=10
KillMode=process

[Install]
WantedBy=multi-user.target
```

#### service with docker container, service dockerized app
```text
[Unit]
Description=Python app 
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=5
Restart=always
ExecStartPre=-/usr/bin/docker stop app
ExecStartPre=-/usr/bin/docker rm app
ExecStart=/usr/bin/docker run \
    --env-file /home/user/.env.app \
    --name app \
    --publish 5001:5001 \
    appauth
ExecStop=/usr/bin/docker stop app

[Install]
WantedBy=multi-user.target
```

managing services
```sh
# alternative of chkconfig
# alternative of sysv-rc-conf

# list all services
systemctl --all
# in case of any changes in service file 
systemctl enable YOUR_SERVICE_NAME

systemctl start YOUR_SERVICE_NAME
systemctl status YOUR_SERVICE_NAME
systemctl daemon-reload YOUR_SERVICE_NAME
systemctl stop YOUR_SERVICE_NAME
```


reset X-server, re-start xserver, reset linux gui
ubuntu only
```
Ctrl-Alt-F1
```
```
sudo init 3
sudo init 5
```
```
sudo pkill X
```
```
sudo service lightdm stop
sudo service lightdm force-reload
```
start
```
sudo startx
```
```
sudo service lightdm start
```

### xserver automation
[keymap](https://gitlab.com/cunidev/gestures/wikis/xdotool-list-of-key-codes)
```
apt-get install xdotool
xdotool windowactivate $each_window 
xdotool key --window $each_window Return alt+f e Down Down Return
```

### find all symlinks
```
ls -lR . | grep ^l
```

### grep asterix, grep between
```
cat secrets | grep ".*Name.*Avvo.*"
```

### grep exclude grep skip folder grep folder
```
grep -ir --exclude-dir=node_modules "getServerSideProps"
grep -r --files-with-matches --exclude-dir={ad-frontend,data-portal}  "\"index\""
```

### grep multi folders
```sh
grep -ir "getServerSideProps" /home/folder1 /home/folder2
```

### full path to file, file behind symlink, absolute path to file
```
readlink -f {file}
readlink -f `dirname $0`
realpath {file}
```
or
```
python -c 'import os.path; print(os.path.realpath("symlinkName"))'
```

### filename from path
```
basename {file}
```

### folder name from path, folder of file, file directory, file folder, parent folder, parent dir
```
dirname {file}
nautilus "$(dirname -- "$PATH_TO_SVG_CONFLUENCE")"
```

### print full path to files inside folder, check folder for existence path check for existence
```sh
ls -d <path to folder>/*
```
```sh
for each_path in `find /mapr/dp.ch/vantage/data/collected/MDF4/complete -maxdepth 5`; do    
    if [ -d "$each_path" ]; 
    then
        echo "exists: $each_path"
    else
        echo "not a path: $each_path"        
    fi
done
```

### ls directory, ls current folder, ls path, ls by path
```sh
find $FOLDER -maxdepth 4 -mindepth 4 | xargs ls -lad
```

### real path to link
```
readlink 'path to symlink'
```

### where is program placed, location of executable file
```
which "program-name"
```

### permission denied
```sh
# issue with permission ( usually on NFS or cluster )
# find: '/mnt/nfs/ml-training-mongodb-pvc/journal': Permission denied
# 
# solution:
sudo docker run --volume  /mnt/nfs:/nfs -it busybox /bin/sh
chmod -R +r /nfs/ml-training-mongodb-pvc/journal
```

### find file by name find by name
```
locate {file name}
```
exclude DB
```
/etc/updatedb.conf
```
### find files by mask
```
locate -ir "brand-reader*"
locate -b "brand-reader"
```
you need to update filedatabase: /var/lib/mlocate/mlocate.db
```
sudo updatedb
```

### find file, search file, skip permission denied suppress permission denied find by name
```sh
find . -name "prd-ticket-1508.txt"  2>&1 | grep -v "Permission denied"
# suppress permission denied, error pipe with stderror
find /tmp -name 'labeler.jar' |& grep -v "Permission denied"
```

### find multiply patterns
```
find . -name "*.j2" -o -name "*.yaml"
```

### find file by last update time
```
find / -mmin 2
```

### find with exec find md5sum
```sh
find . -exec md5sum {} \;
find . -name "*.json" | while read each_file; do cat "$each_file" > "${each_file}".txt; done
```

### delete files that older than 5 days
```sh
find ./my_dir -mtime +5 -type f -delete
# default variable, env var default
find ${IMAGE_UPLOAD_TEMP_STORAGE:-/tmp/image_upload} -mtime +1 -type f -delete
```

### find files/folders by name and older than 240 min
```
find /tmp -maxdepth 1 -name "native-platform*" -mmin +240 | xargs  --no-run-if-empty -I {} sudo rm -r {} \; >/dev/null 2>&1
```

### find files/folders by regexp and older than 240 min, find depth, find deep
```
find /tmp -maxdepth 1 -mmin +240 -iname "[0-9]*\-[0-9]" | xargs -I {} sudo rm -r {} \; >/dev/null 2>&1
```

### find large files, find big files
```
find . -type f -size +50000k -exec ls -lh {} \;
find . -type f -size +50000k -exec ls -lh {} \; | awk '{ print $9 ": " $5 }'
```

### find files on special level, find on level
```
find . -maxdepth 5 -mindepth 5
```

### find by mask find
```sh
find /mapr/vantage/data/store/processed/*/*/*/*/*/Metadata/file_info.json
```

### find with excluding folders, find exclude
```sh
find . -type d -name "dist" ! -path  "*/node_modules/*"
```

### find function declaration, print function, show function
```
type <function name>
declare -f <function name>
```

### builtin, overwrite command, rewrite command
```
cd()
{
   # builtin going to execute not current, but genuine function
   builtin cd /home/projects
}
```

### folder size, dir size, directory size, size directory, size folder size of folder, size of directory
```
sudo du -shc ./*
sudo du -shc ./* | sort -rh | head -5
```

### free space, space size, dir size, no space left
```sh
df -ha
df -hT /
du -shx /* | sort -h

# size of folder
du -sh /home

# size my sub-folders
du -mh /home

# print first 5 leaders of size-consumers
# slow way: du -a /home | sort -n -r | head -n 5
sudo du -shc ./* | sort -rh | head -5

du -ch /home
# find only files with biggest size ( top 5 )
find -type f -exec du -Sh {} + | sort -rh | head -n 5
```

### yum ( app search )
```
yum list {pattern}
```
( example: yum list python33 )
```
yum install {package name}
yum repolist all
yum info {package name}
yumdb info {package name}
```

### rpm (http://ftp.rpm.org/max-rpm/ch-rpm-query.html)
#### print all packages and sort according last updated on top
```
rpm -qa --last
```

#### information about package ( help page, how to execute ... )
```
rpm -qai
```

#### information about package with configuration
```
rpm -qaic
rpm -qi wd-tomcat8-app-brandserver
```

#### install without sudo rpm without sudo
```sh
rpm -ivh --prefix=$HOME browsh_1.6.4_linux_amd64.rpm
```

### jobs
```
fg, bg, jobs
```

### stop process and start it into background
```
ctrl-Z
bg
```

### stop process and resume it, disconnect from process and leave it running
```
ctrl-Z
fg
```
resume process by number into list 'jobs'
```
fg 2
```

### shell replacing, redirect output to file, fork new process start
```sh
bash
exec > output-file.txt
date
# the same as 'exit'
exec <&-
cat output-file.txt
```

## output to file with variable, output to variable
```sh
gen_version="5.2.1"
$(find /mapr/dp.prod/vantage/data/processed/gen/$gen_version/ -maxdepth 5 -mindepth 5 | awk -F '/' '{print $14}' > gt-$gen_version.list)
```

### execute command and exit
```sh
bash
exec ls -la
```

### execute command from string, execute string, run string
```sh
echo "ls" | xargs -i sh -c "{}"
```

### xargs with multiple arguments 
```sh
find . | xargs -I % sh -c 'md5sum %; ls -la %;'
```

### run nacked terminal without bashrc 
```sh
bash --norc
```
	
### disconnect from terminal and let command be runned
```
ctrl-Z
disown -a && exit
```

### postponed execution, execute command by timer, execute command from now, timer command
> for graphical applications DISPLAY must be specified
* using built-in editor
```sh
at now + 5 minutes
at> DISPLAY=:0 rifle /path/to/image
^D
```
* using inline execution
```sh
echo "DISPLAY=:0 rifle /path/to/image/task.png" | at now + 1 min
echo "DISPLAY=:0 rifle /path/to/image/task.png" | at 11:01
```

### print all files that process is reading
```
strace -e open,access <command to run application>
```

### find process by name
```sh
ps fC firefox
pgrep firefox
```

### pid of process by name
```sh
pidof <app name>
pidof chrome
```

### process by id
```sh
ll /proc/${process_id}
# process command line
cat /proc/${process_id}/cmdline
```

### current process id parent process id
```
echo $$
echo ${PPID}
```

### process list, process tree
```sh
# process list with hierarchy 
ps axjf
ps -ef --forest
ps -fauxw

# process list full command line, ps full cmd
ps -ef ww 

# list of processes by user
ps -ef -u my_special_user
```
### process list without ps
[links to processes](https://www.kernel.org/doc/html/latest/filesystems/proc.html#process-specific-subdirectories)
```sh
ls -l /proc/*/exe
ls -l /proc/*/cwd
cat /proc/*/cmdline
```
### process full command, ps full, ps truncate
```sh
ps -ewwo pid,cmd
```
### threads in process
```sh
ps -eww H -p $PROCESS_ID
```


windows analogue of 'ps aux'
```
wmic path win32_process get Caption, Processid, Commandline
```

### kill -3
```
output to log stop process
```
### remove except one file
```
rm -rf  -- !(exclude-filename.sh)
```

### cron
**You have to escape the % signs with \%**
where is file located
```sh
sudo less /var/spool/cron/crontabs/$USER
```
cron activating
```sh
sudo service cron status
```
all symbols '%' must be converted to '\%'
```sh
# edit file 
# !!! last line should be empty !!!
crontab -e
# list of all jobs
crontab -l
```
adding file with cron job
```sh
echo " * * * * echo `date` >> /out.txt" >> print-date.cron
chmod +x print-date.cron
crontab print-date.cron
```

example of cron job with special parameters
```sh
HOME=/home/ubuntu
0 */6 * * * /home/ubuntu/list-comparator-W3650915.sh >/dev/null 2>&1
9 */6 * * * /home/ubuntu/list-comparator-W3653989.sh >/dev/null 2>&1
# each 6 hours in specific hour
25 1,7,13,19 * * * /home/ubuntu/list-comparator-W3651439.sh >/dev/null 2>&1
```

logs
```
sudo tail -f /var/log/syslog
```
is cron running
```
ps -ef | grep cron | grep -v grep
```
start/stop/restart cron
```
systemctl start cron
systemctl stop cron
systemctl restart cron
```

### skip first line, pipe skip line
```sh
# skip first line in output
docker ps -a | awk '{print $1}' | tail -n +2
```

### error to null
```
./hbase.sh 2>/dev/null
```

### stderr to stdout, error to out
```
sudo python3 echo.py > out.txt 2>&1 &
sudo python3 echo.py &> out.txt &
sudo python3 echo.py > out.txt &

```

### grep with line number
```
grep -nr "text for search" .
```
### grep only in certain folder without recursion, grep current folder, grep in current dir
```sh
# need to set * or mask for files in folder !!!
grep -s "search_string" /path/to/folder/*
sed -n 's/^search_string//p' /path/to/folder/*

# grep in current folder
grep -s "search-string" * .*
```

### grep before
```
grep -B 4
grep --before 4
```

### grep after
```
grep -A 4
grep --after 4
```

### [grep regexp](https://linuxize.com/post/regular-expressions-in-grep/)
```sh
printf "# todo\n## one\n### description for one\n## two\n## three" | grep "[#]\{3\}"
# printf is sensitive to --- strings
### grep boundary between two numbers
printf "# todo\n## one\n### description for one\n## two\n## three" | grep "[#]\{2,3\}"
printf "# todo\n## one\n### description for one\n## two\n## three" | grep --extended-regexp "[#]{3}"
### grep regexp 
## characters
# [[:alnum:]]	All letters and numbers.	"[0-9a-zA-Z]"
# [[:alpha:]]	All letters.	                "[a-zA-Z]"
# [[:blank:]]	Spaces and tabs.         	[CTRL+V<TAB> ]
# [[:digit:]]	Digits 0 to 9.	                [0-9]
# [[:lower:]]	Lowercase letters.	        [a-z]
# [[:punct:]]	Punctuation and other characters.	"[^a-zA-Z0-9]"
# [[:upper:]]	Uppercase letters.	        [A-Z]
# [[:xdigit:]]	Hexadecimal digits.	        "[0-9a-fA-F]"
	
## quantifiers
# *	Zero or more matches.
# ?	Zero or one match.
# +	One or more matches.
# {n}	n matches.
# {n,}	n or more matches.
# {,m}	Up to m matches.
# {n,m}	From n up to m matches.
du -ah .  | sort -r | grep -E "^[0-9]{2,}M"
```

### grep between, print between lines
```
oc describe pod/gateway-486-bawfps | awk '/Environment:/,/Mounts:/'
```

### grep text into files
```
grep -rn '.' -e '@Table'
grep -ilR "@Table" .
```

### grep OR operation
```
cat file.txt | grep -e "occurence1" -e "occurence2"
cat file.txt | grep -e "occurence1\|occurence2"
```

### grep AND operation
```sh
cat file.txt | grep -e "occurence1.*occurence2"
```

### grep not included, grep NOT
```
cat file.txt | grep -v "not-include-string"
cat file.txt | grep -v -e "not-include-string" -e "not-include-another"
```

### grep with file mask
```sh
grep -ir "memory" --include="*.scala"
```

### grep with regexp, grep regexp 
```sh
grep -ir --include=README.md ".*base" 2>/dev/null
```
```sh
echo "BN_FASDLT/1/20200624T083332_20200624T083350_715488_BM60404_BN_FASDLT.MF4" | awk -F "/" '{print $NF}' | grep "[0-9]\{8\}"
```
```sh
echo "185.43.224.157" | grep '^[0-9]\{1,3\}\.'
echo "185.43.224.157" | egrep '^[0-9]{1,3}\.'
echo "185.43.224.157" | egrep '^[0-9][0-9]+[0-9]+\.'
```

### grep with filename
```sh
grep  -rH -A 2 "@angular/core"
```

### grep without permission denied
```
grep -ir --include=README.md "base" 2>/dev/null
```

### grep filename, grep name
```
grep -lir 'password'
```

### inner join for two files, compare string from different files
```
grep -F -x -f path-to-file1 path-to-file2
grep --fixed-strings --line-regexp -f path-to-file1 path-to-file2
```

### difference between two files without spaces
```
diff -w file1.txt file2.txt
```

### show difference in lines with context
```
diff -c file1.txt file2.txt
```

### show equal lines ( reverse diff )
```sh
fgrep -xf W3651292.sh W3659261.sh
```

### show difference between two dates, date difference, time difference
```sh
apt install dateutils
dateutils.ddiff -i '%Y%m%d%H%M%S' -f '%y %m %d %H %M %S' 20160312000101 20170817040001
```

### adjust time, adjust clock, sync clock, computer clock
/etc/systemd/timesyncd.conf.d/90-time-sync.conf
```sh
[Time]
NTP=ntp.ubuntu.com
FallbackNTP=ntp.ubuntu.com
```
restart time sync service
```sh
timedatectl set-ntp true && systemctl restart systemd-timesyncd.service
```

### replace character into string
```
array = echo $result | tr {}, ' ' 
```

### change case of chars ( upper, lower )
```
echo "hello World" | tr '[:lower:]' '[:upper:]
```

### replace all chars
```
echo "hello World 1234 woww" | tr -dc 'a-zA-Z'
```

### replace text in all files of current directory, replace inline, replace inplace, inline replace, sed inplace
```bash
sed --in-place 's/LinkedIn/Yahoo/g' *
# replace tab symbol with comma symbol
sed --in-place 's/\t/,/g' one_file.txt

# in case of error like: couldn't edit ... not a regular file
grep -l -r "LinkedIn" | xargs sed --in-place s/LinkedIn/Yahoo/g

# sed for folder sed directory sed for files
find . -type f -exec sed -i 's/import com.fasterxml.jackson.module.scala.DefaultScalaModule;//p' {} +
```

### no editor replacement, no vi no vim no nano, add line without editor, edit property without editor
```
# going to add new line in property file without editor
sed --in-place 's/\[General\]/\[General\]\nenable_trusted_host_check=0/g' matomo-php.ini
```

### timezone
```sh
timedatectl | grep "Time zone"
cat /etc/timezone
```

### date formatting, datetime formatting, timestamp file, file with timestamp
```sh
# print current date
date +%H:%M:%S:%s
# print date with timestamp
date -d @1552208500 +"%Y%m%dT%H%M%S"
date +%Y-%m-%d-%H:%M:%S:%s
# output file with currenttime file with currenttimestamp
python3 /imap-message-reader.py > message_reader`date +%H:%M:%S`.txt
```

### timestamp to T-date
```sh
function timestamp2date(){
    date -d @$(( $1/1000000000 )) +"%Y%m%dT%H%M%S"
}
timestamp2date 1649162083168929800
```

### generate random string 
```sh
openssl rand -hex 30
# or 
urandom | tr -dc 'a-zA-Z0-9' | fold -w 8 | tr '[:upper:]' '[:lower:]' | head -n 1
```

### find inside zip file(s), grep zip, zip grep
```
zgrep "message_gateway_integration" /var/lib/brand-server/cache/zip/*.zip
```

### grep zip, find inside zip, inside specific file line of text
```
ls -1 *.zip | xargs -I{} unzip -p {} brand.xml  | grep instant-limit | grep "\\."
```
### unzip into specific folder
```
unzip file.zip -d output_folder
```
### unzip without asking for action 
```
unzip -o file.zip -d output_folder
```
### unzip one file
```sh
unzip -l $ARCHIVE_NAME
unzip $ARCHIVE_NAME path/to/file/inside
```

## 7zip
```sh
sudo apt install p7zip-full

7za l archive.7z
7za x archive.7z	
```
	
## tar
### tar archiving tar compression 
```sh
# tar create
tar -cf jdk.tar 8.0.265.j9-adpt
# tar compression
tar -czvf jdk.tar.gz 8.0.265.j9-adpt
```

### untar 
```sh
# tar list of files inside
tar -tf jdk.tar

# tar extract untar
tar -xvf jdk.tar -C /tmp/jdk
# extract into destination with removing first two folders
tar -xvf jdk.tar -C /tmp/jdk --strip-components=2
# extract from URL untar from url
wget -qO- https://nodejs.org/dist/v10.16.3/node-v10.16.3-linux-x64.tar.xz | tar xvz - -C /target/directory
```

### pipeline chain 'to file'
```
echo "hello from someone" | tee --append out.txt
echo "hello from someone" | tee --append out.txt > /dev/null
```

### vi
```
vi wrap( :set wrap, :set nowrap )
```
| shortcut |   description   |
|----------|-----------------|
|     /    |  search forward |
|     ?    | search backward |
|     n    | next occurence  |
|     N    | prev occurence  |

### command prompt change console prompt console invitation
.bashrc of ubuntu
```sh
export PS1="my_host $(date +%d%m_%H%M%S)>"
```
```bash
if [ "$color_prompt" = yes ]; then
#    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@$(date +%d%m_%H%M)\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

else
#    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
    PS1='${debian_chroot:+($debian_chroot)}\u:\$(date +%d.%m_%H:%M)\w\$ '

fi
unset color_prompt force_color_prompt
```
```sh
export PROMPT_COMMAND="echo -n \[\$(date +%H:%M:%S)\]\ "
```

### command line color prompt color console
```sh
# green
export PS1=`printf "\033[31m$ staging \033[39m"`
# red
export PS1=`printf "\033[32m$ staging \033[39m"`
```
```md
Color	Foreground	Background
Black	\033[30m	\033[40m
Red	\033[31m	\033[41m
Green	\033[32m	\033[42m
Orange	\033[33m	\033[43m
Blue	\033[34m	\033[44m
Magenta	\033[35m	\033[45m
Cyan	\033[36m	\033[46m
Light gray	\033[37m	\033[47m
Fallback to distro's default	\033[39m	\033[49m
```

### last executed exit code
```
echo $?
```

### memory dump
```sh
cat /proc/meminfo
```

### memory limit memory usage
```sh
cat /sys/fs/cgroup/memory/memory.limit_in_bytes
cat /sys/fs/cgroup/memory/memory.usage_in_bytes
```

### max open files
```
cat /proc/sys/fs/file-max
```

### open file by type, open image 
```
mimetype -d {filename}
xdg-open {filename}
w3m {filename}
```

### open in browser, open url
```sh
sensible-browser http://localhost:3000/api/status
x-www-browser http://localhost:3000/api/status
# for MacOS
open http://localhost:3000/api/status
```

### wget post
```
wget --method=POST http://{host}:9000/published/resources/10050001.zip
```

### wget to console
```
wget -O- http://{host}:8500/wd-only/getBrandXml.jsp?brand=229099017 > /dev/null  2>&1
```

### wget to console without additional info
```
wget -nv -O- http://{host}:8500/wd-only/getBrandXml.jsp?brand=229099017 2>/dev/null
```

### wget to specific file, download file to specific file
```
wget -O out.zip http://{host}:9000/published/resources/10050001.zip
# in case of complex output path
curl -s http://{host}:9000/published/resources/10050001.zip --create-dirs -o /home/path/to/folder/file.zip
```

### wget to specific folder
```sh
wget http://host:9090/wd-only/1005000.zip --directory-prefix="/home/temp/out"
```

### wget https without checking certificate
```
wget --no-check-certificate https://musan999999.mueq.adas.intel.com:8888/data-api/session/
```

### wget with user wget with credentials
```sh
wget --user $ARTIFACTORY_USER --password $ARTIFACTORY_PASS $ARTIFACTORY_URL
```

### wget with specific timeout
```
wget --tries=1 --timeout=5 --no-check-certificate https://musan999999.mueq.adas.intel.com:8888/data-api/session/
```

### wget proxy, wget via proxy
```
wget -e use_proxy=yes -e http_proxy=127.0.0.1:7777 https://mail.ubsgroup.net/
```
or just with settings file "~/.wgetrc"
```properties
use_proxy = on
http_proxy =  http://username:password@proxy.server.address:port/
https_proxy =  http://username:password@proxy.server.address:port/
ftp_proxy =  http://username:password@proxy.server.address:port/
```

### zip files, zip all files
```sh
zip -r bcm-1003.zip *
```

### zip file with password zip protect with password
```sh
zip --encrypt 1.zip 1.html
```

### zip file without saving path, zip path cleanup
```sh
zip --junk_paths bcm-1003.zip *
```

### encrypt decrypt
```sh
sudo apt install ccrypt
# encrypt file
ccencrypt file1.txt
# ccencrypt file1.txt --key mysecretkey
# print decrypted content
ccat file1.txt.cpt
# decrypt file
ccdecrypt file1.txt.cpt
# ccdecrypt file1.txt.cpt --key mysecretkey
# try to guess password
# ccguess file1.txt.xpt
```

### using parameters for aliases
```sh
alias sublime_editor=/Applications/SublimeEditor/Sublime

subl(){
  sublime_editor "$1" &
}
```

### print alias, print function
```sh
alias sublime_editor
type subl
```

### [sed cheat sheet](https://gist.github.com/ssstonebraker/6140154), sed replace
```sh
replace "name" with "nomen" string
sed 's/name/nomen/g'

# replace only second occurence
# echo "there is a test is not a sentence" | sed 's/is/are/2'
```
example of replacing all occurences in multiply files
```sh
for each_file in `find -iname "*.java"`; do
	sed --in-place 's/vodkafone/cherkavi/g' $each_file
done
```
sed add prefix add suffix replace in line multiple commands
```sh
echo "aaaa find_string bbbb " | sed 's/find_string/replace_to/g' | sed 's/"//g; s/$/\/suffix/; s/^/\/prefix/'
```
### [sed remove sed delete](https://linuxhint.com/sed-command-to-delete-a-line/)
```
# remove line with occurence
sed --in-place '/.*jackson\-annotations/d' $each_file
```

### print line by number from output, line from pipeline, print one line from file 
```
locate -ir "/zip$" | sed -n '2p'
cat out.txt | sed -n '96p'
```

### issue with windows/unix carriage return
```txt
/usr/bin/bash^M: bad interpreter: No such file or directory
```
solution
```
sed -i -e 's/\r$//' Archi-Ubuntu.sh 
```

### calculate amount of strings
```
ps -aux | awk 'BEGIN{a=0}{a=a+1}END{print a}'
```

### boolean value
```
true
false
```

### last changed files, last updated file
```
find -cmin -2
```

### temp file temporary file create temp file
```sh
mktemp
```
	
## cURL command
### [curl without password](https://everything.curl.dev/usingcurl/netrc)
~/.netrc
```sh
machine my-secret-host.com login my-secret-login password my-secret-password
machine my-secret-host2.com login my-secret-login2 password my-secret-password2
```
```sh
curl --netrc --request GET my-secret-host.com/storage/credentials 
```
### return code 0 if 200, curl return code 
```sh
curl --fail --request GET 'https://postman-echo.com/get?foo1=bar1&foo2=bar2'
```
### curl with authentication
```sh
curl --request POST \
--data "client_id=myClient" \
--data "grant_type=client_credentials" \
--data "scope=write" \
--data "response_type=token" \
--cert "myClientCertificate.pem" \
--key "myClientCertificate.key.pem" \
"https://openam.example.com:8443/openam/oauth2/realms/root/access_token"
{
  "access_token": "sbQZ....",
  "scope": "write",
  "token_type": "Bearer",
  "expires_in": 3600
}
	
### echo server mock server 
```sh
curl --location --request GET 'https://postman-echo.com/get?foo1=bar1&foo2=bar2'
```

### curl username, curl with user and password, curl credentials
```sh
curl -u username:password http://example.com
# basic authentication
echo -n "${username}:${password}" | base64
curl -v --insecure -X GET "https://codebeamer.ubsgroup.net:8443/cb/api/v3/wikipages/21313" -H "accept: application/json" -H "Authorization: Basic "`echo -n $TSS_USER:$TSS_PASSWORD | base64`

# bearer authentication
curl --insecure --location --oauth2-bearer $KEYCLOAK_TOKEN "https://portal.apps.devops.vantage.org/session-lister/v1/sessions/cc17d9f8-0f96-43e0-a0dc-xxxxxxx"

# or with certificate 
curl  --cacert /opt/CA.cer --location --oauth2-bearer $KEYCLOAK_TOKEN "https://portal.apps.devops.vantage.org/session-lister/v1/sessions/cc17d9f8-0f96-43e0-a0dc-xxxxxxx"
```

### curl head
```
curl --head http://example.com
```

### curl redirect, redirect curl, curl 302
```
curl -L http://example.com
```

### curl PUT example with file
```
curl -X PUT --header "Content-Type: application/vnd.wirecard.brand.apis-v1+json;charset=ISO-8859-1" -H "x-username: cherkavi" -d@put-request.data http://q-brands-app01.wirecard.sys:9000/draft/brands/229099017/model/country-configurations
```

### curl POST example POST request
```sh
curl -X POST http://localhost:8983/solr/collection1/update?commit=true \
-H "Content-Type: application/json" --data '{"add":"data"}'

curl -X POST http://localhost:8983/solr/collection1/update?commit=true \
-H "Content-Type: application/json" --data-raw '{"add":"data"}'

curl -X POST http://localhost:8983/solr/collection1/update?commit=true \
-H "Content-Type: application/json" --data-binary '{"add":"data"}'

# encoding special symbols curl special symbols
curl -X POST http://localhost:8983/solr/collection1/update?commit=true \
-H "Content-Type: application/json" --data-urlencode '{"add":"Tom&Jerry"}'

# or with bash variable
SOME_DATA="my_personal_value"
curl -X POST http://localhost:8983/solr/collection1/update?commit=true -H "Content-Type: application/json" --data-binary '{"add":"'$SOME_DATA'"}'

# or with data from file
curl -X POST http://localhost:8983/test -H "Content-Type: application/json" --data-binary '@/path/to/file.json'

# or with multipart body
curl -i -X POST -H "Content-Type: multipart/form-data" -F "data=@test.mp3" -F "userid=1234" http://mysuperserver/media/upload/

# multiline body
curl -X 'POST' $SOME_HOST \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "bdcTimestamp": 6797571559111,
  "comment": "Some comment",
  "loggerTimestamp": 1623247031477189001,
  }'

# curl with inline data curl here document curl port document here pipe
json_mappings=`cat some_file.json`
response=`curl -X POST $SOME_HOST -H 'Content-Type: application/json' \
-d @- << EOF
{
	"mappings": $json_mappings,
	"settings" : {
        "index" : {
            "number_of_shards" : 1,
            "number_of_replicas" : 0
        }
    }
}
EOF
`
echo $response

# POST request GET style
curl -X POST "http://localhost:8888/api/v1/notification/subscribe?email=one%40mail.ru&country=2&state=517&city=qWkbs&articles=true&questions=true&listings=true" -H "accept: application/json"
```
### curl escape, curl special symbols
```sh
# https://kb.objectrocket.com/elasticsearch/elasticsearch-cheatsheet-of-the-most-important-curl-requests-252
curl -X GET "https://elasticsearch-label-search-prod.vantage.org/autolabel/_search?size=100&q=moto:*&pretty"
```

### escape single quotas
```
echo "'" 'sentence' "'"
```

### curl without progress, curl silent
* curl -s -X GET http://google.com
* curl --silent -X GET http://google.com
* curl  http://google.com 2>/dev/null

### curl certificate skipping, curl ssl, curl https, curl skip ssl
```
curl --insecure -s -X GET http://google.com
```
### curl with additional output, curl verbosive mode
```sh
curl --verbose --insecure -s -X GET http://google.com
```
### curl cookie, curl header cookie
chrome extension ```cookies.txt```
```sh
# send predefined cookie to url
curl -b path-to-cookie-file.txt -X GET url.com

# send cookie from command line
curl --cookie "first_cookie=123;second_cookie=456;third_cookie=789" -X GET url.com

# send cookie from command line 
curl 'http://localhost:8000/members/json-api/auth/user' -H 'Cookie: PHPSESSID=5c5dddcd96b9f2f41c2d2f87e799feac'

# collect cookie from remote url and save in file
curl -c cookie-from-url-com.txt -X GET url.com
```

### string encoding for http
```sh
sudo apt install gridsite-clients
urlencode "- - -"
```
	
### curl with encoding to another codepage, from win1251 to utf8
```sh
curl "http://some.resource/read_book.php?id=66258&p=1" | iconv --from-code WINDOWS-1251 --to-code UTF-8
```

### [curl with parsing, curl part of the page ](https://github.com/cherkavi/python-utilities/blob/master/html-scraping/lxml/curl-output-html-parser.py#L10)

### curl status code, curl response code, curl duration
```sh
airflow_trigger(){
  SESSION_ID=$1
  ENDPOINT=$2
  BODY='{"conf":{"session_id":"'$SESSION_ID'","branch":"merge_labels"}}'
  curl --silent -w "response-code: %{http_code}\n   time: %{time_starttransfer}" --data-binary $BODY -u $AIRFLOW_USER:$AIRFLOW_PASSWORD -X POST $ENDPOINT
  return $?
}
DAG_NAME='labeling'
airflow_trigger $each_session "https://airflow.vantage.org/api/experimental/dags/$DAG_NAME/dag_runs"
```

```sh
curl --show-error "http://some.resource/read_book.php?id=66258&p=1"
```

### curl execution time
```
curl --max-time 10 -so /dev/null -w '%{time_total}\n' google.com
```

### curl script curl replacement
```sh
curl "https://{foo,bar}.com/file_[1-4].webp" --output "#1_#2.webp"
```

### [json parser output to json pipe json](https://pypi.org/project/jc/)
```sh
# installation 
pip3 install jc
apt-get install jc

# parse general output to json
jc --pretty ls -la
# using predefined parser
dig www.google.com | jc --dig --pretty
```

### [json xml sql yaml converter](https://github.com/dcmoura/spyql)
* [spyql doc](https://spyql.readthedocs.io/en/latest/recipes.html)
* https://danielcmoura.com/blog/2022/spyql-cell-towers/
```sh
echo '{"a": 10, "b": "kitchen"}' | spyql -Otable=my_table "SELECT  json.a as indicator_value, json.b as place FROM json TO sql" 
```

### [jq json navigator json parser json parsing parse json parsing json json processing json query](https://stedolan.github.io/jq/manual/)
* [json query doc](https://stedolan.github.io/jq/)  
* [json tool json walk json analyzer](https://github.com/antonmedv/fx)
  > `snap install fx`
* [jq playground](https://jqplay.org/jq?q=.[%22foo%22]&j={%22foo%22%3A%2042})  
WARNING: jq round up big numbers:
```sh
echo '{"loggerTimestamp": 1657094097468421888}' | jq .
# {
#  "loggerTimestamp": 1657094097468422000
# }
```

> jq is not working properly with "-" character in property name !!!  
jq is not working sometimes with "jq any properties", need to split them to two commands
```sh
docker network inspect mysql_web_default | jq '.[0].Containers' | jq .[].Name
```
```bash
echo '[{"id": 1, "name": "Arthur", "age": "21"},{"id": 2, "name": "Richard", "age": "32"}]' | \
jq ".[] | .name"

# json output pretty print, json pretty print, json sort
echo output.json | jq .
# sort by keys
echo output.json | jq -S .

# jq select with condition
jq -e 'select(.[].name == "CP_END")' $SESSION_METADATA_FOLDER/$SESSION_ID
echo $? # return 0 only when met the condition, otherwise - 1

# .repositories[].repositoryName
aws ecr describe-repositories | jq '.repositories[] | select(.repositoryName == "cherkavi-udacity-github-action-fe")'

# jq filter by condition
docker network inspect mysql_web_default | jq '.[0].Containers' | jq '.[] | select(.Name=="web_mysql_ui")' | jq .IPv4Address

# jq create another document filter json transform json
echo '[{"id": 1, "name": "Arthur", "age": "21"},{"id": 2, "name": "Richard", "age": "32"}]' | jq '[.[] | {number:.id, warrior:.name} ]'
	
# jq convert to csv
echo '[{"id": 1, "name": "Arthur", "age": "21"},{"id": 2, "name": "Richard", "age": "32"}]' | \
jq '.[] | if .name == "Richard" then . else empty end | [.id, .name] | @csv'

# jq as a table
echo '[{"id": 1, "name": "Arthur", "age": "21"},{"id": 2, "name": "Richard", "age": "32"}]' | jq -r '["ID","NAME"], ["--","------"], (.[] | [.id,.name]) | @tsv' 
	
# jq get first element
echo '[{"id": 1, "name": "Arthur", "age": "21"},{"id": 2, "name": "Richard", "age": "32"}]' | jq '.[0] | [.name, .age] | @csv'

# convert from yaml to json, retrieve values from json, convert to csv
cat temp-pod.yaml | jq -r -j --prettyPrint | jq '[.metadata.namespace, .metadata.name, .spec.template.spec.nodeSelector."kubernetes.io/hostname"] | @csv'

# multiply properties from sub-element
aws s3api list-object-versions --bucket $AWS_S3_BUCKET_NAME --prefix $AWS_FILE_KEY | jq '.Versions[] | [.Key,.VersionId]'

echo '{"smart_collections":[{"id":270378401973},{"id":270378369205}]}' | jq '. "smart_collections" | .[] | .id'

jq 'if .attributes[].attribute == "category" and (.attributes[].normalizedValues != null) and (.attributes[].normalizedValues | length )>1 then . else empty end'
	
# jq remove quotas raw text
jq -r ".DistributionList.Items[].Id"

# jq escape symbols
kubectl get nodes -o json | jq -r '.items[].metadata.annotations."alpha.kubernetes.io/provided-node-ip"'

# edit variables inside JSON file
ENV_DATA=abcde
jq --arg var_a "$ENV_DATA" '.ETag = $var_a' cloud_front.json
jq '.Distribution.DistributionConfig.Enabled = false' cloud_front.json
```

### json compare json diff
```sh
cmp <(jq -cS . A.json) <(jq -cS . B.json)
diff <(jq --sort-keys . A.json) <(jq --sort-keys . B.json)
```
### [json deep compare](https://github.com/cherkavi/python-utilities/blob/master/json/compare-json.py)
```sh
# export JSON_COMPARE_SUPPRESS_OUTPUT=""
export JSON_COMPARE_SUPPRESS_OUTPUT="true"
python3 jsoncompare.py  $FOLDER_CSV/$SESSION_ID $FOLDER/$SESSION_ID
```

### [parsing yaml, yaml processing yaml query](https://mikefarah.gitbook.io/yq/)
### [yaml tool edit yaml xpath](https://pypi.org/project/yamlpath/)
```sh
pip install yamlpath
```

#### yaml get value by xpath 
```sh
echo '
first: 
  f_second: one_one
second: 2
' | yaml-get -p first.f_second
# one_one
```

#### yaml search for xpath, print files with values in xpath
```sh
echo '
first: 
  f_second: one_one
second: 2
' | yaml-paths --search=%one
```

#### yaml edit by xpath ( scalars only )
```sh
echo '
first: one
second: 2 
' | yaml-set -g first --value=1
# ---
# first: 1
# second: 2
```

#### yaml difference between two yaml files 
```sh
echo '
second: 2
first: 
  f_second: one_one
' > temp_1.yaml
echo '
first: 
  f_second: one_two
second: 2
' > temp_2.yaml
yaml-diff temp_1.yaml temp_2.yaml
# < "one_one"
# ---
# > "one_two"
```

### [yq doc](https://mikefarah.gitbook.io/yq/operators)
#### [yq examples](https://metacpan.org/pod/distribution/ETL-Yertl/bin/yq)
```sh
# read value
cat k8s-pod.yaml | yq r - --printMode pv  "metadata.name"

# convert to JSON
cat k8s-pod.yaml | yq - r -j --prettyPrint
# convert yaml to json|props|xml|tsv|csv
cat k8s-pod.yaml | yq --output-format json

# yaml remove elements clear ocp fields
yq 'del(.metadata.managedFields,.status,.metadata.uid,.metadata.resourceVersion,.metadata.creationTimestamp,.spec.clusterIP,.spec.clusterIP)' service-data-api-mdf4download-service.yaml

# yaml editor 
yq 'del(.metadata.managedFields,.status,.metadata.uid,.metadata.resourceVersion,.metadata.creationTimestamp,.spec.clusterIP,.spec.clusterIP),(.metadata.namespace="ttt")' service-data-api-mdf4download-service.yaml
```
#### yq converter
```sh
# convert yaml to json|props|xml|tsv|csv
cat file.yaml | yq --output-format json
```

### parsing xml parsing xml processing
### xq 
```sh
# installation
pip3 install xq
# xq usage ??? is it not working as expected ????
```
#### parse xml with xpath 
```sh
# installation
sudo apt install libxml-xpath-perl
# parse xml from stdin
curl -s https://www.w3schools.com/xml/note.xml | xpath -e '/note/to | /note/from'
curl -s https://www.w3schools.com/xml/note.xml | xpath -e '/note/to/text()'
```

#### parse xml with xmllint
```sh
# installation
sudo apt  install libxml2-utils

## usage
TEMP_FILE=$(mktemp)
curl -s https://www.w3schools.com/xml/note.xml > $TEMP_FILE
xmllint --xpath '//note/from' $TEMP_FILE
xmllint --xpath 'string(//note/to)' $TEMP_FILE
xmllint --xpath '//note/to/text()' $TEMP_FILE

# avoid xmllint namespace check
xmllint --xpath "//*[local-name()='project']/*[local-name()='modules']/*[local-name()='module']/text()" pom.xml
# avoid issue with xmllint namespace
cat pom.xml | sed '2 s/xmlns=".*"//g' | xmllint --xpath "/project/modules/module/text()" -

# debug xml xpath debug 
xmllint --shell  $TEMP_FILE
rm $TEMP_FILE
```
	
### xml pretty print, xml format
```
xmllint --format /path/to/file.xml > /path/to/file-formatted.xml
```

### xml validation
```sh
xmllint --noout file.xml; echo $?
```

### [html parsing html processing html query](https://github.com/rbwinslow/hq/wiki/Language-Reference)
```sh
pip install hq
curl https://www.w3schools.com/xml | hq '`title: ${/head/title}`'
cat index.html | hq '/html/body/table/tr/td[2]/a/text()'
# retrieve all classes
cat p1-utf8.txt | hq '/html/body/table//p/@class'
# retrieve all texts from 'p'
cat p1-utf8.txt | hq '/html/body/table//p/text()'
# retrieve all texts from 'p' with condition
cat p1-utf8.txt | hq '/html/body/table//p[@class="MsoNormal"]/text()'
# retrieve html tag table
cat p1-utf8.txt | hq '//table'
```

### chmod recursively
```
chmod -R +x <folder name>

# remove world access 
chmod -R o-rwx /opt/sm-metrics/grafana-db/data
# remove group access
chmod -R g-rwx /opt/sm-metrics/grafana-db/data
# add rw access for current user
chmod u+rw screenshot_overlayed.png
```
```
find . -iname "*.sql" -print0 | xargs -0 chmod 666
```

### create dozen of folders using one-line command
```
mkdir -p some-folder/{1..10}/{one,two,three}
```

### execute command with environment variable, new environment variable for command
```
ONE="this is a test"; echo $ONE
```

# activate environment variables from file, env file, export env, export all env, all variable from file, all var export, env var file
```bash
FILE_WITH_VAR=.env.local
source $FILE_WITH_VAR
export $(cut -d= -f1 $FILE_WITH_VAR)

# if you have comments in file
source $FILE_WITH_VAR
export `cat $FILE_WITH_VAR | awk -F= '{if($1 !~ "#"){print $1}}'`
```

### system log file
```
/var/log/syslog
```
### Debian install package via proxy, apt install proxy, apt proxy, apt update proxy
```
sudo http_proxy='http://user:@proxy.muc:8080' apt install meld
```
#### proxy places, change proxy, update proxy, system proxy
> remember about escaping bash spec chars ( $,.@.... )
* .bashrc
* /etc/environment
* /etc/systemd/system/docker.service.d/http-proxy.conf
* /etc/apt/auth.conf
```sh
Acquire::http::Proxy "http://username:password@proxyhost:port";
Acquire::https::Proxy "http://username:password@proxyhost:port";
```
* snap 
```sh
sudo snap set system proxy.http="http://user:password@proxy.zur:8080"
sudo snap set system proxy.https="http://user:password@proxy.zur:8080"
```

### snap installation issue
```sh
# leads to error: cannot connect to the server
snap install <app>

# Unmask the snapd.service:
sudo systemctl unmask snapd.service

# Enable it:
systemctl enable snapd.service

# Start it:
systemctl start snapd.service
```

### install version of app, install specific version, accessible application version
```sh
sudo apt list -a [name of the package]
sudo apt list -a kubeadm
```

### install package for another architecture, install x86 on x64
```
dpkg --add-architecture i386
dpkg --print-architecture
dpkg --print-foreign-architectures
sudo apt-get install libglib2.0-0:i386 libgtk2.0-0:i386
```
### installed package check package information 
```
apt list <name of package>
apt show <name of package>
```

### package update package mark 
```sh
apt mark hold kubeadm
# install: this package is marked for installation.
# deinstall (remove): this package is marked for removal.
# purge: this package, and all its configuration files, are marked for removal.
# hold: this package cannot be installed, upgraded, removed, or purged.
# unhold: 
# auto: auto installed
# manual: manually installed
```

### Debian update package
```
sudo apt-get install --only-upgrade {packagename}
```

### Debian list of packages
```
sudo apt list
sudo dpkg -l
```

| First letter | desired package state ("selection state")|
----|------------------------
| u | unknown |
| i | install |
| r | remove/deinstall |
| p |  purge (remove including config files) |
| h |  hold |

| Second letter | current package state |
---|---------
| n | not-installed |
| i | installed |
| c | config-files (only the config files are installed) |
| U | unpacked |
| F | half-configured (configuration failed for some reason) |
| h | half-installed (installation failed for some reason) |
| W | triggers-awaited (package is waiting for a trigger from another package) |
| t | triggers-pending (package has been triggered) |

|  Third letter | error state (you normally shouldn't see a third letter, but a space, instead)|
|---|---------
|  R |  reinst-required (package broken, reinstallation required)|

### Debian list the versions available in your repo
```
sudo apt-cache madison {package name}
```

### Debian install new version of package with specific version
```
sudo apt-get install {package name}={version}
```

### Debian system cleanup
```
sudo apt-get clean
sudo apt-get autoremove --purge
```

### uninstall specific app
```
sudo apt-get --purge remote {app name}
```

### remove service ( kubernetes )
* sudo invoke-rc.d localkube stop
* sudo invoke-rc.d localkube status
( sudo service localkube status )
* sudo update-rc.d -f localkube remove
* sudo grep -ir /etc -e "kube"
* rm -rf /etc/kubernetes
* rm -rf /etc/systemd/system/localkube.service
* vi /var/log/syslog

### last executed code, last script return value
```
echo $?
```

### remove VMWare player
```
sudo vmware-installer -u vmware-player
```

### pdf watermark, merge pdf files into one
```
pdftk original.pdf stamp watermark.pdf output output.pdf
```

### version of OS, linux version os information 
* lsb_release -a
* cat /etc/system-release
* uname -a
* `. /etc/os-release` 

### [cidr calculator](https://cidr.xyz/)

### ip address of the site show ip address of remote host ip address
```sh
host google.com
dig mail.ru
```
	
### print all networks
```
ip -4 a
ip -6 a
```

### print all network interfaces all wifi devices
```sh
interfaces
ifconfig
nmcli d
```

### switch on and off network interface
```
sudo ifdown lo && sudo ifup lo
```

### restart network, switch off all interfaces
```
sudo service network-manager restart
```
### vpn connection, connect to network
```sh
# status of all connections
nmcli d
nmcli connection
nmcli connection up id {name from previous command}
nmcli connection down id {name of connection}
```

### connect to wifi
```sh
wifi_code='188790542'
point="FRITZ!Box 7400 YO"
nmcli device wifi connect  "$point" password $wifi_code
# sudo cat /etc/NetworkManager/system-connections/*
```

### raw vpn connection
```sh
sudo openconnect --no-proxy {ip-address} --user=$VPN_USER $URL_VPN
sudo openconnect --no-cert-check --no-proxy --user=$VPN_USER ---servercert $URL_VPN
openconnect $URL_VPN --interface=vpn0 --user=$(id -un) --authgroup=YubiKey+PIN -vv --no-proxy --no-dtls

FILE_CERT_CA=WLAN_CA.crt
FILE_USER_KEY=xxx.ubs.corp.key
FILE_USER_CERT=xxx.ubs.corp.crt
URL_VPN=https://vpn.ubs.com
USER_VPN=xxxyyyzzz
sudo openconnect --no-proxy --user=$USER_VPN --authgroup='YubiKey+PIN' --cafile=$FILE_CERT_CA --sslkey=$FILE_USER_KEY --certificate=$FILE_USER_CERT $URL_VPN	
```
### openvpn vpn connection
```sh
# apt install network-manager-openvpn
sudo openvpn file_config.ovpn
```

### debug network collaboration, ip packages
example with reading redis collaboration ( package sniffer )
```sh
sudo ngrep -W byline -d docker0 -t '' 'port 6379'
```
### debug connection, print collaboration with remote service, sniffer
```sh
#                    1------------     2--------------------     3--------------
sudo tcpdump -nvX -v src port 6443 and src host 10.140.26.10 and dst port not 22
# and, or, not
```

### keystore TrustStore 
> TrustStore holds the certificates of external systems that you trust.  
> So a TrustStore is a KeyStore file, that contains the public keys/certificate of external hosts that you trust.
```sh
## list of certificates inside truststore 
keytool -list -v -keystore ./src/main/resources/com/ubs/crm/data/api/rest/server/keystore_server
# maybe will ask for a password

## generating ssl key stores
keytool -genkeypair -keystore -keystore ./src/main/resources/com/ubs/crm/data/api/rest/server/keystore_server -alias serverKey -dname "CN=localhost, OU=AD, O=UBS AG, L=Zurich, ST=Bavaria, C=DE" -keyalg RSA
# enter password...

## Importing ( updating, adding ) trusted SSL certificates
keytool -import -file ~/Downloads/certificate.crt -keystore ./src/main/resources/com/ubs/crm/data/api/rest/server/keystore_server -alias my-magic-number
```
#### in other words, rsa certificate rsa from url x509 url: 
1. Download the certificate by opening the url in the browser and downloading it there manually.  
2. Run the following command: ```keytool -import -file <name-of-downloaded-certificate>.crt -alias <alias for exported file> -keystore myTrustStore```  

### DNS
```sh
# check 
sudo resolvectl status | grep "DNS Servers"
systemd-resolve --status
systemctl status systemd-resolved

# restart
sudo systemctl restart systemd-resolved

# current dns
sudo cat /etc/resolv.conf
# resolving hostname
dig google.com
```
aws example, where 10.0.0.2 AWS DNS internal server
```sudo vim /etc/resolv.conf```
```
# nameserver 127.0.0.53
nameserver 10.0.0.2
options edns0 trust-ad
search ec2.internal
```


### encrypt file, decrypt file, encode/decode
```
gpg --symmetric {filename}
gpg --decrypt {filename}
```
```bash
# encrypt
# openssl [encryption type] -in [original] -out [output file]
openssl des3 -in original.txt -out original.txt.encrypted
# decrypt
# openssl [encryption type] -d -in [encrypted file] -out [original file]
openssl des3 -d -in original.txt.encrypted -out original.txt

# list of encryptors (des3):
openssl enc -list
```

### add user into special group, add user to group
* adduser {username} {destination group name}
* edit file /etc/group
```
add :{username} to the end of line with {groupname}:x:999
```
### create/add user, create user with admin rights
```
sudo useradd test

sudo useradd --create-home test --groups sudo 
# set password for new user
sudo passwd test
# set default bash shell 
chsh --shell /bin/bash tecmint
```

### sudo for user, user sudo, temporary provide sudo
```
sudo adduser vitalii sudo
# close all opened sessions
# after your work done
sudo deluser vitalii sudo
```

### admin rights for script, sudo rights for script, execute as root
```
sudo -E bash -c 'python3'
```

### remove user
```
sudo userdel -r test
```

### create group, assign user to group, user check group, user group user roles hadoop
```
sudo groupadd new_group
usermod --append --groups new_group my_user
id my_user
```

### create folder for group, assign group to folder
```
chgrp new_group /path/to/folder
```

### execute sudo with current env variables, sudo env var, sudo with proxy
```
sudo -E <command>
```

### execute script with current env variables send to script
```
. ./airflow-get-log.sh
source ./airflow-get-log.sh
cat dag-runs-failed.id | . ./airflow-get-log.sh
```

### print all logged in users, active users, connected users
```bash
users
```
```bash
w
```
```bash
who --all
```

### send message to user
```sh
write <username> <message>
```

### print all users registered into system
```
cat /etc/passwd | cut --delimiter=: --fields=1
```

### issue with ssh, ssh connection issue
when you see message:
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
```
use this:
```
ssh-keygen -R <host>
```
or
```
rm ~/.ssh/known_hosts
```

### proxy
* proxy local, proxy for user /etc/profile.d/proxy.sh
```
export HTTP_PROXY=http://webproxy.host:3128
export http_proxy=http://webproxy.host:3128
export HTTPS_PROXY=http://webproxy.host:3128
export https_proxy=http://webproxy.host:3128
export NO_PROXY="localhost,127.0.0.1,.host,.viola.local"
export no_proxy="localhost,127.0.0.1,.host,.viola.local"
```

* #### global proxy, proxy global, system proxy, proxy system /etc/apt/apt.conf 
```
Acquire::http::proxy "http://proxy.company.com:80/";
Acquire::https::proxy "https://proxy.company.com:80/";
Acquire::ftp::proxy "ftp://proxy.company.com:80/";
Acquire::socks5::proxy "socks://127.0.0.1:1080/";
```

* #### global proxy, proxy global, system proxy, proxy system /etc/environment
```
http_proxy=http://webproxy.host:3128
no_proxy="localhost,127.0.0.1,.host.de,.viola.local"
```

* #### for application
create environment for http
```
sudo gedit /etc/systemd/system/{service name}.service.d/http-proxy.conf

[Service]
Environment="http_proxy=http://user:passw@webproxy.host:8080"
```

create environment for https
```
sudo gedit /etc/systemd/system/{service name}.service.d/https-proxy.conf
[Service]
Environment="https_proxy=http://user:passw@webproxy.host:8080"
```

service list of services
```sh
systemctl list-unit-files --type=service
```

restart service restart service stop service start
```
$ sudo systemctl daemon-reload
$ sudo systemctl restart {service name}
# or
sudo service {service name} stop
sudo service {service name} start
```

check service status
```sh
sudo systemctl is-active {service name}
```

enable automatic start disable autostart disable service
```
sudo systemctl enable {service name}
sudo systemctl disable {service name}
```

service check logs
```
systemctl status {service name}
journalctl -u {service name} -e
# print all units
journalctl -F _SYSTEMD_UNIT

# system log
journalctl -f -l 
# system log for app log
$ journalctl -f -l -u python -u mariadb
# system log since 300 second
$ journalctl -f -l -u httpd -u mariadb --since -300
```

check settings
```
systemctl show {service name} | grep proxy
```

* #### for snapd 
```
# export SYSTEM_EDITOR="vim"
# export SYSTEMD_EDITOR="vim"
sudo systemctl edit snapd.service
# will edit: /etc/systemd/system/snapd.service.d/override.conf
```
add next lines
```
[Service]
Environment=http_proxy=http://proxy:port
Environment=https_proxy=http://proxy:port
```
restart service
```
sudo systemctl daemon-reload
sudo systemctl restart snapd.service
```

### snap proxy settings
```
sudo snap set system proxy.http="http://user:password@proxy.muc:8080"
sudo snap set system proxy.https="http://user:password@proxy.muc:8080"
export proxy_http="http://user:password@proxy.muc:8080"
export proxy_https="http://user:password@proxy.muc:8080"
sudo snap search visual 
```

### tools:
- [ETL](www.talend.com)
- [ETL](https://hekad.readthedocs.io)
- web management - atomicproject.io, cockpit
  * [install](https://cockpit-project.org/running#ubuntu)
  * [guide](https://cockpit-project.org/guide/latest/)
  * [after installation](https://127.0.0.1:9090)
  * use your own user/password

### virtual machines
* [images](http://osboxes.org)
* [app with infrastructure](https://bitnami.com/stacks)

## mapping keys, keymap, assign actions to key
### show key codes
```
xmodmap -pke
# or take a look into "keycode ... " 
xev 
```

### remap key 'Druck' to 'Win'
```
xmodmap -e "keycode 107 = Super_L"
```
to reset
```
setxkbmap
```

### save to use during each reboot
```
create file '~/.Xmodmap'
```

### find key code
```
xev | grep keysym
```

### key code, scan code, keyboard code
```
sudo evtest 
sudo evtest /dev/input/event21
```

### remap [hjkl] to [Left, Down, Up, Right], cursor hjkl
[mapping list](https://wiki.linuxquestions.org/wiki/List_of_Keysyms_Recognised_by_Xmodmap)  
content of $HOME/.config/xmodmap-hjkl
```
keycode 66 = Mode_switch
keysym h = h H Left 
keysym l = l L Right
keysym k = k K Up
keysym j = j J Down
```
execute re-mapping, permanent solution
```sh
# vim /etc/profile
xmodmap $HOME/.config/xmodmap-hjkl
```

## remap reset, reset xmodmap
```sh
setxkbmap -option
```

## terminal title
```
set-title(){
  ORIG=$PS1
  TITLE="\e]2;$@\a"
  PS1=${ORIG}${TITLE}
}

set-title "my title for terminal"
```

## code/decode
### base64
```sh
# !!! important !!! will produce line with suffix "\n" 
base64 cAdvisor-start.sh | base64 --decode
echo "just a text string" | base64 | base64 --decode

# !!! important !!! will produce line WITHOUT suffix "\n" 
echo -n "just a text string " | base64 
printf "just a text string " | base64 
```

### md5 digest
```
echo -n foobar | sha256sum

md5sum filename
sha224sum filename
sha384sum filename
sha512sum filename
```

## driver install hardware
```sh
sudo ubuntu-drivers autoinstall
reboot
```

## hardware serial numbers, hardware id, hardware version, system info
```sh
sudo dmidecode --string system-serial-number
sudo dmidecode --string processor-family
sudo dmidecode --string system-manufacturer
# disk serial number
sudo lshw -class disk
```

## equipment system devices
```sh
inxi -C
inxi --memory
inxi -CfxCa
```

## pdf
### convert pdf to image
```sh
FILE_SOURCE="certificate_Vitalii.pdf"
pdftoppm -png $FILE_SOURCE  $FILE_SOURCE
# pdftoppm -mono -jpeg $FILE_SOURCE $FILE_SOURCE

# tesseract $FILE_SOURCE-1.png - -l eng
```
```sh
convert -geometry 400x600 -density 100x100 -quality 100 test-pdf.pdf test-pdf.jpg
```

### bar code create into pdf
```
barcode -o 1112.pdf -e "code39" -b "1112" -u "mm" -g 50x50
```

### qr code online generator
```
http://goqr.me/api/doc/create-qr-code/
http://api.qrserver.com/v1/create-qr-code/?data=HelloWorld!&size=100x100
```

### bar code finder
```sh
apt install zbar-tool
zbarimg <file>
```

### pdf file merge, pdf join
```sh
gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -sOutputFile=finished.pdf test-pdf2.pdf test-pdf3.pdf test-pdf4.pdf
```

### pdf file decrease size pdf compression
```sh
# -dPDFSETTINGS=/screen — Low quality and small size at 72dpi.
# -dPDFSETTINGS=/ebook — Slightly better quality but also a larger file size at 150dpi.
# -dPDFSETTINGS=/prepress — High quality and large size at 300 dpi.
# -dPDFSETTINGS=/default — System chooses the best output, which can create larger PDF files.

gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
```

### doc to pdf, convert to pdf
```sh
libreoffice --headless --convert-to pdf "/home/path/Dativ.doc" --outdir /tmp/output
```

## zip
### unzip bz2
```sh
bzip2 -dc ricochet-1.1.4-src.tar.bz2 | tar xvf -
```
### gzip unzip gzip decompress
```sh
gzip -d out.gz
# unknown suffix -- ignored
# add "gz" suffix to file
```

## console and clipboard
```sh
alias clipboard="xclip -selection clipboard" 
alias clipboard-ingest="xclip -selection clipboard"
function clipboard-copy-file(){
    xclip -in -selection c $1
}
alias clipboard-print="xclip -out -selection clipboard"
```

## screenshot, copy screen
```
screenshot(){
	file_name="/home/user/Pictures/screenshots/screenshot_"`date +%Y%m%d_%H%M%S`".png"
	scrot $file_name -s -e "xdg-open $file_name"
}
```

## printer managing ( add/remote/edit )
* [printer installation](http://cups.org)
* [admin page](http://localhost:631/admin)
* [help page](http://localhost:631/help/options.html)

### in case of authorization issue:
/etc/cups/cupsd.conf and changed the AuthType to None and commented the Require user @SYSTEM:
```
<Limit CUPS-Add-Modify-Printer CUPS-Delete-Printer CUPS-Add-Modify-Class CUPS-Delete-Class CUPS-Set-Default CUPS-Get-Devices>
AuthType None
# AuthType Default
# Require user @SYSTEM
Order deny,allow
</Limit>
```
and restart the service
```sh
sudo service cups restart
```

### default printer
```sh
# show all printer drivers in system
lpinfo -m

# print all printer names
lpstat -l -v
# device for Brother_HL_L8260CDW_series: implicitclass://Brother_HL_L8260CDW_series/

# set default printer
PRINTER_NAME=Brother_HL_L8260CDW_series
sudo lpadmin -d $PRINTER_NAME
```

### printer queue
```sh
lpq -P 
```

### print to printer
```sh
lpr -P $PRINTER_NAME myfile.txt
lpr -P $PRINTER_NAME -o fit-to-page=false -o position=top $out_file
```

## kernel related messages
```sh
dmesg --level=err,warn
dmesg --follow
# save all messages /var/log/dmesg
dmesg -S
```

## disk usage
```sh
df -ha
# with visualization
ncdu
```

## create startup disk, write iso image, usb stick, bootable drive
```bash
# list of all hard drives, disk list
sudo lshw -class disk -short
# write image
sudo dd bs=4M if=/home/my-user/Downloads/archlinux-2019.07.01-x86_64.iso of=/dev/sdb status=progress && sync
```

## create usb live with persistence, usb persistence, stick persistence
```sh
sudo add-apt-repository universe
sudo add-apt-repository ppa:mkusb/ppa
sudo apt-get update
sudo apt install --install-recommends mkusb mkusb-nox usb-pack-efi
mkusb
# Install, persistent live, upefi
```

## split usb drive, split disk
```bash
# detect disks
sudo lshw -class disk -short
sudo fdisk -l

# format drive
DEST_DRIVE=/dev/sdb
sudo dd if=/dev/zero of=$DEST_DRIVE  bs=512  count=1
# sudo mke2fs -t xfs $DEST_DRIVE

# split drive, split disk, split usb
sudo parted $DEST_DRIVE
```

```
print
rm 1
rm 2

mklabel kali
msdos

mkpart primary ext4 0.0 5GB
I

mkpart extended ntfs 5GB -1s

print 
set 1 boot on
set 2 lba on
quit
```
```sh
sudo fdisk -l
```

## time command resource consumption command exec information
```sh
\time -v date
```
### command time consumption
```sh
time curl google.com
```
	
### elapsed time between two commands
```sh
STARTTIME=$SECONDS
sleep 2
echo $SECONDS-$STARTTIME
```

```sh
STARTTIME=`date +%s.%N`
sleep 2.5
ENDTIME=`date +%s.%N`
TIMEDIFF=`echo "$ENDTIME - $STARTTIME" | bc | awk -F"." '{print $1"."substr($2,1,3)}'`
```

## language translator 
```sh
sudo apt-get install translate-shell
trans -source de -target ru -brief "german sentance"
```

## gnome
### remove emoji
```sh
ibus-setup
# go to emojii and remove shortcuts
```

## video
### join mp4 fusion mp4
```sh
ffmpeg -i video.mp4 -i audio.mp4 output.mp4
```
### convert webm video to mp3  
```sh
FILE_INPUT=video.webm
FILE_OUTPUT=audio.mp3
ffmpeg -i $FILE_INPUT -vn -ab 64k -ar 44100 -y $FILE_OUTPUT
```
### convert mp4 to mp3 with slow playing
```sh
file_input="Cybercity.mp4"
file_output="Cybercity.mp3"
ffmpeg -i $file_input -vn -acodec libmp3lame -q:a 4 -filter:a "atempo=0.75 "$file_output
```

## sound
### join files
```
sox 1.wav 2.wav 3.wav 4.wav output.wav
ffmpeg -i 1.wav -i 2.wav -i 3.wav output.wav
```

## copy users, import/export users
```
sudo awk -F: '($3>=LIMIT) && ($3!=65534)' /etc/passwd > passwd-export
sudo awk -F: '($3>=LIMIT) && ($3!=65534)' /etc/group > /opt/group-export
sudo awk -F: '($3>=LIMIT) && ($3!=65534) {print $1}' /etc/passwd | tee - | egrep -f - /etc/shadow > /opt/shadow-export
sudo cp /etc/gshadow /opt/gshadow-export
```

### calculcate size of files by type
```
find . -name "*.java" -ls | awk '{byte_size += $7} END{print byte_size}'
```
### calculcate size of files by type, list of files, sort files by size
```sh
du -hs * | sort -h
```

### calculator arithmethic operations add sub div multiply evaluation 
```bash
expr 30 / 5
myvar=$(expr 1 + 1)
```
```bash
python3 -c "print(4*3)"
perl -e "print 4*3"
```
desc calculator
```bash
echo "2 3 + p" | dc
```
basic calculator
```bash
echo "4+5" | bc
bc <<< 4+5
```
interactive calculator
```sh
bc -l -i
```

### sudo without password, apple keyboard, sudo script without password
```
echo 'password' | sudo -S bash -c "echo 2 > /sys/module/hid_apple/parameters/fnmode" 2>/dev/null
```

### default type, detect default browser, mime types, default application set default app
```sh
xdg-mime query default x-scheme-handler/http

## where accessible types
# echo $XDG_DATA_DIRS # avaialible in applications
# locate google-chrome.desktop
# /usr/share/applications/google-chrome.desktop

## set default browser 
xdg-mime default firefox.desktop x-scheme-handler/http
xdg-mime default firefox.desktop x-scheme-handler/https
xdg-settings set default-web-browser firefox.desktop

## check default association
cat ~/.config/mimeapps.list
cat /usr/share/applications/defaults.list
```
or change your alternatives
```sh
locate x-www-browser
# /etc/alternatives/x-www-browser
```

open in default browser
```sh
x-www-browser http://localhost:9090
```

### alternatives
set default browser
```sh
sudo update-alternatives --display x-www-browser
sudo update-alternatives --query x-www-browser
sudo update-alternatives --remove x-www-browser /snap/bin/chromium
sudo update-alternatives --remove x-www-browser /usr/bin/chromium
sudo update-alternatives --install /usr/bin/x-www-browser x-www-browser /usr/bin/chromium-browser 90
```
java set default
```sh
update-alternatives --install /usr/bin/java java $JAVA_HOME/bin/java 10
```

### open file with default editor, default viewer, with more appropriate viewr
```sh
# ranger should be installed 
rifle <path to file>
```

### cat replacement bat
```sh
# apt install bat - not working sometimes
cargo install bat
batcat textfile
# alias bat=batcat
bat textfile
```

## install haskell
```
sudo apt-get install haskell-stack
stack upgrade
stack install toodles
```
[get started with hackell](https://haskell.fpcomplete.com/get-started)

## shell examples
### byobu
```
sudo apt install byobu
```

### check architecture
```sh
dpkg --add-architecture i386
dpkg --print-architecture
dpkg --print-foreign-architectures
```

## move mouse, control X server
```
apt-get install xdotool
# move the mouse  x    y
xdotool mousemove 1800 500
# left click
xdotool click 1
```
pls, check that you are using Xorg and not Wayland:
```sh
# uncomment false
cat /etc/gdm3/custom.conf | grep WaylandEnable
```
how to check your current display server:
```sh
# x11 - xorg
# wayland
echo $XDG_SESSION_TYPE
```

### calendar, week number
```sh
gcal --with-week-number
```

## vnc server
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

### vnc server with connecting to existing X session
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


## apt 
### apt package description 
```sh
apt-cache show terminator
```
### apt cache
```sh
cd /var/cache/apt/archives
```
### apt force install 
```sh
sudo apt install --fix-broken -o Dpkg::Options::="--force-overwrite" {package name}
```

## Issues
### issue with go package installation
```sh
>pkg-config --cflags  -- devmapper
Package devmapper was not found in the pkg-config search path.
Perhaps you should add the directory containing `devmapper.pc'
to the PKG_CONFIG_PATH environment variable
No package 'devmapper' found
```
```sh
sudo apt install libdevmapper-dev
export PKG_CONFIG_PATH=`echo $(pkg-config --variable pc_path pkg-config)${PKG_CONFIG_PATH:+:}${PKG_CONFIG_PATH}`
```
