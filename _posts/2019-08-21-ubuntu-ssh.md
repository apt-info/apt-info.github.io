---
title: (Ubuntu) SSH 설정
categories:
- 개발
tags:
- ubuntu
- linux
- ssh
---

우분투 설치 후 SSH 설정 방법

# 0. 준비 사항

- Ubuntu 18.04 (다른버전도 상관 없음)

# 1. ssh server 설치

```bash
$ sudo apt-get update
$ sudo apt-get install openssh-server
```

# 2. rsa key 생성

```bash
$ ssh-keygen
```

# 3. remote 접속 시 비밀번호 입력 생략하기

- remote PC의 ~/.ssh/authorized_keys에 내 public key 추가

```bash
# 내 PC
$ cat ~/.ssh/id_rsa.pub

# remote PC
$ echo "{위에서 cat으로 출력된 내용}" >> ~/.ssh/authorized_keys
```

