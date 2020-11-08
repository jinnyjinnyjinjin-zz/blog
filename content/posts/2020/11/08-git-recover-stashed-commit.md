---
title: "[Git] drop 된 stash 복구하기"
date: 2020-11-08T15:26:07+09:00
tags: [Git]
draft: false
---
개발을 하면서 `stash` 를 잘 활용하는데 실수로 저장해둔 `stash` 를 삭제 했던 적이 있었다.
순간 당황스러웠지만 생각했던 것보다 간단하게 아래의 과정으로 복구시킬 수 있었다.

## 1. Stash 커밋 찾기
```shell
git log --graph --oneline --decorate ( git fsck --no-reflog | awk '/dangling commit/ {print $3}' )
```

위 명령어는 모든 stash 커밋들과 더 이상 참조하지 않는 모든 브랜치를 포함하여, 현재까지 생성한 커밋과 잃어버린 커밋까지 모든 커밋 로그를 출력한다.

## 2. Commit Hash 복구

출력된 로그 중에 `WIP on` 으로 시작하는 커밋 Hash 를 찾아 아래 명령어를 실행하면 잃어버린 커밋을 되찾을 수 있었다.
```shell
git stash apply <YOUR_WIP_COMMIT_HASH>
```
