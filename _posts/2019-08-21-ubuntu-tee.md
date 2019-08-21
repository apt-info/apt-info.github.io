---
title: (Ubuntu) 명령어 결과를 출력하며 동시에 파일에 쓰기
categories:
- 개발
tags:
- ubuntu
- linux
- file
- tee
---

command에 대한 결과를 파일에 쓰면서 동시에 화면에 출력하여 확인하고 싶을때가 있습니다.

```bash
$ {command} 2>&1 | tee {file명}
$ ls -al 2>&1 | tee ./ls_result.txt
```