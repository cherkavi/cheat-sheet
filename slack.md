## slack
### generate token
https://api.slack.com/custom-integrations/legacy-tokens

### how to write message
https://api.slack.com/docs/messages/builder

### send message using web api
```
curl -X POST -H "Content-type: application/json" -H "Authorization: Bearer xoxp-283316862324-298911817009-298923149681-44f585044d66634f5701618e97cd1c0b" --data @data.json https://wirecard-issuing.slack.com/api/chat.postMessage
```
```
{"text":"another message from tech user", "channel":"team-brand-server"}
```
!!! for Windows need to escape " symbol
