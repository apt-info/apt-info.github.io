---
title: (Ubuntu) journal log 제한 없애기
categories:
- 개발
tags:
- ubuntu
- linux
- journal
- limit
---

journal 로그 사용 시, 특정 프로세스에서 journal 로그를 너무 자주 출력하면 제한이 걸려 출력이 되지 않습니다.

너무 많이 로그를 출력하지 않도록 하는 것이 우선이지만, 개발 시에는 로그 제한이 불편할 수 있습니다.

이럴땐 로그 제한을 풀어주면 해결됩니다.

```bash
# journald 설정 변경
$ vi /etc/systemd/journald.conf

# RateLimitBurst=1000 부분을 아래처럼 변경
RateLimitBurst=0

# journal daemon 재시작
$ systemctl restart systemd-journald
```