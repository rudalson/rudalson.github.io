---
layout: post
title:  "InterlockedXXXXX() 함수의 인자는 왜 volatile 일까?"
categories: development
tags: Interlocked c c++ volatile
---
결론부터 이야기 하면 아직 모르겠다.

먼저 volatile 부터 상기해야 했었다. 실제로 고민해야 할 필요가 없었고 보긴 봤지만 정확히 왜 필요한건지? 어디다 사용하는건지? 등등은 잘 이해가 안갔다.

## volatile 이란?
우선 하스켈 전도사가 되신 서광열님의 [C/C++ volatile 키워드](http://skyul.tistory.com/337)을 보면 volatile에는 크게 2가지 기능이 있다.

1. 가시성(visibility)
2. 재배치 방지(barrirer reordering)

### Visibility
레지스터 내용이 아닌 메모리에서 새로 값을 매번 읽어온다. 멀티스레드 환경에서는 다른 스레드에서도 메모리의 값을 본다는 것을 의미한다.

이것을 이해하면서 volatile란 이름은 왜 휘발성일까라는 고민이 어느 정도 해결되었다. 매번 보고 나면 휙 날아가버린다가 아닐까? MMIO의 예제도 쉽게 이해되지만 개인적으로는 멀티스레드 환경을 다뤄야 하는데 변수의 값을 참조하려고 하면 각각의 스레드 레지스터 값이 아닌 메모리의 내용을 직접 봐야 한다. 그렇게 read를 하고 나더라도 바로 다음번에는 같은 값이 유지되리라는 보장은 없다. 읽을 때는 정확하지만 이후엔 샤샤삭~ 이런 느낌??? 뭐 난 이렇게 정리했다.

### Barrier reordering
이 부분은 MSVC 계열에서 지원하는 기능이다. gcc같은 다른 compiler에서는 이런 처리가 있지 않은것 같다. write와 read 에 대한 각각의 barrier 를 지원하며 둘다도 지원한다.

barrier에 대해서는 또 믿고 보는 김민장님의 [C/C++ volatile 키워드](http://egloos.zum.com/minjang/v/2274079)의 글을 보면 잘 설명해주신다. lock prefix에서 결국은 강제적으로 기다리는 부분이 있다는 것을 알려주신다. 김민장님의 글을 보면 결국 intel계열 (x86_64) 에서는 gcc도 lock prefix를 사용할 것 같은데 그렇다면 꼭 MSVC 만의 문제는 아닐것도 같고....

여튼 난 현재 MSCS에 intel계열이니깐 뭐 다 필요하다 치면.....

저 두글만 종합해서 봐도 이해에 큰 도움이 되지만 결론은 `volatile 만으로는 멀티스레드환경에서의 동기화가 보장되지 않는다` 이다. volatile은 동기화 요소가 아니기 때문이다.


이 외에도 volatile 관련 글로는

* [volatile과 interlocked operation](http://lacti.me/2011/08/02/volatile-interlocked-operation/)
* [volatile과 최적화 장벽의 비교 실험](https://kldp.org/node/104910)

이 있는데....

volatile과 InterlockedXXX 계열을 비교한 글은 많고 완전한(?) 동기화를 위해서는 InterlockedXXX 계열을 사용해야 한다는 것은 알겠는데 `왜 InterlockedXXX 계열 함수의 인자가 volatile 인지`는 아직 모르겠다.

InterlockedXXX 쓰는 의도는 메모리의 atomic한 연산을 보장해주는 것이고 barrier 나 visibility 모두 보장해주는 것 같은데... 이거 하나로 끝 아닌가? 하지만 InterlockedXXX 함수들의 선언을 보면 volatile 인자를 사용한다. 만약 이거 안쓰면 어찌 되나???
{% highlight c %}
LONG InterlockedDecrement(
  _Inout_ LONG volatile *Addend
);
{% endhighlight %}
