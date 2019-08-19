---
layout: post
title:  "Python3 venv 사용하기"
categories: python
tags: python venv
---
python3.3 이후부터 추가된 [venv](https://docs.python.org/ko/3/library/venv.html#module-venv)는 python의 가상 환경이다. 이를 사용하기 위해서는

# venv 생성
```bash
$ python3 -m venv venv
```
이를 확인해보려면 아래처럼 가능하다.

## venv 사용
```bash
$ pip3 list
Package           Version
----------------- -------
astroid           2.2.5  
isort             4.3.21 
lazy-object-proxy 1.4.1  
mccabe            0.6.1  
pip               19.1.1 
pylint            2.3.1  
setuptools        41.0.1 
six               1.12.0 
typed-ast         1.4.0  
wheel             0.33.4 
wrapt             1.11.2

$ source ./venv/bin/activate
(venv) $ pip3 list
Package    Version
---------- -------
pip        19.0.3 
setuptools 40.8.0

(venv) $ which python
/Users/rudalson/repo/some-project/venv/bin/python
```

## venv 사용 해제
```bash
(venv) $ deactivate
```

# global venv 생성
## global 환경과 같은 패키지 생성
```bash
$ python3 -m venv venv --system-site-packages
```

## global 환경과 다른 패키지 확인
```bash
$ pip list --local
```

# requirements
설치 패키지 requirements파일 생성
```bash
$ pip freeze > requirements.txt
```

requirements 파일로 필요한 패키지 설치
```bash
$ pip install -r requirements.txt
```