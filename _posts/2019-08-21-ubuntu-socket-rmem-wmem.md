---
title: (Ubuntu) socket read/writer buffer 사이즈 설정
categories:
- 개발
tags:
- ubuntu
- linux
- socket
- buffer
- rmem
- wmem
---

Linux에서 socket 통신 시 packet drop이 발생하는 경우 **최후의 방법**으로 socket의 read/write buffer를 튜닝하여 drop을 회피할 수 있습니다.

```bash
# buffer size 확인
# rmem: read buffer, wmem: write buffer
$ sysctl -a | grep "net.core.rmem\|net.core.wmem"
net.core.rmem_default = 212992
net.core.rmem_max = 212992
net.core.wmem_default = 212992
net.core.wmem_max = 212992

# buffer size 변경
$ sudo sysctl -w net.core.rmem_default={변경값}
$ sudo sysctl -w net.core.rmem_max={변경값}
$ sudo sysctl -w net.core.wmem_default={변경값}
$ sudo sysctl -w net.core.wmem_max={변경값}
```