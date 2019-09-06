---
title: (python) telegram bot 3. command 받아 처리하기
categories:
- 개발
tags:
- telegram
- telegrambot
- 텔레그램
- 텔레그램봇
- Comand
- python
- 파이썬
---

텔레그램 봇을 만들어 보고, 업비트의 코인 가격을 가져와 메시지로 전송하는 방법을 알아보았습니다.

- [(python) telegram bot 1. 봇 만들고 메시지 보내보기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/telegram-bot)
- [(python) telegram bot 2. 업비트 코인 가격을 메시지로 전송](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/telegram-bot-upbit/)

이번에는 봇이 일방적으로 메시지를 보내는 것이 아닌, 봇에게 Command/arg를 넘겨 처리하는 예제를 알아보겠습니다.

# 1. 개발환경

- [python3](https://www.python.org/downloads/)
- [python-telegram-bot](https://python-telegram-bot.org)

# 2. updater에 CommandHandler 추가

CommandHandler는 '이 command를 요청하면 이 function을 실행해 줘'를 처리합니다.

```python
def start(bot: Bot, update: Update):
    logging.info('>>> start')


def stop(bot, update: Update):
    logging.info('>>> stop')


TELEGRAM_BOT_TOKEN = accounts.TELEGRAM_DEV_DEMO_BOT # 생성한 bot token으로 변경해주세요
updater = Updater(TELEGRAM_BOT_TOKEN)
updater.dispatcher.add_handler(CommandHandler('start', start)) # start command를 요청하면 start function 실행
updater.dispatcher.add_handler(CommandHandler('stop', stop))
updater.start_polling() # command 입력받을때 까지 대기 시작
updater.idle()
```

# 3. command + arg 처리

command뿐만 아니라 사용자가 인자를 전달하고 싶은 경우가 있습니다.

예) 'search' command에 keyword전달

CommandHandler에 pass_args를 True로 주시면 args를 전달할 수 있습니다.

args 는 list로 전달됩니다.

```python
def search(bot, update: Update, args):
    logging.info('>>> search')

updater.dispatcher.add_handler(CommandHandler('send', send, pass_args=True))
```

# 4. command에 대한 응답으로 메시지를 보내기

callback으로 전달된 bot, update를 가지고 응답 메시지를 보낼 수 있습니다.

```python
def start(bot: Bot, update: Update):
    logging.info('>>> start')
    bot.send_message(chat_id=update.message.chat_id, text='welcome...')


def stop(bot, update: Update):
    logging.info('>>> start')
    bot.send_message(chat_id=update.message.chat_id, text='bye...')


def search(bot, update: Update, args):
    logging.info('>>> search')
    keywords = 'keywords:'
    for arg in args:
        keywords += '\n - {}'.format(arg)
    bot.send_message(chat_id=update.message.chat_id, text=keywords)
```

# 5. 실행 결과

지금 핫한 인물인 조국과 윤석렬을 검색해보았습니다.

![screenshot](https://apt-info.github.io/images/2019-09-07-telegram-command/1.jpg)

# 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/blob/master/python/telegram3-command.py) 에서 확인하실 수 있습니다.