# How to speedup your working activities, small advices about workplace improvements

## Linux 
### list of useful application:
* [screen shot](https://github.com/ksnip/ksnip)
* [local console in browser](https://github.com/subhra74/linux-web-console)
* [remote server utility](https://github.com/subhra74/snowflake)

## [jira cheat sheet](./jira-cheat-sheet.md)

## CodeBeamer
### CodeBeamer REST API
[swagger example](https://codebeamer.ubsgroup.net:8443/cb/v3/swagger/editor.spr)
```sh
# reading one page 
curl -v --insecure -X GET "https://codebeamer.ubsgroup.net:8443/cb/api/v3/wikipages/1343" -H "accept: application/json" -H "Authorization: Basic "`echo -n $TSS_USER:$TSS_PASSWORD | base64`
```
### CodeBeamer notice
if you are sending json encoded data to CB over the swagger API, check the definitions of the expected payloads – they might have changed without notice, breaking you calls  
CB rejects all requests with unknown information in the payload  
With some “methods” available over swagger the expected payload has changed  
 
Example: POST /v3/projects/{projectId}/content
```json
{
  "password": "xxx",
  "skipTrackerItems": false,
  "skipWikiPages": false,
  "skipAssociations": false,
  "skipDocuments": false,
  "skipReports": false,
  "skipBranches": false,
  "selectedTrackerIds": [
    0
  ]
}
```
or
```json
{
  "password": "xxx",
  "skipTrackerItems": false,
  "skipWikiPages": true,
  "skipAssociations": false,
  "skipReports": false,
  "selectedTrackerIds": [
    0
  ]
}
```

## Crucible
### activity
```
https://fisheye.wirecard.sys/user/thomas%40partner.com
```


## Windows
### color of console
```
cmd color 08
cmd color 0F
```

### stdout/stderr to stdout/stderr
```
ls > out.txt 2>&1
```

### autoexec on startup
* create batch file
* create shortcut
* run command: shell:startup
* move into opened window just created shortcut


## URL
[web sequence diagrams](https://www.websequencediagrams.com/)
