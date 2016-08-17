---
layout: post
title:  "Short-Circuit Evaluation에 대해서"
date:   2015-05-27 16:17:28 +0900
categories: development
tags: code general sce
---
Short-Circuit Evaluation 은 `&&`이나 `||` 에서 첫번째 argument 에서 조건의 결과값에서 이미 전체 결과값이 판단되었을 경우 첫번째로만 수행하는 것을 말한다.

예를 들어,
{% highlight c %}
BOOL condition1(int pValue)
{
    printf("condition1() has been executed\n");
    return 0 == pValue;
}

BOOL condition2(int pValue)
{
    printf("condition2() has been executed\n");
    return 0 == pValue;
}

int main()
{
    int count1 = 1;
    int count2 = 0;

    if (condition1(count1) && condition2(count2))
    {
        printf("&& test\n");
    }

    count1 = 0;
    if (condition1(count1) || condition2(count2))
    {
        printf("|| test\n");
    }
}
{% endhighlight %}
첫번째 `&&` 연산에서는 condition1 이 false 이므로 condition2() 까지는 수행 시키지 않는다.
두번째 `||` 연산에서는 condition1 이 true 이므로 condition2() 까지는 수행 시키지 않는다.


여기까지만 보면 그렇구나 싶긴 한데 골치 아픈건 다음의 경우이다.
{% highlight c %}
static void *cache_alloc_refill(struct kmem_cache *cachep, gfp_t flags)
{
...
    while (slabp->inuse < cachep->num && batchcount--) {
        STATS_INC_ALLOCED(cachep);
        STATS_INC_ACTIVE(cachep);
        STATS_SET_HIGH(cachep);

        ac->entry[ac->avail++] = slab_get_obj(cachep, slabp,
				    node);
    }
    check_slabp(cachep, slabp);
....
}
{% endhighlight %}

linux-2.6.24의 커널 코드 중 일부이다.
저기 while 문 안의 경우를 보면 && 연산자는 충분히 Short-Circuit Evaluation 를 할 수 있으며 따라서 2번째 조건인 batchcount--를 수행하지 않을 수도 있다.
물론 리눅스 커널 코드야 저런경우까지 고려해서 작성되었겠지만 Short-Circuit Evaluation을 모르고 막쓰다가 2번째 조건에 뭔가 연산을 일으키지 못한다면 흐름을 이해하지 못하게 될 것이다.

정 헷갈리면 나이브하게 다 풀어서 해야 한다.
