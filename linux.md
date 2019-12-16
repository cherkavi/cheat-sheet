# Linux

* [cheat sheet cmd](http://cheat.sh/)  
* [Security cheat sheet](https://www.jaiminton.com/cheatsheet/DFIR)

### socket proxy, proxy to remote machine
```
ssh -D <localport> <user>@<remote host>
```
and checking if it is working for 'ssh -D 7772 cherkavi@151.190.211.1'
```
ssh -o "ProxyCommand nc -x 127.0.0.1:7772 %h %p" cherkavi@151.190.211.47
```

### tunnel, port forwarding from local machine to outside
```
ssh -L <localport>:<remote host>:<remote port> <hostname>
ssh -L 28010:vldn337:8010 localhost
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
```
ssh -R <remoteport>:<local host name>:<local port> <hostname>
ssh -R 9020:127.0.0.1:9092 localhost
```

### possible solution to detect remote client
```
# open access
ping -s 120 -c 1 146.255.193.66
ping -s 121 -c 1 146.255.193.66
ping -s 122 -c 1 146.255.193.66

# close access
ping -s 123 -c 1 146.255.193.66
```

### mount remote filesystem via ssh, map folder via ssh, ssh remote folder
```
sudo mkdir /mnt/vendor-cluster-prod
sudo sshfs -o allow_other,IdentityFile=~/.ssh/id_rsa vcherkashyn@190.17.19.11:/remote/path/folder /mnt/vendor-cluster-prod
# sudo fusermount -u /remote/path/folder
# sudo umount /remote/path/folder
```

### mount remote filesystem via ftp
```
sudo apt install curlftpfs
sudo mkdir /mnt/samsung-note
curlftpfs testuser:testpassword@192.168.178.20:2221 /mnt/samsung-note/
```

### gpg signature check, asc signature check, crt signature check
```
kgpg --keyserver keyserver.ubuntu.com --recv-keys 9032CAE4CBFA933A5A2145D5FF97C53F183C045D
gpg --import john-brooks.asc

gpg --verify ricochet-1.1.4-src.tar.bz2.asc

gpg --keyserver keyserver.ubuntu.com --recv-keys D09FB15F1A24768DDF1FA29CCFEEF31651B5FDE8
```

### connect to remote machine via ssh without credentials
```
ssh-keygen -t rsa
```
( check created file /home/{user}/.ssh/id_rsa )

```
ssh-copy-id {username}@{machine ip}:{port}
ssh-copy-id -i ~/.ssh/id_rsa.pub -o StrictHostKeyChecking=no vcherkashyn@bmw000013.adv.org
```
login without typing password
```
sshpass -p my_password ssh my_user@192.178.192.10
```

automate copying password
```
#!/usr/bin/expect -f
spawn ssh-copy-id vcherkashyn@host000159
expect "(yes/no)?"
send "yes\n"
expect "password: "
send "my_password\n"
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

the same, but manually:
```
cat .ssh/id_rsa.pub | ssh {username}@{ip}:{port} "cat >> ~/.ssh/authorized_keys"
chmod 700 ~/.ssh ;
chmod 600 ~/.ssh/authorized_keys
```

### install ssh
```
sudo apt get ssh
sudo service ssh start
```

### copy from local machine to remote one, remote copy
```
scp filename.txt cherkavi@129.191.200.15:~/temp/filename-from-local.txt
```
### copy from remote machine to local
```
scp -r cherkavi@129.191.200.15:~/temp/filename-from-local.txt filename.txt 
```

### copy directory to remote machine, copy folder to remote machine
```
scp -pr /source/directory user@host:the/target/directory
```
the same as local copy folder
```
cp -var /path/to/folder /another/path/to/folder
```
### syncronize folders, copy everything between folders
```bash
# local sync
rsync -r /tmp/first-folder/ /tmp/second-folder
# remote sync
rsync -avh /tmp/local-folder/ root@remote-host:/tmp/remote-folder
# remote sync with specific port
rsync -azh /tmp/local-folder/ -e 'ssh -p 2233' root@remote-host:/tmp/remote-folder
```

### create directory on remote machine, create folder remotely
```
ssh user@host "mkdir -p /target/path/"
```

### here document, sftp batch command with bash
```
sftp -P 2222 my_user@localhost << END_FILE_MARKER
ls
exit
END_FILE_MARKER
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

### repeat command with predefined interval, execute command repeatedly
```
watch -n 60 'ls -la | grep archive'
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
```
sort <filename>
```
sort by column ( space delimiter )
```
sort -k 3 <filename>
```
sort with reverse order
```
sort -r <filename>
```

### split and merge big files
```
split --bytes=1M /path/to/image/image.jpg /path/to/image/prefixForNewImagePieces

cat prefixFiles* > newimage.jpg
```

### cut big file, split big file, cat after threshold
```
cat --lines=17000 big_text_file.txt
```

### unique lines (duplications) into file
#### add counter and print result
```
uniq -c
```

#### duplicates
print only duplicates ( distinct )
```
uniq -d
```
print all duplications
```
uniq -D
```

#### unique
```
uniq -u
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

### log information
```
/var/log/messages
/var/log/syslog
```

### add repository
```
add-apt-repository ppa:inkscape.dev/stable
```
you can find additional file into
```
/etc/apt/sources.list.d
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

### bash settings, history lookup
~/.inputrc
```
"\e[A": history-search-backward
"\e[B": history-search-forward
set show-all-if-ambiguous on
set completion-ignore-case on
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
```
pwd
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

### auto execute during startup, autoexec
```
folder: /etc/rc1.d ( rc2.d ... )
contains links to ../init.d/<name of bash script>
should understand next options: start, stop, restart
```
```
#! /bin/sh
# /etc/init.d/blah
#

# Some things that run always
touch /var/lock/blah

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
or
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
```
sudo systemctl enable YOUR_SERVICE_NAME
sudo systemctl start YOUR_SERVICE_NAME
sudo systemctl status YOUR_SERVICE_NAME
sudo systemctl daemon-reload YOUR_SERVICE_NAME
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

### mc color, midnight commander
file:~/.mc/ini
```
[Colors]
base_color=normal=brightgray,black:marked=brightcyan,black:selected=black,lightgray:directory=white,black:errors=red,black:executable=brightgreen,black:link=brightblue,black:stalelink=red,black:device=brightmagenta,black:special=brightcyan,black:core=lightgray,black:menu=white,black:menuhot=brightgreen,black:menusel=black,white:editnormal=brightgray,black:editmarked=black,brightgreen:editbold=brightred,cyan
```
```
mc --nocolor
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

### folder name from path
```
dirname {file}
```

### print full path to files inside folder
```
ls -d <path to folder>/*
```

### real path to link
```
readlink 'path to symlink'
```

### where is program placed, location of executable file
```
which "program-name"
```

### find file by name
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
### find file, search file, skip permission denied
```
find . -name "prd-ticket-1508.txt"  2>&1 | grep -v "Permission denied"
```

### find multiply patterns
```
find . -name "*.j2" -o -name "*.yaml"
```

### find file by last update time
```
find / -mmin 2
```

### find files/folders by name and older than 240 min
```
find /tmp -maxdepth 1 -name "native-platform*" -mmin +240 | xargs -I {} sudo rm -r {} \; >/dev/null 2>&1
```

### find files/folders by regexp and older than 240 min
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

### find with excluding folders, find exclude
```sh
find . -type d -name "dist" ! -path  "*/node_modules/*"
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

### disconnect from terminal and let command be runned
```
ctrl-Z
disown -a && exit
```

### postponed execution
```
at <date and time>
> "write commands"
^D
```

### find process by name
```
ps fC firefox
pgrep firefox
```

pid of process by name
```
pidof <app name>
pidof chrome
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

### youtube
```
 youtube-dl --list-formats https://www.youtube.com/watch?v=nhq8e9eE_L8
 youtube-dl --format 22 https://www.youtube.com/watch?v=nhq8e9eE_L8

```

### cron
all symbols '%' must be converted to '\%'
```
crontab -e
crontab -l
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

### error to null
```
./hbase.sh 2>/dev/null
```

### grep with line number
```
grep -nr "text for search" .
```
### grep only in certain folder without recursion
```
# need to set * or mask for files in folder !!!
grep -s "search_string" /path/to/folder/*
sed -n 's/^search_string//p' /path/to/folder/*
```

### grep line before
```
grep -B 4
grep --before 4
```

### grep line after
```
grep -A 4
grep --after 4
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
```
cat file.txt | grep -e "occurence1" | grep -e "occurence2"
```

### grep not included, grep NOT
```
cat file.txt | grep -v "not-include-string"
```

### grep with file mask
```
grep -ir "memory" --include="*.scala"
```

### grep with regexp
```
grep -ir --include=README.md ".*base" 2>/dev/null
```

### grep without permission denied
```
grep -ir --include=README.md "base" 2>/dev/null
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

### show difference between two dates, date difference, time difference
```sh
apt install dateutils
dateutils.ddiff -i '%Y%m%d%H%M%S' -f '%y %m %d %H %M %S' 20160312000101 20170817040001
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

### generate random string 
```
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

### unzip tar file from url, wget unzip, wget untar
```
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

### command prompt, change console prompt
```
export PROMPT_COMMAND="echo -n \[\$(date +%H:%M:%S)\]\ "
```

### last executed exit code
```
echo $?
```

### memory dump
```
cat /proc/meminfo
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
```
sensible-browser http://localhost:3000/api/status
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
```

### wget to specific folder
```sh
wget http://host:9090/wd-only/1005000.zip --directory-prefix="/home/temp/out"
```

### wget https without checking certificate
```
wget --no-check-certificate https://musan999999.mueq.adas.intel.com:8888/data-api/session/
```

### zip files, zip all files
```
zip -r bcm-1003.zip *
```
### using parameters for aliases
```
alias sublime_editor=/Applications/SublimeEditor/Sublime

subl(){
  sublime_editor "$1" &
}
```

### [sed cheat sheet](https://gist.github.com/ssstonebraker/6140154), replace
```
replace "name" with "nomen" string
sed 's/name/nomen/g'

# replace only second occurence
# echo "there is a test is not a sentence" | sed 's/is/are/2'
```
example of replacing all occurences in multiply files
```
for each_file in `find -name "*.java"`; do
	sed --in-place 's/vodkafone/cherkavi/g' $each_file
done
```

### print line by number from output, line from pipeline
```
locate -ir "/zip$" | sed -n '2p'
```

### calculate amount of strings
```
ps -aux | awk 'BEGIN{a=0}{a=a+1}END{print a}'
```

### last changed files, last updated file
```
find -cmin -2
```

## cURL
### curl username, curl with user and password, curl credentials
```
curl -u username:password http://example.com
```

### curl PUT example with file
```
curl -X PUT -H "Content-Type: application/vnd.wirecard.brand.apis-v1+json;charset=ISO-8859-1" -H "x-username: cherkavi" -d@put-request.data http://q-brands-app01.wirecard.sys:9000/draft/brands/229099017/model/country-configurations
```
```
curl -X POST http://localhost:8983/solr/collection1/update?commit=true -H "Content-Type: text/json" --data '{"add":"data"}'
```
```
curl -X POST http://localhost:8983/solr/collection1/update?commit=true -H "Content-Type: text/json" --data-binary '{"add":"data"}'
# or with bash variable
SOME_DATA="my_personal_value"
curl -X POST http://localhost:8983/solr/collection1/update?commit=true -H "Content-Type: text/json" --data-binary '{"add":"'$SOME_DATA'"}'
# or with data from file
curl -X POST http://localhost:8983/test -H "Content-Type: text/json" --data-binary '@/path/to/file.json'
# or with multipart body
curl -i -X POST -H "Content-Type: multipart/form-data" -F "data=@test.mp3" -F "userid=1234" http://mysuperserver/media/upload/
```

### curl without progress
* curl -s -X GET http://google.com
* curl --silent -X GET http://google.com
* curl  http://google.com 2>/dev/null

### curl certificate skipping
```
curl --insecure -s -X GET http://google.com
```
### curl with additional output, curl verbosive mode
```sh
curl --verbose --insecure -s -X GET http://google.com
```
### curl cookie
chrome extension ```cookies.txt```
```sh
# send predefined cookie to url
curl -b path-to-cookie-file.txt -X GET url.com

# send cookie from command line
curl --cookie "first_cookie=123;second_cookie=456;third_cookie=789" -X GET url.com

# collect cookie from remote url and save in file
curl -c cookie-from-url-com.txt -X GET url.com
```

### curl with encoding to another codepage, from win1251 to utf8
```sh
curl "http://some.resource/read_book.php?id=66258&p=1" | iconv --from-code WINDOWS-1251 --to-code UTF-8
```

### xml pretty print, xml format
```
xmllint --format /path/to/file.xml > /path/to/file-formatted.xml
```

### xml validation
```sh
xmllint --noout file.xml; echo $?
```

### json output pretty print, json pretty print
```
echo output.json | jq .
```

### [parsing json, json processing](https://stedolan.github.io/jq/manual/)
[jq playground](https://jqplay.org/jq?q=.[%22foo%22]&j={%22foo%22%3A%2042})
```bash
echo '[{"id": 1, "name": "Arthur", "age": "21"},{"id": 2, "name": "Richard", "age": "32"}]' | \
jq ".[] | .name"

echo '[{"id": 1, "name": "Arthur", "age": "21"},{"id": 2, "name": "Richard", "age": "32"}]' | \
jq '.[] | if .name == "Richard" then . else empty end | [.id, .name] | @csv'
```

### chmod recursively
```
chmod -R +x <folder name>
```
```
find . -name "*.sql" -print0 | xargs -0 chmod 666
```

### create dozen of folders using one-line command
```
mkdir -p some-folder/{1..10}/{one,two,three}
```

### execute command with environment variable, new environment variable for command
```
ONE="this is a test"; echo $ONE
```

### system log file
```
/var/log/syslog
```
### Debian install package via proxy
```
sudo http_proxy='http://user:@proxy.muc:8080' apt install meld
```

### install package for another architecture, install x86 on x64
```
dpkg --add-architecture i386
dpkg --print-architecture
dpkg --print-foreign-architectures
sudo apt-get install libglib2.0-0:i386 libgtk2.0-0:i386
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

### version of OS, linux version
* lsb_release -a
* cat /etc/system-release
* uname -a

### print all networks
```
ip -4 a
ip -6 a
```

### print all network interfaces
```
interfaces
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
```
nmcli connection
nmcli connection up id {name from previous command}
nmcli connection down id {name of connection}
```
### raw vpn connection
```
sudo openconnect --no-proxy {ip-address} --user={user name}
sudo openconnect --no-cert-check --no-proxy {ip-address} --user={user name} ---servercert
```

### debug network collaboration, ip packages
example with reading redis collaboration ( package sniffer )
```sh
sudo ngrep -W byline -d docker0 -t '' 'port 6379'
```

### DNS
```
systemd-resolve --status
```
```
dig {hostname}

```

### encrypt file, decrypt file
```
gpg --symmetric {filename}
gpg --decrypt {filename}
```

### add user into special group
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
```

### remove user
```
sudo userdel -r test
```

### print all logged in users
```
users
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
* for user /etc/profile.d/proxy.sh
```
export HTTP_PROXY=http://webproxy.host:3128
export http_proxy=http://webproxy.host:3128
export HTTPS_PROXY=http://webproxy.host:3128
export https_proxy=http://webproxy.host:3128
export NO_PROXY="localhost,127.0.0.1,.host,.viola.local"
export no_proxy="localhost,127.0.0.1,.host,.viola.local"
```

* #### global /etc/apt/apt.conf 
```
Acquire::http::proxy "http://proxy.company.com:80/";
Acquire::https::proxy "https://proxy.company.com:80/";
Acquire::ftp::proxy "ftp://proxy.company.com:80/";
Acquire::socks5::proxy "socks://127.0.0.1:1080/";
```

* #### global /etc/environment
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

restart service, service restart
```
$ sudo systemctl daemon-reload
$ sudo systemctl restart {service name}
```

enable automatic start, disable autostart
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
```

### apache
[manage httpd](https://httpd.apache.org/docs/current/stopping.html)

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

### remap [jlki] to [Left, Right, Down, Up]
file .Xmodmap
```
keycode 66 = Mode_switch
keysym j = j J Left 
keysym l = l L Right
keysym i = i I Up
keysym k = k K Down
```
execute re-mapping
```
xmodmap .Xmodmap
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
```
base64 cAdvisor-start.sh | base64 --decode
echo "just a text string" | base64 | base64 --decode
```
### md5
```
md5sum
```

## Touch screen
### calibration
tool installation
```
sudo apt install xinput-calibrator
```
configuration
```
xinput_calibration
```
list of all devices
```
xinput --list
```
permanent applying
```
vi /usr/share/X11/xorg.conf.d/80-touch.conf
```
disable device
```
xinput --disable {number from command --list}
```

## Keyboard Lenovo
### middle button
```bash
# check input source - use name(s) for next command
xinput
# create file and add content
sudo vim /usr/share/X11/xorg.conf.d/50-thinkpad.conf
```
```
Section "InputClass"
    Identifier  "Trackpoint Wheel Emulation"
    MatchProduct    "Lenovo ThinkPad Compact USB Keyboard with TrackPoint|ThinkPad Extra Buttons"
    MatchDevicePath "/dev/input/event*"
    Option      "EmulateWheel"      "true"
    Option      "EmulateWheelButton"    "2"
    Option      "Emulate3Buttons"   "false"
    Option      "XAxisMapping"      "6 7"
    Option      "YAxisMapping"      "4 5"
EndSection
```

### recover usb drive
```
sudo fdisk -l
sudo lsblk
sudo fsck /dev/sdb
e2fsck -b 32768 /dev/sdb
sudo e2fsck -b 32768 /dev/sdb
sudo dd if=/dev/zero of=/dev/sdb
sudo fdisk /dev/sdb
sudo partprobe -s
sudo mkfs.vfat -F 32 /dev/sdb
sudo dd if=/dev/zero of=/dev/sdb bs=512 count=1
sudo fdisk /dev/sdb
```

## home automation
### DTMF generator
```
sox -n dtmf-1.wav synth 0.1 sine 697 sine 1209 channels 1
sox -n dtmf-2.wav synth 0.1 sine 697 sine 1336 channels 1
sox -n dtmf-3.wav synth 0.1 sine 697 sine 1477 channels 1
```

## pdf
### convert pdf to image
```
convert -geometry 400x600 -density 100x100 -quality 100 test-pdf.pdf test-pdf.jpg
```
### bar code create into pdf
```
barcode -o 1112.pdf -e "code39" -b "1112" -u "mm" -g 50x50
```

### bar code finder
```
apt install zbar-tool
zbarimg <file>
```

### pdf file merge, pdf join
```
gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -sOutputFile=finished.pdf test-pdf2.pdf test-pdf3.pdf test-pdf4.pdf
```
## zip
### unzip bz2
```
bzip2 -dc ricochet-1.1.4-src.tar.bz2 | tar xvf -
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
http://cups.org - printer installation
http://localhost:631/admin
in case of authorization issue:

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
```
sudo service cups restart
```

## kernel related messages
```
dmesg --level=err,warn
dmesg --follow
# save all messages /var/log/dmesg
dmesg -S
```

## disk usage
```
ncdu
```

## create startup disk, write iso image, usb stick, bootable drive
```bash
# list of all hard drives, disk list
sudo lshw -class disk -short
# write image
sudo dd bs=4M if=/home/my-user/Downloads/archlinux-2019.07.01-x86_64.iso of=/dev/sdb status=progress && sync
```

## Elapsed time between two commands
```
STARTTIME=$SECONDS
sleep 2
echo $SECONDS-$STARTTIME
```

```
STARTTIME=`date +%s.%N`
sleep 2.5
ENDTIME=`date +%s.%N`
TIMEDIFF=`echo "$ENDTIME - $STARTTIME" | bc | awk -F"." '{print $1"."substr($2,1,3)}'`
```

## vim
[vim cheat sheet](http://najomi.org/vim)
### copy-paste
* v - *visual* selection ( start selection )
* y - *yank* ( end selection )
* p - *paste* into position
* u - *undo* last changes
* ctrl-r - *redo* last changes

## sound
### join files
```
sox 1.wav 2.wav 3.wav 4.wav output.wav
ffmpeg -i 1.wav -i 2.wav -i 3.wav output.wav
```

# MacOS
## bashrc replacement
.bashrc - .bash_profile

## package manager
brew


## copy users, import/export users
```
sudo awk -F: '($3>=LIMIT) && ($3!=65534)' /etc/passwd > passwd-export
sudo awk -F: '($3>=LIMIT) && ($3!=65534)' /etc/group > /opt/group-export
sudo awk -F: '($3>=LIMIT) && ($3!=65534) {print $1}' /etc/passwd | tee - | egrep -f - /etc/shadow > /opt/shadow-export
sudo cp /etc/gshadow /opt/gshadow-export
```

# AWK
### single quota escape
```
\x27
```
example:
```
a=$((a+`zip_textfiles part_0 part_0.txt | awk '{if(NF>=5){print $4"/"$5}}' | awk -F '/' '{print $14" "$15"/"$16"/"$17" "$18}' | python sql-update-files-with-md5sum.py`))

awk '{print "a=$((a+`zip_textfiles "$2" "$2".txt | awk \x27 "}'
| awk -F \'/\' \'{print $14\" \"$15\"/\"$16\"/\"$17\" \"$18}\' | python sql-update-files-with-md5sum.py\`)); echo \"update "$2".txt"}' > update-db-from-files.sh
```

### last index of, lastIndexOf, substring
```
head completed2.files.list  | awk -F '/' '{print substr($0, 1, length($0) - length($NF))}'
```

### awk another FieldSeparator
```
awk -F '<new delimiter>'
```
example of multi delimiter:
```
awk -F '[/, ]'
```
example of determination delimiter in code 
```
awk 'BEGIN{FS=",";}{print $2}'
```

### print NumberofFields
```
awk '{print NF}'
```

### awk print last column
```
awk '{print $NF}'
```

### print NumberofRow
```
awk '{print NR}'
```

### awk OutputFieldSeparator
```
awk 'BEGIN{OFS="<=>"}{print $1,$2,$3}'
```

### [awk example of applying function and conditions](https://www.gnu.org/software/gawk/manual/html_node/)
```
ps -aux | awk '{if(index($1,"a.vcherk")>0){print $0}}'
ps -aux | awk '{print substr($0,1,20)}'
```

### awk execute script from file
```
awk -f <filename>
```
### awk print last element
```
print($NF)
```
### awk print all elements
```
print($0)
```

### awk complex search, print line below
```
BEGIN{
  need_to_print = 0
}
{
    if(need_to_print >0){
        print $N
        need_to_print = need_to_print - 1
    }else{
    if( index($N, "Exception")>0 && index($N, "WrongException")==0 )  {
      if(index($N,"[ERROR")==1 || index($N,"[WARN")==1){
        print $N
        need_to_print = 3
      }
    }
    }
}
```

### calculcate size of files by type
```
find . -name "*.java" -ls | awk '{byte_size += $7} END{print byte_size}'
```

### reset Gnome to default
```
rm -rf .gnome .gnome2 .gconf .gconfd .metacity .cache .dbus .dmrc .mission-control .thumbnails ~/.config/dconf/user ~.compiz*
```

### restart Gnome shell
```sh
alt-F2 r
```

### install drivers, update drivers ubuntu
```
sudo ubuntu-drivers autoinstall
```

### sudo without password
```
echo 'password' | sudo -S bash -c "echo 2 > /sys/module/hid_apple/parameters/fnmode"
```

### default type, detect default browser, mime types
```
xdg-mime query default x-scheme-handler/http
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

## icaclient
### [download receiver](https://www.citrix.de/downloads/citrix-receiver/)

### sudo apt remove icaclient
```sh
sudo dpkg --add-architecture i386
```

### check architecture
```sh
dpkg --add-architecture i386
dpkg --print-architecture
dpkg --print-foreign-architectures
```

### install dependencies
```sh
#sudo apt-get install ia32-libs ia32-libs-i386 libglib2.0-0:i386 libgtk2.0-0:i386
sudo apt-get install libglib2.0-0:i386 libgtk2.0-0:i386
sudo apt-get install gcc-multilib
sudo apt-get install libwebkit-1.0-2:i386 libwebkitgtk-1.0-0:i386
sudo dpkg --install icaclient_13.10.0.20_amd64.deb
```

## i3wm
### [custom status bar](https://py3status.readthedocs.io/en/latest/intro.html#installation)

### exit from i3 window manager
```
bindsym $mod+Shift+e exec i3-msg exit
```
## external monitor settings, external screen, external display
```monitor.sh
#!/bin/sh
xrandr --output $1
xrandr --output $2 --auto --right-of $1
xrandr --output $3 --auto --right-of $2
```
```
xrandr | grep " connected" | awk '{print $1}'
 ./monitor.sh "DP-4" "DP-1-3" "eDP-1-1"
```

or just install 'arandr' and generate bash script 
```
sudo apt install arandr
```
## move mouse, control X server
```
apt-get install xdotool
# move the mouse  x    y
xdotool mousemove 1800 500
# left click
xdotool click 1
```

## control mouse from keyboard
```
sudo apt-get install keynav
killall keynav 
cp /usr/share/doc/keynav/keynavrc ~/.keynavrc
keynav ~/.keynavrc
```
example of custom configfile
```
clear
daemonize
Super+j start,cursorzoom 400 400
Escape end
shift+j cut-left
shift+k cut-down
shift+i cut-up
shift+l cut-right
j move-left
k move-down
i move-up
l move-right
space warp,click 1,end
Return warp,click 1,end
1 click 1
2 click 2
3 click 3
w windowzoom
c cursorzoom 400 400
a history-back
Left move-left 10
Down move-down 10
Up move-up 10
Right move-right 10
```

---
# useful links:
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

---
# bluejeans installation ubuntu 18+
```sh
# retrieve all html anchors from url, html tags from url
curl -X GET https://www.bluejeans.com/downloads | grep -o '<a .*href=.*>' | sed -e 's/<a /\n<a /g' | sed -e 's/<a .*href=['"'"'"]//' -e 's/["'"'"'].*$//' -e '/^$/ d' | grep rp

sudo alien --to-deb bluejeans-1.37.22.x86_64.rpm 
sudo dpkg -i bluejeans_1.37.22-2_amd64.deb 

sudo apt install libgconf-2-4 
sudo ln -s /lib/x86_64-linux-gnu/libudev.so.1 /lib/x86_64-linux-gnu/libudev.so.0

sudo ln -s /opt/bluejeans/bluejeans-bin /usr/bin/bluejeans
```
---
# vim
## [vim plugin](https://github.com/junegunn/vim-plug)
file ```.vimrc``` should have next content: 
```
if empty(glob('~/.vim/autoload/plug.vim'))
  silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
    \ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
  autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif

call plug#begin('~/.vim/plugged')
Plug 'vim-airline/vim-airline'
call plug#end()

set laststatus=1
```
```sh
git clone https://github.com/vim-airline/vim-airline ~/.vim/plugged/vim-airline
```


## .vim folder example
```
.vim
├── autoload
│   └── plug.vim
├── colors
│   └── wombat.vim
├── pack
│   └── plugins
└── plugged
    ├── goyo.vim
    ├── lightline.vim
    ├── limelight.vim
    ├── seoul256.vim
    ├── vim-airline
    └── vim-airline-themes
```
---
# vifm
## colorschema
copy to ```~/.config/vifm/colors``` [color scheme](https://vifm.info/colorschemes.shtml)  
```:colorscheme <tab>```

---
# visual code plugins
```
vscjava.vscode-java-debug
peterjausovec.vscode-docker
ryu1kn.edit-with-shell
inu1255.easy-shell
vscjava.vscode-java-dependency
vscjava.vscode-java-pack
vscjava.vscode-java-test
redhat.java
yzhang.markdown-all-in-one
vscjava.vscode-maven
ms-python.python
liximomo.remotefs
scala-lang.scala
visualstudioexptteam.vscodeintellicode
miguel-savignano.terminal-runner
```
