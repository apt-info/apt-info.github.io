---
title: (python) flask 1. 시작하기
categories:
- 개발
tags:
- python
- 파이썬
- flask
---

python flask 를 이용해 웹 서버를 만드는 방법을 알아보겠습니다.

django도 있지만, flask보다 복잡해 보입니다.

microservice에 적합하다는 flask를 이용하여 웹 서비스를 시작해보고, 간단한 요청을 처리하는 페이지를 작성해보겠습니다.

# 1. 설치

python3 에 최신 flask 릴리즈를 이용해보겠습니다.

```console
$ pip3 install flask
```

# 2. 웹 서비스 시작

flask 객체를 생성하고 root page 접근 시 호출될 function과 sub page 접근 요청에 대한 function을 만듭니다.

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def root():
    return 'welcome to flask!'


@app.route('/sub')
def sub():
    return 'sub page'
```

# 3. 진입 점 만들고 app 시작

```python
if __name__ == '__main__':
    app.debug = True
    app.run()
```

# 4. 동작 확인

저 코드만으로 웹 서비스가 동작합니다.

실제로 요청에 대한 처리가 잘 이뤄지는지 확인해보겠습니다.


# 4.1. 실행

```bash
$ python3 app.py

 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 272-160-151
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

# 4.2. 웹 브라우저로 접근

- http://127.0.0.1:5000
![screenshot](https://apt-info.github.io/images/2019-09-10-python-flask/1.png)

- http://127.0.0.1:5000/sub
![screenshot](https://apt-info.github.io/images/2019-09-10-python-flask/2.png)
두 페이지 모두 잘 처리 되었습니다.

다음 시간에는 좀더 복잡한 요청에 대한 처리를 다뤄보도록 하겠습니다.

# 5. 코드

전체 코드는 [https://github.com/apt-info/samples/](https://github.com/apt-info/samples/blob/master/python/flask/1.%20%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0/app.py) 에서 확인하실 수 있습니다.
