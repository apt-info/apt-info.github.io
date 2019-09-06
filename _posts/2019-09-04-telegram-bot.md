---
title: (python) telegram bot 1. 봇 만들고 메시지 보내보기
categories:
- 개발
tags:
- telegram
- telegrambot
- 텔레그램
- 텔레그램봇
- python
- 파이썬
---

텔레그램 봇을 만들고, python 에서 봇을 통해 나에게 메시지를 보내는 방법을 알아보겠습니다.

이 예제를 응용하여 주기적으로 나에게 정보 메시지를 전달하거나 특정 이벤트 알림을 주는 프로그램을 만들 수 있습니다.

# 1. 개발 환경 구성

- [python3](https://www.python.org/downloads/)
- [python-telegram-bot](https://python-telegram-bot.org)

```bash
# https://github.com/python-telegram-bot/python-telegram-bot#installing
$ pip install python-telegram-bot --upgrade
```

# 2. bot 생성

Telegram에 접속하여 Bot을 생성합니다. (당연히 Telegram에 가입되어 있어야 합니다.)

@BotFather 를 검색합니다.

![screenshot](https://apt-info.github.io/images/2019-09-04-telegram-bot/1.jpg)

BotFather와 대화를 시작합니다.

BotFather는 봇과 관련된 여러가지 Command를 지원하는데, 지금은 Bot을 생성하기위해 /newbot을 대화창에 입력합니다. (물론 목록에서 선택해도 됩니다.)

![screenshot](https://apt-info.github.io/images/2019-09-04-telegram-bot/2.jpg)

/newbot을 입력하면 봇 이름을 입력하라는 메시지가 나옵니다. 저는 테스트봇 이라고 입력하였습니다.

![screenshot](https://apt-info.github.io/images/2019-09-04-telegram-bot/3.jpg)

봇 이름을 입력하면 Bot의 Username을 입력하라고 나옵니다.

Bot의 username은 'bot'으로 끝나야하고, 다른사람이 이미 만든 봇과 중복이 허용되지 않습니다.

저는 apt_info_test_bot 이라고 입력하였습니다.

![screenshot](https://apt-info.github.io/images/2019-09-04-telegram-bot/4.jpg)

봇이 생성되면 위와같이 봇 정보 메시지가 출력됩니다.

빨간색 네모친 부분이 실제 코드에서 사용하게 될 token입니다. 잘 저장해놓습니다.

# 3. python에서 봇으로 나에게 메시지 날려보기

## 3.1. Chat id 얻기

봇으로 메세지를 날리기 위해서는 먼저 chat_id 를 알아야 합니다.

chat_id는 대화방마다 부여된 고유 식별 id라고 생각하시면 됩니다.

먼저 위에서 생성한 bot을 검색하여 대화방을 생성합니다.

![screenshot](https://apt-info.github.io/images/2019-09-04-telegram-bot/5.jpg)

아무 메시지나 보내봅니다.

![screenshot](https://apt-info.github.io/images/2019-09-04-telegram-bot/6.jpg)

당연히 응답이 없습니다.

웹 브라우저를 열고 https://api.telegram.org/bot{위에서 얻은 Bot Token}/getUpdates 로 접속합니다.

저의 경우는 https://api.telegram.org/bot924028002:AAG4LznJkjaNNNwCFFFMO9iNDA2RhQ6-Jko/getUpdates 입니다.

오타없이 입력하였다면, {"ok":true..로 시작되는 메시지 정보가 출력됩니다. (여러개를 받았다면 리스트로 출력)

저희가 필요한건 "chat":{"id":xxxx} 값입니다. xxxx값을 적어둡니다.

## 3.2. 봇으로 메시지 전송

Bot token과 Chat id를 얻었으니 코드를 생성합니다.

```python
from telegram.ext import Updater

updater = Updater('924028002:AAG4LznJkjaNNNwCFFFMO9iNDA2RhQ6-Jko')
updater.bot.send_message(chat_id={위에서 얻은 xxxx값}, text='Hello telegram bot!')
```

![screenshot](https://apt-info.github.io/images/2019-09-04-telegram-bot/7.jpg)

불과 3줄 입력으로 봇으로부터 메시지가 도착하였습니다.

# Code

전체 코드는 [https://github.com/apt-info/samples](https://github.com/apt-info/samples/blob/master/python/telegram1.py) 에서 확인하실 수 있습니다.


```info
테스트로 생성한 Bot은 바로 제거하여 유효하지 않습니다
```