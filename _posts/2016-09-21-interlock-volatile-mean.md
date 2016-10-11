---
layout: post
title:  "InterlockedXXXXX() 함수의 인자는 왜 volatile 일까?"
categories: development
tags: Interlocked c c++ volatile
---

volatile 키워드가 어떤 동작을 하는 것인지 부터 살펴보고 Interlocked계열 (Linux에서는 atomic 계열)과 연관성을 살펴보겠다.

## volatile 이란?
우선 서광열님의 [C/C++ volatile 키워드](http://skyul.tistory.com/337)을 보면 volatile에는 크게 2가지 기능이 있다.

1. 가시성(visibility)
2. 재배치 방지(barrirer reordering)

### Visibility
레지스터 내용이 아닌 메모리에서 값을 매번 새로 읽어온다. 그렇기 때문에 멀티스레드 환경에서 다른 스레드에서도 이런 공유변수 값을 메모리에서 직접 본다는 것을 의미한다.

이것을 이해하면서 volatile란 이름은 왜 휘발성일까라는 고민이 어느 정도 해결되었다. 매번 보고 나면 휙 날아가버린다가 아닐까? MMIO의 예제도 쉽게 이해되지만 개인적으로는 멀티스레드 환경을 다뤄야 하는데 변수의 값을 참조하려고 하면 각각의 스레드 레지스터 값이 아닌 메모리의 내용을 직접 봐야 한다. 그렇게 read를 하고 나더라도 바로 다음번에는 같은 값이 유지되리라는 보장은 없다. 읽을 때는 정확하지만 이후엔 샤샤삭~ 이런 느낌???

### Barrier reordering
메모리에 대한 접근 중 컴파일러 최적화 기능으로 인해 재배치를 막는 것이다.

이 부분은 MSVC 계열에서 지원하는 기능이다. gcc같은 다른 compiler에서는 이런 처리가 있지 않은것 같다. write와 read 에 대한 각각의 barrier 를 지원하며 둘다도 지원한다.

이 barrier 기능 제공 때문에 Interlocked계열 함수 본연의 기능과 겹침으로 인해 많은 혼동이 왔었다.

barrier에 대해서는 또 믿고 보는 김민장님의 [C/C++ volatile 키워드](http://egloos.zum.com/minjang/v/2274079)의 글을 보면 잘 설명 해주신다. 그리고 뒤이어 [C/C++ volatile에 대한 오해](http://minjang.egloos.com/2370662)도 같이 봐야한다. lock prefix에서 결국은 강제적으로 기다리는 부분이 있고 그것이 바로 barrier를 의미한다. 김민장님의 글을 보면 결국 intel계열 (x86_64) 에서는 gcc도 lock prefix를 사용할 것 같은데 그렇다면 꼭 MSVC 만의 문제는 아닐것도 같고....

여튼 난 현재 MSCS에 intel계열이니깐 뭐 다 필요하다 치면.....

저 두글만 종합해서 봐도 이해에 큰 도움이 되지만 결론은 `volatile 만으로는 멀티스레드환경에서의 동기화가 보장되지 않는다` 이다. volatile은 동기화 요소가 아니기 때문이다.

이 외에도 volatile 관련 참고한 글로는

* [volatile과 interlocked operation](http://lacti.me/2011/08/02/volatile-interlocked-operation/)
* [volatile과 최적화 장벽의 비교 실험](https://kldp.org/node/104910)


이 2가지 기능을 토대로 하지만 volaile의 기능 범위는 컴파일러마다, 아키텍쳐마다, 시대별 마다 조금씩 달라져 가는 것 같다. 그만큼 C/C++에서도 불분명하게 정의되어 있는 면이 강해서이기도 한것 같다.

## Interlocked 에서의 volatile

{% highlight c %}
LONG InterlockedDecrement(
  _Inout_ LONG volatile *Addend
);
{% endhighlight %}

Interlocked계열 함수중 MSDN에 나와있는 정의이다. 처음 궁금했던 인자의 volatile 여부가 필요한 이유는 1번 Visibility와 관련이 있다. 컴파일러 최적화를 막는 것이다. 2번째 기능은 MSVC 계열에서 제공된 것이기도 하지만 Interlocked 본연의 barrier 가 제공되는 것이고...


## 그 외 참조

봐야 할 글은 많으나 봐도 다 모르겠다.

* [Why the "volatile" type class should not be used](https://www.kernel.org/doc/Documentation/volatile-considered-harmful.txt)
* [Memory Ordering at Compile Time](http://preshing.com/20120625/memory-ordering-at-compile-time/)
* [volatile (C++)](https://msdn.microsoft.com/en-us/library/12a04hfd%28v=vs.120%29.aspx?f=255&MSPPError=-2147217396)
* [Synchronization and Multiprocessor Issues](https://msdn.microsoft.com/en-us/library/ms686355.aspx)
