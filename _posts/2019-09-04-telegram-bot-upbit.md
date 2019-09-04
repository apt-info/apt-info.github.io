---
title: (python) 1시간마다 업비트 코인 가격을 Telegram으로 전송
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

1시간마다 Upbit의 bitcoin 가격을 가져와 텔레그램 봇으로 나에게 메시지를 전송하는 앱을 만들어보겠습니다.

# 1. 개발환경

- [python3](https://www.python.org/downloads/)
- [python-telegram-bot](https://python-telegram-bot.org)

# 2. 텔레그램 봇 만들기

업비트 가격을 전송할 텔레그램 봇을 [이 페이지](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/telegram-bot/)를 참고해 생성합니다.

# 3. 업비트 코인 가격 가져오기

코인 가격을 가져오는 상세한 방법은 [이 페이지](https://apt-info.github.io/%EC%95%94%ED%98%B8%ED%99%94%ED%8F%90/%EC%97%85%EB%B9%84%ED%8A%B8-%EC%BD%94%EC%9D%B8-%EA%B0%80%EA%B2%A9/)를 참고하면 됩니다.

# 4. 1시간마다 코인 가격을 가져와 텔레그램에 전송

2,3 번 페이지를 참고하여 응용 프로그램을 만듭니다.

```python
from telegram.ext import Updater
from accounts import accounts
from telegram.ext import Updater
from upbitpy import Upbitpy
import time
import datetime
import logging

INTERVAL_MIN = 1
CHAT_ID = accounts.TELEGRAM_MY_CHAT_ID
TELEGRAM_BOT_TOKEN = accounts.TELEGRAM_DEV_DEMO_BOT

def wait(min):
    now = datetime.datetime.now()
    remain_second = 60 - now.second
    remain_second += 60 * (min - (now.minute % min + 1))
    time.sleep(remain_second)


def main():
    upbit = Upbitpy()
    updater = Updater(TELEGRAM_BOT_TOKEN)

    while True:
        ticker = upbit.get_ticker(['KRW-BTC'])[0]
        price_str = format(int(ticker['trade_price']), ',')
        text = '({}) 비트코인 가격: {} 원'.format(datetime.datetime.now().strftime('%m/%d %H:%M:%S'), price_str)
        updater.bot.send_message(chat_id=CHAT_ID, text=text)
        wait(INTERVAL_MIN)


if __name__ == '__main__':
    logging.basicConfig(level=logging.INFO)
    main()
```

# 5. 실행 결과

테스트를 위해 INTERVAL_MIN 을 1로 설정하여 1분마다 메시지가 오게 했습니다.

INTERVAL_MIN 을 60으로 변경하면 매 시 정각마다 메시지를 보냅니다.

![screenshot](https://apt-info.github.io/images/2019-09-04-telegram-upbit/1.jpg)

# Code

전체 코드는 [https://github.com/apt-info/samples](https://github.com/apt-info/samples/blob/master/python/20190904-telegram-upbit.py) 에서 확인하실 수 있습니다.

