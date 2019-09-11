---
title: (python) flask 2. Get 요청 처리
categories:
- 개발
tags:
- python
- 파이썬
- flask
- get
- request
---

지난 시간에 이어 Flask로 Get 요청을 처리하는 방법에 대해 알아보겠습니다.

---

지금까지 공부한 것

* [(python) flask 1. 시작하기](https://github.com/apt-info/samples/blob/master/python/flask/1_setup_/app.py)

---

client로부터 get parameter를 전달받고, 전달받은 parameter를 출력하는 예제를 만들어 볼 것입니다.

get parameter는 주소로 넘기는 parameter를 뜻합니다.

# 1. app 생성

flask app을 생성하고, root page 요청에 대한 handler를 생성합니다.

```python
# _*_ coding: utf-8 _*_

from flask import Flask

app = Flask(__name__)

@app.route('/')
def root():
    pass
```

# 2. request 에 대한 parameter 처리

request parameter는 request.args 변수에 ImmutableMultiDic 형태로 넘어옵니다.

parameter key가 정해져있다면 request.args.get({key}) 형태로 받아올 수 있습니다.

이번엔 아무 key/value나 받아볼 것이기 때문에 ImmutableMultiDic의 to_dict() 를 통해 dictionary 형태로 변환하여 출력해보겠습니다.

flask 모듈에서 request를 import하여 parameter 를 얻는 코드를 추가합니다.

```python
from flask import Flask, request # request import 추가

@app.route('/')
def root():
    parameter_dict = request.args.to_dict()
    if len(parameter_dict) == 0:
        return 'No parameter'

    parameters = ''
    for key in parameter_dict.keys():
        parameters += 'key: {}, value: {}\n'.format(key, request.args[key])
    return parameters
```

# 3. app 실행

### 3.1. parameter를 전달하지 않은 경우

![screenshot](https://apt-info.github.io/images/2019-09-12-python-flask2-get/1.png)

### 3.2. 1개의 parameter를 전달

![screenshot](https://apt-info.github.io/images/2019-09-12-python-flask2-get/2.png)

### 3.2. 2개의 parameter를 전달

![screenshot](https://apt-info.github.io/images/2019-09-12-python-flask2-get/3.png)

### 3.2. 한글 parameter 전달

![screenshot](https://apt-info.github.io/images/2019-09-12-python-flask2-get/4.png)

# 5. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/blob/master/python/flask/2_get_request/app.py) 에서 확인하실 수 있습니다.
