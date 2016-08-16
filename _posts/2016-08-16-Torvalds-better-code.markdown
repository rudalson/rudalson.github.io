---
layout: post
title:  "토발즈의 더 좋은 코드"
date:   2016-08-16 14:34:28 +0900
categories: jekyll update
---
TED의 [리누스 토발즈의 기본 철학][Torvalds-TED]편에서 보면

토발즈가 같이 작업하고픈 개발자에 대해 간략히 설명하면서
Singly-List를 구현하는 코드를 보여준다.

자신이 생각할 때 `좋은 코드`와 `나쁜 코드`에 대해 보여준다.

개인적으로 이런 코드는 완전히 모른다고 하기에도 할 수 없지만 안다고도 할 수 없는게
실개발에서는 잘 나오지가 않는다.

꾸준히 적용해보려 노력해보는 수 밖에 없겠다.

일반적인 코드
{% highlight c %}
remove_list_entry(entry)
{
	prev = NULL;
	walk = head;
	
	// Walk the list
	while (walk != entry) {
		prev = walk;
		walk = walk->next;
	}
	
	// Remove the entry by updating the
	// head or the previous entry
	if (!prev)
		head = entry->next;
	else
		prev->next = entry->next;
}
{% endhighlight %}


더 나은 코드
{% highlight c %}
remove_list_entry(entry)
{
	// The "indirect" pointer points to the
	// *address* of the thing we'll update
	
	indirect = &head;
	
	// Walk the list, looking for the thing that
	// points to the entry we want to remove_list_entry
	while ((*indirect) != entry)
		indirect = &(*indirect)->next;
	
	// .. and just remove it
	*indirect = entry->next;
}
{% endhighlight %}

[Torvalds-TED]: https://www.ted.com/talks/linus_torvalds_the_mind_behind_linux?language=ko#t-17586
