## Linux

### tunnel, port forwarding from local machine to outside
```
ssh -L <localport>:<remote host>:<remote port> <hostname>
ssh -L 28010:vldn337:8010 localhost
```

### tunnel, port forwarding from outside to localmachine
```
ssh -R <remoteport>:<local host name>:<local port> <hostname>
ssh -R 9020:127.0.0.1:9092 localhost
```
### gpg signature check, asc signature check, crt signature check
```
gpg --keyserver keyserver.ubuntu.com --recv-keys 9032CAE4CBFA933A5A2145D5FF97C53F183C045D gpg --import john-brooks.asc

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

### mount cdrom ( for virtual machine )
```
sudo mount /dev/cdrom /mnt
```

### sudo reboot
``` 
shutdown -r now
```

### sort, order
```
sort <filename>
```

### unique lines (duplications) into file
#### add count
```
uniq -c
```

#### duplicates
```
uniq -d
```

#### unique
```
uniq -u
```

### print column from file
```
cut --delimiter "," --fields 2,3,4 test1.csv
```

### log information
```
/var/log/messages
/var/log/syslog
```

### folder into bash script
working folder
```
pwd
```
folder of the script
```
`dirname $0`/another_script_into_folder.sh
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

### mc color, midnight commander
file:~/.mc/ini
```
[Colors]
base_color=normal=brightgray,black:marked=brightcyan,black:selected=black,lightgray:directory=white,black:errors=red,black:executable=brightgreen,black:link=brightblue,black:stalelink=red,black:device=brightmagenta,black:special=brightcyan,black:core=lightgray,black:menu=white,black:menuhot=brightgreen,black:menusel=black,white:editnormal=brightgray,black:editmarked=black,brightgreen:editbold=brightred,cyan
```
mc --nocolor

### find files by mask
```
locate -ir "brand-reader*"
locate -b "brand-reader"
```
### full path to file 
```
readlink -f {file}
```

### where is program placed, location of executable file
```
which "program-name"
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

### find large files
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


### postponed execution
```
at <date and time>
> "write commands"
^D
```

### find process by name
```
ps fC firefox
```
windows analogue of 'ps aux'
```
wmic path win32_process get Caption, Processid, Commandline
```

### kill -3
```
output to log stop process
```

### cron
```
crontab -l
```

### grep line before
```
grep -B 4
```

### grep line after
```
grep -A 4
```

### grep text into files
```
grep -rn '.' -e '@Table'
grep -ilR "@Table" .
```

### grep OR operation
```
cat file.txt | grep -e "occurence1" -e "occurence2"
```

### grep AND operation
```
cat file.txt | grep -e "occurence1" | grep -e "occurence2"
```

### grep not included, grep NOT
```
cat file.txt | grep -v "not-include-string"
```

### find inside zip file(s), grep zip, zip grep
```
zgrep "message_gateway_integration" /var/lib/brand-server/cache/zip/*.zip
```

### grep zip, find inside zip, inside specific file line of text
```
ls -1 *.zip | xargs -I{} unzip -p {} brand.xml  | grep instant-limit | grep "\\."
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

### wget to specific file
```
wget -O out.zip http://{host}:9000/published/resources/10050001.zip
```

### zip files, zip all files
```
zip -r bcm-1003.zip *
```

## AWK

### awk another delimiter
```
awk -F '<new delimiter>'
```
example of multi delimiter:
```
awk -F '[/, ]'
```

### awk print last column
```
awk '{print $NF}'
```

### [awk example of applying function and conditions](https://www.gnu.org/software/gawk/manual/html_node/)
```
ps -aux | awk '{if(index($1,"a.vcherk")>0){print $0}}'
ps -aux | awk '{print substr($0,1,20)}'
```

### awk one column output to two column separated comma
```
awk 'BEGIN{a=""}{if(a==""){a=$NF}else{print a","$NF; a=""}}'
```

### awk execute script from file
```
awk -f <filename>
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

### bash
```
[-n "$variable"] - non zero
[-z "$variable"] - zero
```

### last changed files, last updated file
```
find -cmin -2
```

### curl PUT example with file
```
curl -X PUT -H "Content-Type: application/vnd.wirecard.brand.apis-v1+json;charset=ISO-8859-1" -H "x-username: cherkavi" -d@put-request.data http://q-brands-app01.wirecard.sys:9000/draft/brands/229099017/model/country-configurations
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

### execute command with environment variable, new environment variable for command
```
ONE="this is a test"; echo $ONE
```

### system log file
```
/var/log/syslog
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

### remove service ( kubernetes )
* sudo invoke-rc.d localkube stop
* sudo invoke-rc.d localkube status
( sudo service localkube status )
* sudo update-rc.d -f localkube remove
* sudo grep -ir /etc -e "kube"
* rm -rf /etc/kubernetes
* rm -rf /etc/systemd/system/localkube.service
* vi /var/log/syslog

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
* /etc/profile.d/proxy.sh
```
export HTTP_PROXY=http://webproxy.host:3128
export http_proxy=http://webproxy.host:3128
export HTTPS_PROXY=http://webproxy.host:3128
export https_proxy=http://webproxy.host:3128
export NO_PROXY="localhost,127.0.0.1,.host,.viola.local"
export no_proxy="localhost,127.0.0.1,.host,.viola.local"
```

* /etc/environment 
```
http_proxy=http://webproxy.host:3128
no_proxy="localhost,127.0.0.1,.host.de,.viola.local"
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
