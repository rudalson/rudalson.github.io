---
layout: post
title:  "Ubuntu 16.04 Xerial 설치 후 해줘야 할 일"
date:   2016-08-17 16:48:28 +0900
categories: linux
tags: linux os 한글
---
# 한글 설치
* sudo apt purge ibus
* sudo apt install fcitx-hangul
* sudo apt install fcitx
* sudo apt install language-selector-gnome

language-selector-gnome 가셔서, 입력기를 fcitx 로 변경

윈도우키 누르시고 -> fcitx 환경설정 -> Global Config -> Trigger input method 에서 Hangul 을 선택하시고 원>하는 키 조합을 누르세요


# google chrome 설치
sudo apt install chromium-browser

