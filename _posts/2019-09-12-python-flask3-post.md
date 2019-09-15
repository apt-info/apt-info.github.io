---
title: (python) flask 3. Post 요청 처리
categories:
- 개발
tags:
- python
- 파이썬
- flask
- post
- request
---

지난 시간에 이어 Flask로 Post 요청을 처리하는 방법에 대해 알아보겠습니다.

---

지금까지 공부한 것

* [(python) flask 1. 시작하기](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/python-flask/)
* [(python) flask 2. Get 요청 처리](https://apt-info.github.io/%EA%B0%9C%EB%B0%9C/python-flask2-get/)

---

client로부터 post parameter를 전달받고, 전달받은 parameter를 출력하는 예제를 만들어 볼 것입니다.

post parameter는 body로부터 넘어오는 paramter입니다.

# 1. app 생성

flask app을 생성하고, root page 요청에 대한 handler를 생성합니다.

```python
# _*_ coding: utf-8 _*_

from flask import Flask

app = Flask(__name__)

@app.route('/')
def root():
    return 'welcome to flask'
```

# 2. post 요청을 날릴 page 생성

특정 page(send_post)에 접속하면, post 요청으 처리하는 page(handle_post)에 post 요청을 날려 테스트 해보겠습니다.

### 2.1. send_post 요청 처리

send_post 접속 시 requests모듈을 이용하여 handle_post page에 post요청을 합니다.

handle_post로부터 전달받은 결과를 return 합니다.

```python
import requests

@app.route('/send_post', methods=['GET'])
def send_post():
    params = {
        "param1": "test1",
        "param2": 123,
        "param3": "한글"
    }
    res = requests.post("http://127.0.0.1:5000/handle_post", data=json.dumps(params))
    return res.text
```

### 2.2. handle_post 요청 처리

post 요청을 처리할 handle_post 도 만듭니다.

post 요청을 처리하기 위해 app.route 인자로 methods=['POST']를 넣어야 합니다.

전달받은 data를 json.loads를 이용해 dictionary 형태로 변환하여 파싱하였습니다.

```python
@app.route('/handle_post', methods=['POST'])
def handle_post():
    params = json.loads(request.get_data(), encoding='utf-8')
    if len(params) == 0:
        return 'No parameter'

    params_str = ''
    for key in params.keys():
        params_str += 'key: {}, value: {}<br>'.format(key, params[key])
    return params_str
```

# 3. app 실행

전달한 parameter들이 잘 파싱되는 것을 볼 수 있습니다.

![screenshot](https://apt-info.github.io/images/2019-09-12-python-flask3-post/1.png)

# 4. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/blob/master/python/flask/3_post_request/app.py) 에서 확인하실 수 있습니다.
