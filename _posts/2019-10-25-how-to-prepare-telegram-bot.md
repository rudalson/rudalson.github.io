---
layout: post
title:  "Python으로 텔레그램 봇을 만들기 위해 뜸들이는 글"
categories: python
tags: python telegram bot
---
텔레그램 봇 만들기 위해 몇몇 글들을 봤지만 생각보다 방향이 잘 안잡혔다. 그래서 지금 현재 가장 잘 맞게 제안된 글을 보기 위해 공식사이트 글을 살펴보았다.

## [Bots: An introduction for developers](https://core.telegram.org/bots)

뭐니뭐니해도 이글부터 봤던 것이 가장 큰 도움이 되었다. 봇에 대한 개념 및 텔레그램 봇에서는 어떤 것들을 할 수 있는지에 대한 설명 및 기존에 만들어진 봇들에 실행해볼 수 있는 링크등을 제공해주면서 어디까지 만들 수 있나를 파악할 수 있었다. 

봇으로 게임도 만들 수 있다니...

봇개발에 대해서는 이글에서 방향을 잡으면 될 것 같고 기본 `튜터리얼 코드`를 살펴 보고 싶으면 [python-telegram-bot](https://github.com/python-telegram-bot/python-telegram-bot) 코드부터 시작하면 된다.

여기서 `example` [디렉토리](https://github.com/python-telegram-bot/python-telegram-bot/blob/master/examples/README.md)를 가보면 11가지의 기본 샘플 코드를 볼 수 있다. 이 모두는 각각 `main()`에서 token 만 `@BotFather`에서 구한 자신의 토큰으로 입력해서 실행해보면 된다.

여기보면 간단히 `에코 실행`, `타이머로 주기별로 실행`, `인라인키보드 사용`등에 대한 예제가 나온다. 심지어 `지불`에 대한 봇 예제도 있는데 한국에서는 해보기 힘들 것 같다.


## [Extensions – Your first Bot](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Extensions-%E2%80%93-Your-first-Bot)

나처럼 뒤늦게 `텔레그램봇` 개발에 대해 파악하고 있는 경우 과거 `bare-metal` API 라고 마치 날것의 api가 제공 되던 시절에서 이젠 라이브러리화가 잘된 것을 활용하는 것으로 넘어온것으로 추정되는 분위기도 느낄 수 있었다.

그래서 텔레그램봇 만들기를 검색해서 너무 이전것을 보는 것보단 공식문서 보는 것이 더 나은 경우인것 같다. 이쪽도 변화가 빠른가 보다.


## [Code snippets](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Code-snippets)
조금 더 자세한 코드에 대한 내용은 snippets를 보고 함수들을 장착하면 될 것 같다.

이제 다음엔 진짜 만들어야 될텐데.........언제까지 파악만 할 것인가

