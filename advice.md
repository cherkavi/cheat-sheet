## JIRA
### Jira Query Language, JQL
[jql search](https://www.atlassian.com/blog/jira-software/jql-the-most-flexible-way-to-search-jira-14)
reporter = currentUser() and status = open ORDER BY createdDate DESC
reporter = currentUser() and status in ('open', 'new') ORDER BY createdDate DESC
reporter = currentUser() and status = 'in progress' ORDER BY createdDate DESC
text ~ "is_abstract" and project = "Brand Configuration Management"


## Crucible
### activity
https://fisheye.wirecard.sys/user/thomas%40partner.com


## Windows
### color of console
cmd color 08
cmd color 0F

### stdout/stderr to stdout/stderr
ls > out.txt 2>&1

### autoexec on startup
create batch file
create shortcut
run command: shell:startup
move into opened window just created shortcut


## slack
### generate token
https://api.slack.com/custom-integrations/legacy-tokens

### how to write message
https://api.slack.com/docs/messages/builder

### send message using web api
curl -X POST -H "Content-type: application/json" -H "Authorization: Bearer xoxp-283316862324-xxxxxx" --data @data.json https://wirecard-issuing.slack.com/api/chat.postMessage
{"text":"another message from tech user", "channel":"team-brand-server"}
!!! for Windows need to escape " symbol


## URL
[web sequence diagrams](https://www.websequencediagrams.com/)
