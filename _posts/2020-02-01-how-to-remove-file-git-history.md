---
layout: post
title:  "Git History에서 파일 영구적으로 지우기"
categories: etc
tags: git
---

`git`은 파일의 이력을 모두 관리하며 최초 이후부터는 증감분을 관리한다. 따라서 파일을 커밋한 이후 나중에 이것을 지우는 delete 를 수행한다 해도 이 자체가 하나의 이력이므로 소스이력상에서 파일이 지울 수는 없다.


라고 알고 있었다. 그런데 다른 사람의 팀프로젝트에서 지워야할 필요가 생겼다. 초기 `.gitignore`에 포함 못시킨 디렉토리로 인해서 이후 지속적으로 용량의 고통을 받아오다 결국 프로젝트를 삭제하고 다시 생성하려 했었다.

그런데 이것은 모든 `commit history`가 사라질텐데...... 라며 망설이고 있었다.

# 모든 이력에서 특정 파일 혹은 디렉토리 삭제

## filter-branch
`filter-branch`는 브랜치내에서 특정이력을 다시 쓰는 history rewrite 가 가능하다. 그래서 파일제거나 rewrite 같은 것이 가능하다. 하지만 이런것들은 조심해서 사용되어야 한다. 위의 경우처럼 [초기 커밋에서 포함된 파일들을 지울 때](https://help.github.com/en/github/managing-large-files/removing-files-from-a-repositorys-history) 사용하면 될 것 같다.

그런데 검색해보니 사용하는 방법이 제각각이었다. 그중에서 내가 해보고 효과를 본것은 [Git가지고 놀기(3) - 파일 영원히 지우기](https://wani.kr/posts/2015/02/02/git-3-remove-file-forever/)의 방법으로 하였을 때 효과가 있었다.

```shell
$ git filter-branch --tree-filter 'rm -rf ./node_modules' HEAD
WARNING: git-filter-branch has a glut of gotchas generating mangled history
         rewrites.  Hit Ctrl-C before proceeding to abort, then use an
         alternative filtering tool such as 'git filter-repo'
         (https://github.com/newren/git-filter-repo/) instead.  See the
         filter-branch manual page for more details; to squelch this warning,
         set FILTER_BRANCH_SQUELCH_WARNING=1.
Proceeding with filter-branch...
Rewrite dc1f1ce75d65ddca1a00dfc7936a9c6c6f8635ba (1/30) (0 seconds passed, remaiRewrite b84f79c97d4cfe5eaea489780905004fb975e570 (2/30) (101 seconds passed, remRewrite 36b524992fe444bcb9ae4e4371863cebbf00cea1 (3/30) (202 seconds passed, remRewrite 702be0956a8272dace9375b87c6fbe282c215dec (4/30) (302 seconds passed, remRewrite eb92a21aaae71c085625215208b10ab64da3a6eb (5/30) (303 seconds passed, remRewrite e24bb84769fa4e1ee35fe1136100a7f25fc7bd75 (6/30) (303 seconds passed, remRewrite eb01c02df740889faaa8dd93ae43211c0c75082b (7/30) (304 seconds passed, remRewrite ccbceef34a24022d1d984b1c4551685d1bb90598 (8/30) (305 seconds passed, remRewrite 53ebb33ed4ff2d5d38b2f017777d6b663fa5949b (9/30) (305 seconds passed, remRewrite 48b0a8c08435df990201045c73143fc582edd6c6 (10/30) (306 seconds passed, reRewrite 7569ca66c979f65b30645889f8629db0cca4a132 (11/30) (307 seconds passed, reRewrite 2d9d01cced5bac42d89dd38a9d495eddea40dfe5 (12/30) (307 seconds passed, reRewrite 44ed319c656a7dedffe17093fdd3af2213770896 (13/30) (308 seconds passed, reRewrite 34e23214bd16229451b17bb5dd543fc57a7b57fa (14/30) (309 seconds passed, reRewrite c0a8cb173eaee7b568e994165996fee637f500b5 (15/30) (309 seconds passed, reRewrite 31d0a691867aca4ce1abce41b5a14f31c0486c7c (16/30) (310 seconds passed, reRewrite 27eb484ade2efb8685e953fc467cf65905998739 (17/30) (311 seconds passed, reRewrite 4ea7a431b4aa1cb05ac88f622f3d73b8f5b8f0c2 (18/30) (311 seconds passed, reRewrite ff791ac6fa92ed42f6bb9d83857354d7c180c115 (19/30) (312 seconds passed, reRewrite 1e3678175318dec256895fd837e63fc3f672c153 (20/30) (312 seconds passed, reRewrite 7d26651ba9c832e61d937ff292aab85c713eabcb (21/30) (313 seconds passed, reRewrite dc8391645f3f1929846c833a6d6b2be1e4e723d9 (22/30) (314 seconds passed, reRewrite 507a61a33a4d1a9420d0131bc0d45ac02ebe64c9 (23/30) (316 seconds passed, reRewrite d8585851b034af7f39bc2f1791a568d38e0f868d (24/30) (318 seconds passed, reRewrite 09cfdece594fbd29ceaed00e21abc28c44ca9b01 (25/30) (319 seconds passed, reRewrite f9a6fad83a5fd00b0c74e5f9f1481ddd528d0e6b (26/30) (319 seconds passed, reRewrite 7c697b2618f8c27b4f5a5f3dc3ec6889b1edb17c (27/30) (320 seconds passed, reRewrite 70b4c133f202922581fa72639f3b3a78d7708297 (28/30) (321 seconds passed, reRewrite 80d4c415680bf3428a4cb98892bef237edea1006 (29/30) (321 seconds passed, reRewrite 35d1ea7be6d7807b9994c8971ef03bf433bf3187 (30/30) (322 seconds passed, remaining 0 predicted)
Ref 'refs/heads/master' was rewritten
```
그리고 `push` 해주자.

## master branch unprotected

```
$ git push origin master --force
Enumerating objects: 663, done.
Counting objects: 100% (663/663), done.
Delta compression using up to 12 threads
Compressing objects: 100% (416/416), done.
Writing objects: 100% (663/663), 317.17 MiB | 5.70 MiB/s, done.
Total 663 (delta 194), reused 628 (delta 192)
remote: Resolving deltas: 100% (194/194), done.
remote: GitLab: You are not allowed to force push code to a protected branch on this project.
To https://프로젝트 경로.git
 ! [remote rejected]   master -> master (pre-receive hook declined)
error: failed to push some refs to 'https://프로젝트 경로.git'
```

이것을 처음 올렸을 때는 위와 같은 에러가 발생하였는데 이것은 gitlab내 master 브랜치 설정을 변경하면 해결이 가능하다.

[Settings] - [Repository] - [Protected Branches] 에서 master 부분에 `UnProtected`를 눌러주면 된다.

이후 다시 하면 성공적으로 지운 것을 확인 할 수 있다.