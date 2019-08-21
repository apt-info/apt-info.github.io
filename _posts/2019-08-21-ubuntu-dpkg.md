---
title: (Ubuntu) dpkg 명령어 모음
categories:
- 개발
tags:
- ubuntu
- linux
- dpkg
---

dpkg (debian package) 관련 유용한 명령어들입니다.

# 1. 설치된 파일이 어느 package로부터 설치된 것인지 확인

```bash
$ dpkg --search {file}
```

# 2. package update 실패 시

```bash
$ dpkg --purge {package name}
```

# 3. 설치된 전체 package list 확인

```bash
$ dpkg -l
```

# 4. 설치된 package 검색

```bash
$ dpkg -l {keyword}
```