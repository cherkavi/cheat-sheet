# Linux

[cheat sheet cmd](http://cht.sh/), [chat sheet cmd](http://cheat.sh/)

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

### mount remote filesystem via ssh
```
sudo sshfs -o allow_other,IdentityFile=~/.ssh/id_rsa vcherkashyn@190.17.19.11:/ /mnt/vendor-cluster-prod
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

### avoid to put command into history, hide password into history, avoid history
add space before command

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
```
### all command line arguments to another program
```
original.sh $*
```

### auto execute during startup
```
folder: etc/rc1.d ( rc2.d ... )
contains links to ../init.d/<name of bash script>
should understand next options: start, stop, restart
```

reset X-server, re-start xserver, reset linux gui
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

### mc color, midnight commander
file:~/.mc/ini
```
[Colors]
base_color=normal=brightgray,black:marked=brightcyan,black:selected=black,lightgray:directory=white,black:errors=red,black:executable=brightgreen,black:link=brightblue,black:stalelink=red,black:device=brightmagenta,black:special=brightcyan,black:core=lightgray,black:menu=white,black:menuhot=brightgreen,black:menusel=black,white:editnormal=brightgray,black:editmarked=black,brightgreen:editbold=brightred,cyan
```
```
mc --nocolor
```

### find files by mask
```
locate -ir "brand-reader*"
locate -b "brand-reader"
```
### full path to file
```
readlink -f {file}
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

### find file by last update time
```
find / -mmin 2
```

### find files/folders by name and older than 240 min
```
find /tmp -maxdepth 1 -name "native-platform*" -mmin +240 | xargs sudo rm -r {} \; >/dev/null 2>&1
```

### find files/folders by regexp and older than 240 min
```
find /tmp -maxdepth 1 -mmin +240 -iname "[0-9]*\-[0-9]" | xargs sudo rm -r {} \; >/dev/null 2>&1
```

### find large files, find big files
```
find . -type f -size +50000k -exec ls -lh {} \;
find . -type f -size +50000k -exec ls -lh {} \; | awk '{ print $9 ": " $5 }'
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
```
crontab -l
```

### error to null
```
./hbase.sh 2>/dev/null
```
### grep with line number
```
grep -nr "text for search" .
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

### difference between two files without spaces
```
diff -w file1.txt file2.txt
```

### show difference in lines with context
```
diff -c file1.txt file2.txt
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

### sed, replace
```
"name : " with "nomen" string
sed 's/"name" : "/nomen/g'
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

### curl PUT example with file
```
curl -X PUT -H "Content-Type: application/vnd.wirecard.brand.apis-v1+json;charset=ISO-8859-1" -H "x-username: cherkavi" -d@put-request.data http://q-brands-app01.wirecard.sys:9000/draft/brands/229099017/model/country-configurations
```
```
curl -X POST http://localhost:8983/solr/collection1/update?commit=true -H "Content-Type: text/json" --data '{"add":"data"}'
```
```
curl -X POST http://localhost:8983/solr/collection1/update?commit=true -H "Content-Type: text/json" --data-binary '{"add":"data"}'
```

### curl without progress
* curl -s -X GET http://google.com
* curl --silent -X GET http://google.com
* curl  http://google.com 2>/dev/null

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

### DNS
```
systemd-resolve --status
```
```
dig {hostname}

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
service check logs
```
journalctl -u {service name}
```

check settings
```
systemctl show {service name} | grep proxy
```

* #### for snapd 
```
sudo systemctl edit snapd.service
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
permanent applying
```
vi /usr/share/X11/xorg.conf.d/80-touch.conf
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
```
xclip -o
cat file.txt | xclip
```

## wifi
```
ifconfig ( result - wlan0 )
airmon-ng check kill
airmon-ng check ( should be empty )
airmon-ng start wlan0 ( result - wlan0mon )
airodump-ng wlan0mon ( result - BSSID )
reaver -i wlan0mon -b <BSSID> -vv -K 1
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

## disk usage
```
ncdu
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

### sudo without password
```
echo 'password' | sudo -S bash -c "echo 2 > /sys/module/hid_apple/parameters/fnmode"
```

---
# useful links:
[web page like a screensaver](https://github.com/lmartinking/webscreensaver)
[jira editing shortcuts](https://jira.atlassian.com/secure/WikiRendererHelpAction.jspa?section=all)
