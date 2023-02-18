# Windows OS 
## CMD automation
```sh
start C:\soft\Open-Sankore\Open-Sankore.exe
start "" "C:\soft\PyCharm\bin\pycharm64.exe"

start im:"<sip:christian.xxxxx@external.ubs.com>"
start microsoft-edge:http://wirenet.ubs.lan/
start notepad++ %*
start notepad++ C:\drive\important.info
start %windir%\system32\SnippingTool.exe
```
```sh
call gradle -b c:\project\ccp-brand-server\server\build.gradle asciidoctor
```
## change environment
```
set JAVA_HOME=C:\soft\java\jdk1.8.0_65
set Path=C:\oracle\product\11.2.0\client_1\BIN\;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;C:\Program Files\TortoiseHg\;C:\Program Files\MicroStrategy\MicroStrategy Desktop\DataServices\180_77\64-bit\bin\server;C:\Users\Vitalii.Cherkashyn;C:\soft\git\bin;C:\soft\gradle\bin;C:\soft\java\jdk1.8.0_65\bin;C:\soft\maven\bin;C:\Users\Vitalii.Cherkashyn\AppData\Local\atom\bin;C:\soft\notepad++;C:\Python35;C:\Python35\Scripts;C:\Users\Vitalii.Cherkashyn\.babun;C:\soft\cygwin64\bin;C:\"Program Files (x86)"\PuTTY\;C:\"Program Files (x86)"\"Microsoft Office"\Office15;C:\soft\"OpenOffice 4"\program;C:\soft\groovy\groovy-2.4.8\bin;C:\soft\automation
doskey python3=C:\Python35\python.exe $*
```
### putty
```sh
@C:\soft\ssh\putty.exe -pw my_secret_password  a.vcherk@d-horus-stk03.ubs.sys
```
### DB
#### oracle
```
rem sqlplus CHERKASHYN_V/my_password@q-ora-db-scan.ubs.sys:1521/stx11de.ubs
rem sqlplus CHERKASHYN_V@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=q-ora-db-scan.ubs.sys)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=stx11qa.ubs)))

sqlplus brand_server_data_dev/brand_server_data_dev@d-xd01-scan.ubs.sys:1521/BUILD.UBS %*
```
