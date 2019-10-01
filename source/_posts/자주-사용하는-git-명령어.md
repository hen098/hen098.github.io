---
title: 자주 사용하는 git 명령어
tags: ["git"]
---

종종 검색해서 확인하던 git 명령어 정리

##### 원격 저장소 확인

``` bash bash
$ git remote -v
origin  https://github.com/hen098/hen098.github.io.git (fetch)
origin  https://github.com/hen098/hen098.github.io.git (push)
```

##### 원격 저장소 변경

```bash bash
$ git remote set-url origin https://github.com/~
```

##### 브랜치 확인

```bash bash 
$ git branch    (로컬 브랜치 확인)
$ git branch -r (리모트 브랜치 확인)
```

##### 방금 수행한 것 취소

```bash bash
$ git reset HEAD^
```
