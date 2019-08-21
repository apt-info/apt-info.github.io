---
title: (Ubuntu) crontab editor 변경
categories:
- 개발
tags:
- ubuntu
- linux
- crontab
- editor
---

select-editor를 사용하여 crontab editor 변경할 수 있습니다.

```bash
$ select-editor

# 기본으로 nano가 선택되어있음, 익숙한 editor로 변경
Select an editor.  To change later, run 'select-editor'.
  1. /bin/ed
  2. /bin/nano        <---- easiest
  3. /usr/bin/code
  4. /usr/bin/vim.basic
  5. /usr/bin/vim.tiny

Choose 1-5 [2]:
```
