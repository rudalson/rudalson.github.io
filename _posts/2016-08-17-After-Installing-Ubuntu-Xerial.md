---
layout: post
title:  "Ubuntu 16.04 Xerial 설치 후 해줘야 할 일"
date:   2016-08-17 16:48:28 +0900
categories: system
tags: linux os 한글 ubuntu
---
# 한글 설치
* sudo apt purge ibus
* sudo apt install fcitx-hangul
* sudo apt install fcitx
* sudo apt install language-selector-gnome

language-selector-gnome 가서, 입력기를 fcitx 로 변경

윈도우키 누르시고 -> fcitx 환경설정 -> Global Config -> Trigger input method 에서 Hangul 을 선택하시고 원>하는 키 조합을 누르세요


# google chrome 설치
웹에서 다운받아서 설치하지 말고 웬만하면 아래의 방식으로 설치할 것을 권장

{% highlight bash %}
$ sudo apt install chromium-browser
{% endhighlight %}

# Package Manager 버그 패치
보통은 apt 로 패치가 끝나지만 이 버그는 이런 패키지 매니저 자체의 버그 수정 패치이다. 따라서 별도로 설치해줘야 한다.

[libappstream3 0.9.4-1ubuntu1 (amd64 binary) in ubuntu xenial](https://launchpad.net/ubuntu/xenial/amd64/libappstream3/0.9.4-1ubuntu1)
