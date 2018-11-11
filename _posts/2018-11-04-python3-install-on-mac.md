---
layout: post
title:  "Mac에서 python3 install"
categories: python
tags: python install mac
---
# Macbook 에서 python3 설치

mac에 기본적으로 python 2.7이 설치가 되어 있긴 하지만 python3을 사용하기 위해서는 별도의 설치가 필요하다.
몇몇 방법이 있겠지만 여기서는 brew를 사용한다.

먼저 brew 가 설치되어 있는지 확인해본다.
```
> ls /usr/local/bin/br*
/usr/local/bin/brew
```

## install by brew
```
brew install python3
```

그런 다음 python3  버전을 확인해 본다.
```
> python3
Python 3.7.0 (default, Oct  2 2018, 09:20:07) 
[Clang 10.0.0 (clang-1000.11.45.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

brew 로 설치하고 나면 pip3도 같이 설치가 된다.
```
> pip3 -V
pip 18.0 from /usr/local/lib/python3.7/site-packages/pip (python 3.7)
```

내친김에 장고까지 설치해보자
```
> pip3 install django
Collecting django
  Downloading https://files.pythonhosted.org/packages/d1/e5/2676be45ea49cfd09a663f289376b3888accd57ff06c953297bfdee1fb08/Django-2.1.3-py3-none-any.whl (7.3MB)
    100% |████████████████████████████████| 7.3MB 611kB/s 
Collecting pytz (from django)
  Downloading https://files.pythonhosted.org/packages/f8/0e/2365ddc010afb3d79147f1dd544e5ee24bf4ece58ab99b16fbb465ce6dc0/pytz-2018.7-py2.py3-none-any.whl (506kB)
    100% |████████████████████████████████| 512kB 731kB/s 
Installing collected packages: pytz, django
Successfully installed django-2.1.3 pytz-2018.7
```

설치된 장고의 버전을 확인
```
> python3
Python 3.7.0 (default, Oct  2 2018, 09:20:07) 
[Clang 10.0.0 (clang-1000.11.45.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 
>>> import django
>>> django.__version__
'2.1.3'
```
