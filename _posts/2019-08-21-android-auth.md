---
title: (Android) adb - auth 방법
categories:
- 개발
tags:
- Android
- 안드로이드
- adb
- auth
- route
---

adb 에서 route등의 명령을 하기 위해서는 auth 작업을 먼저 해주셔야 합니다.

```bash
# token 값 확인
$ cat ~/.emulator_console_auth_token

# auth
$ adb shell
$ auth ${위에서 얻은 token값 입력}
```