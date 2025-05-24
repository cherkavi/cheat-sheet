# [Discord](https://discord.com/) 

## create link to your profile
1. copy UserId from your profile ( avatar in left bottom corner )
   > maybe need to activate "Development Mode"
   > settings -> Advanced: developer mode - on
2. insert your DISCORD_USER_ID at the end of the link:
   https://discord.com/users/$DISCORD_USER_ID

## Obtain BOT Token 
1. https://discord.com/developers/applications
2. new application 
3. bot
4. add bot
5. reset your token 
6. save your token in environment variable DISCORD_BOT_TOKEN

## add bot to your channel
> not possible to read direct messages


## read your messages from channel 
```sh
DISCORD_CHANNEL_ID=my_channel_id
curl -H "Authorization: Bot $DISCORD_BOT_TOKEN" "https://discord.com/api/v10/channels/${DISCORD_CHANNEL_ID}/messages?limit=10"
```