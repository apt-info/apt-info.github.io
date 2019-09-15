---
title: (python) flask 4. 코인 차트 그리기
categories:
- 개발
tags:
- python
- 파이썬
- flask
- coin
- chart
- chart.js
---

이전 시간에 flask를 시작하는 방법과, get/post request를 처리하는 방법을 알아보았습니다.

---

지금까지 공부한 것

* [(python) flask 1. 시작하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/python-flask/)
* [(python) flask 2. Get 요청 처리](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/python-flask2-get/)
* [(python) flask 3. Post 요청 처리](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/python-flask3-post//)

---

이번엔 응용으로, market을 요청받아 해당 market에 대한 가격 변화 chart를 보여주는 예제를 만들어 보겠습니다.

# 1. chart.html 파일 작성

먼저, 차트를 그리는 html을 작성해보겠습니다.

나중에 요청받은 코인의 가격 리스트를 받아와서 데이터를 채워 html을 보여줄 것입니다.

web에서 동적으로 chart를 생성하는 방법은 여러가지가 있습니다.

여기서는 chart.js를 이용합니다.

**참고할만한 페이지**: [Jekyll 블로그 - line chart 그리기](https://apt-info.github.io/it%EC%9D%BC%EB%B0%98/line-chart/)

templates/chart.html

```html
<!DOCTYPE html>
<html>

<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.7.2/Chart.min.js"></script>

<div style="width:30%;">
    <canvas id="canvas"></canvas>
</div>

<script>
new Chart(document.getElementById("canvas"), {
    type: 'line',
    data: {
        labels: {{ xlabels | tojson }},
        datasets: [{
            label: '{{ label }}',
            data: {{ dataset | tojson }},
            borderColor: "rgba(247, 16, 74, 1)",
            fill: false,
            lineTension: 0
        }]
    },
    options: {
        responsive: true,
        title: {
            display: true,
            text: '{{ label }} 가격 변화'
        },
        tooltips: {
            mode: 'index',
            intersect: false,
        },
        hover: {
            mode: 'nearest',
            intersect: true
        },
        scales: {
            xAxes: [{
                display: true,
                scaleLabel: {
                    display: true,
                }
            }],
            yAxes: [{
                display: true,
                ticks: {
                },
                scaleLabel: {
                    display: true,
                }
            }]
        }
    }
});

</script>
</html>
```

'{{ }}' 로 작성된 부분은 나중에 flask에서 전달할 parameter 입니다.
- **label** 에는 market 이름을 넣을 예정입니다.
- **xlabels** 에는 빈 리스트를 예정입니다.
- **dataset** 에는 실제 가격 변화 리스트를 넣을 예정입니다.

# 2. upbit util 클래스 작성

app.py에서 Upbitpy를 바로 import하여 사용할 수도 있지만, 코드를 분리하였습니다.

특정 마켓의 60분봉을 10개 가져오는 util성 클래스를 생성합니다.

upbit.py

```python
from upbitpy import Upbitpy

class Upbit:
    def __init__(self):
        self.__upbit = Upbitpy()
        self.__krw_markets = self.__get_krw_markets()


    def __get_krw_markets(self):
        krw_markets = dict()
        all_markets = self.__upbit.get_market_all()
        for market in all_markets:
            if market['market'].startswith('KRW-'):
                krw_markets[market['market']] = market
        return krw_markets


    def get_hour_candles(self, market):
        if market not in self.__krw_markets.keys():
            return None
        candles = self.__upbit.get_minutes_candles(60, market, count=10)
        print(candles)
        return candles
```

# 3. app.py 작성

이제 get request를 처리하여 chart html을 로직을 작성합니다.

app.py

```python
# _*_ coding: utf-8 _*_

import logging
from upbit import Upbit
from flask import Flask, request, render_template


app = Flask(__name__)
upbit = Upbit()
upbit.get_hour_candles('KRW-BTC')


@app.route('/')
def root():
    market = request.args.get('market')
    if market is None or market == '':
        return 'No market parameter'

    candles = upbit.get_hour_candles(market)
    if candles is None:
        return 'invalid market: {}'.format(market)

    label = market
    xlabels = []
    dataset = []
    i = 0
    for candle in candles:
        xlabels.append('')
        dataset.append(candle['trade_price'])
        i += 1
    return render_template('chart.html', **locals())


def main():
    app.debug = True
    app.run()


if __name__ == '__main__':
    main()
```

- xlabels 를 빈 string array를 넣음으로써 xlabel을 숨기는 효과를 낼 수 있습니다.
- dataset 에는 trade_price (close_price)를 넣었습니다.
- render_template에 **local()을 넘기면 local변수를 그대로 셋팅하는 효과를 낼 수 있습니다. [참고링크](https://stackoverflow.com/questions/12096522/render-template-with-multiple-variables)

# 4. 실행 화면

## 4.1. http://127.0.0.1:5000/?market=KRW-BTC

![screenshot](https://apt-info.github.io/images/2019-09-16-python-flask4-chart/1.png)

## 4.2. http://127.0.0.1:5000/?market=KRW-LTC

![screenshot](https://apt-info.github.io/images/2019-09-16-python-flask4-chart/2.png)

## 4.3. http://127.0.0.1:5000/?market=KRW-EOS

![screenshot](https://apt-info.github.io/images/2019-09-16-python-flask4-chart/3.png)

## 4.4. http://127.0.0.1:5000/?market=KRW-ABC

예외처리 1 - invalid market

![screenshot](https://apt-info.github.io/images/2019-09-16-python-flask4-chart/4.png)

## 4.5. http://127.0.0.1:5000

얘외처리 2 - invalid parameter

![screenshot](https://apt-info.github.io/images/2019-09-16-python-flask4-chart/5.png)


# 5. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/tree/master/python/flask/4_coin_chart) 에서 확인하실 수 있습니다.
