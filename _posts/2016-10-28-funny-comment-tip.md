---
layout: post
title:  "재미있는 주석 활용"
categories: etc
tags: code
---

{% highlight c %}
//*
	printf("1\n");
/*/
	printf("2\n");
//*/
{% endhighlight %}

이렇게 재미있는 것을 이제야 알다니...

첫번째 "/" 를 넣고 안넣고 간에 첫번째 부분 혹은 두번째 부분 선택이 바뀐다.
왜 그런지는 잘 생각해보면 답이 나온다.

개인적으로는 기존에 아래의 방식처럼 사용했었다. 이건 뭐 팁이랄것도 없긴 하지만 C/C++ 에서만 사용가능한 방법이고 위의 주석은 다른 언어에서도 모두 활용이 가능한 방법이다.

{% highlight c %}
#if 1
	printf("1\n");
#else
	printf("2\n");
#endif
{% endhighlight %}
