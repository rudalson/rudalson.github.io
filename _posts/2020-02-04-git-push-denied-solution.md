---
layout: post
title:  "git push에서 Permission denied 발생 해결"
categories: etc
tags: git
---

물려받은 PC, 포트제한도 많은 환경.

일하던 곳에서의 환경이고 역시나 편치는 않았다. 회사계정, 개인계정이 이것저것 뒤죽박죽 섞여있던 곳에서 git을 사용하는데 어느 순간 `https` 프로토콜로 `remote fetch`가 안되기 시작했다.

처음에는 또 포트가 막혔나 싶었다. 그래서 테더링으로 해봤지만 역시나 안되었다. 이 문제를 풀어야 겠다.

## 문제 상황
정확한 에러내용은 아래처럼 나왔다.
```
D:\repo> git push --set-upstream origin master
remote: Permission to 유저/프로젝트.git denied to 모르는유저.
fatal: unable to access 'https://github.com/유저/프로젝트.git/': The requested URL returned error: 403
```

## 윈도우즈 환경
일반 `bash` 환경이 아닌 윈도우즈 환경에서 이런건 어떻게 풀어야 하나 싶어 검색해보니 아래방식으로 해결했다. `윈도우즈10`에서 `자격 증명 관리자`를 실행해준다.


여기서 문제가 되는 계정을 `편집`에 들어가서 계정과 비번을 새로 입력해주면 된다.

