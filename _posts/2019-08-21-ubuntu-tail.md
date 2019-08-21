---
title: (Ubuntu) 특정 파일 변경 내용 계속 확인하기
categories:
- 개발
tags:
- ubuntu
- linux
- file
- watch
- tail
---

계속 변경되고 있는 파일 내용을 화면에 출력하여 확인하고 싶을 때 tail 명령어를 사용하면 됩니다.

```bash
$ tail -f {file이름}
$ tail -f ./result.txt
```