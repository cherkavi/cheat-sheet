[graph explorer, swagger](https://developer.microsoft.com/en-us/graph/graph-explorer)
[how to use](https://docs.microsoft.com/en-us/graph/graph-explorer/graph-explorer-features)


## microsoft teams example teams read all messages
** please, login to microsoft **
> afterward you can retrieve "Access token"
```sh
TOKEN=eyJ0eXAiOiJKV1QiLCJub25jZSI6I...
```

### "my joined teams"
```sh
curl 'https://graph.microsoft.com/v1.0/me/joinedTeams' -H "Authorization: Bearer $TOKEN" | jq .
```
> find out one of the channel and retrieve "id" of it, like: "id": "45626dcc-04da-4c2f-a72a-b28b",
```sh
GROUP_ID=45626dcc-04da-4c2f-a72a-b28b
```

## "members of the channel "
```sh
curl https://graph.microsoft.com/v1.0/groups/$GROUP_ID/members  -H "Authorization: Bearer $TOKEN" | jq .
```

## "channels of a team which I am member of"
```sh
curl https://graph.microsoft.com/v1.0/teams/$GROUP_ID/channels  -H "Authorization: Bearer $TOKEN" | jq .
```
> retrieve value.id of the channel, like: "id": "19:lApNBJeeI0aFWXNa7dqlbODC2ZkpwMYl8@thread.tacv2",
```sh
CHANNEL_ID="19:lApNBJeeI0aFWXNa7dqlbODC2ZkpwMYl8@thread.tacv2"
```

## read messages from the channel : "messages (without replies) in a channel"
```sh
curl https://graph.microsoft.com/beta/teams/$GROUP_ID/channels/$CHANNEL_ID/messages -H "Authorization: Bearer $TOKEN" | jq .value[].body.content
```

