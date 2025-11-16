# Telegram 
## [Telegram API](https://core.telegram.org/api) steps:
1. [create credentials](https://my.telegram.org/auth?to=apps)
2. [show your personal api id and api_hash](https://my.telegram.org/apps)
3. api_id and api_hash

## Telegram Bot

### [Bot API](https://core.telegram.org/bots)
* [create simple bot via BotFather](https://core.telegram.org/bots/tutorial)
  * [java bot TMA hello world](https://core.telegram.org/bots/tutorial)  
  * [java bot project](https://core.telegram.org/bots/tutorial#create-your-project)
* [telegram bot api commands](https://core.telegram.org/bots/api)
  * https://core.telegram.org/bots
  * https://core.telegram.org/bots/faq

### Bot API commands
```sh
curl https://api.telegram.org/bot$TELEGRAM_TOKEN_BOT/getMe
```

### telegram bot python SDK
```sh
pip3 install --break-system-packages telegram-bot
# sudo apt install python3-telegram-bot

# Issue: python setup.py egg_info did not run successfully.
pip install git+https://github.com/python-telegram-bot/python-telegram-bot.git
```

```python
from telegram import InlineKeyboardButton, InlineKeyboardMarkup 
keyboard = [[InlineKeyboardButton("Open App", web_app=WebAppInfo(url="https://url.com"))]] 
reply_markup = InlineKeyboardMarkup(keyboard) 
bot.send_message(chat_id=chat_id, text="my app:", reply_markup=reply_markup)
```

## Telegram Mini App

### TMA - Telegram Mini App - how to create simple application 
* [JS TMA basic example](https://docs.ton.org/v3/guidelines/dapps/tma/tutorials/app-examples)  
  [JavaScript source code](https://github.com/telegram-mini-apps-dev/vanilla-js-boilerplate/blob/master/index.html)
* [how to build tma](https://adsgram.ai/mini-application-in-telegram-what-it-is-and-how-to-create-it/)

### TMA singularities
* size: 509x690

### direct link to app
1. /newapp
2. @mysecret_bot  
   name of the bot
3. title of the app
4. description of the app
5. 640x360 image
6. /empty
7. https://my-html-app.com
   direct link to the html resource with app
8. myapp
   url path for your app
9. https://t.me/mysecret_bot/myapp

### java sdk
* [telegram bot java library](https://github.com/rubenlagus/TelegramBots)

### TMA build example
> just an example of building simple application based on https existing web page
1. https://t.me/BotFather
2. `/newbot`
3. enter name of your bot ( like a name of your domain )
4. enter name of main user ( like a name of your domain + '_bot' )
5. "Bot Settings" -> "Channel Admin Rights"
6. "Bot Settings" -> "Group Admin Rights"
7. same the 'url' and 'token' 
8. create your own html web page with one additional link:
```html
<script src="https://telegram.org/js/telegram-web-app.js"></script>
```
9. host it ( you just need to provide https:// link )
10. update your bot:
   'mybots' -> '<select your bot>' -> 'bot settings' -> 'configure menu button' -> <set url to https resource>
11. create Telegram Channel (write new message -> new channel)
  * add your bot as an admin
  * set rights for it
12. send message to channel ( for opening as external URL )  
```sh
CHAT_ID='-10031409999999'
APP_URL='https://some-url.com/'

# https://core.telegram.org/bots/api
curl -X POST "https://api.telegram.org/bot$TELEGRAM_TOKEN_BOT/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{
    "chat_id": "'$CHAT_ID'",
    "text": "Try the app",
    "reply_markup": {
      "inline_keyboard": [
        [
          {
            "text": "Open App",
            "url": "'$APP_URL'"
          }
        ]
      ]
    }
  }'
```
or send to privat chat as web_app ( will be opened as application inside Telegram)
```sh
curl -X POST "https://api.telegram.org/bot$TELEGRAM_TOKEN_BOT/sendMessage" \
  -H "Content-Type: application/json" \
  -d '{
    "chat_id": "271900000000",
    "text": "Try the app:",
    "reply_markup": {
      "inline_keyboard": [
        [
          {
            "text": "Open App",
            "web_app": {"url": "'$APP_URL'"}
          }
        ]
      ]
    }
  }'
```
    

## TDLib - Telegram Database Library to build Telegram App ( like CLI )
* [Telegram Database Library](https://core.telegram.org/tdlib)
* Telegram API - steps
  - [Getting Started](https://core.telegram.org/api#getting-started)
  - [Security](https://core.telegram.org/api#security)
  - [Optimization](https://core.telegram.org/api#optimization)
  - [API methods](https://core.telegram.org/api#api-methods)
* [using library in diff projects](https://github.com/tdlib/td/blob/master/example/README.md)
  * [python](https://github.com/alexander-akhmetov/python-telegram)
  * [java](https://github.com/tdlib/td/tree/master/example/java)
* [build native libraries](https://tdlib.github.io/td/build.html?language=Java)
* [functions](https://core.telegram.org/tdlib/docs/classtd_1_1td__api_1_1_function.html)

## possible ready to use applications
### Python
#### Telethon
* [usage example](https://github.com/cherkavi/python-utilities/tree/master/telegram)
* [git](https://github.com/LonamiWebs/Telethon)
* [doc](https://docs.telethon.dev/en/stable/)

#### python-telegram
* [usage example](https://github.com/cherkavi/python-utilities/blob/master/telegram/)
* https://github.com/alexander-akhmetov/python-telegram
* [python telegram doc](https://python-telegram.readthedocs.io/latest/tutorial.html)

