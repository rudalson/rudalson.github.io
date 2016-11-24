---
layout: post
title:  "Laravel with Sentinel - 04. Create User"
categories: PHP
tags: php laravel sentinel
---
[Laravel 5.3 advanced Authentication #4 Create User](https://www.youtube.com/watch?v=eWhEztW3KEU&index=4&list=PL3ZhWMazGi9KB9PajJHWvV2NJ1ITNoNGp) 참조

유저를 등록하기 위해 post route를 작성한다.

## Route 등록
등록을 마치면 root로 갈 수 있도록 작성

#### RegistrationController
{% highlight php %}
public function postRegister(Request $request)
{
    $user = Sentinel::registerAndActivate($request->all());
    return redirect('/');
}
{% endhighlight %}

## link 등록
{% highlight html %}{% raw %}
...
<div class="panel-body">
    <form action="/register" method="POST">
        {{ csrf_field() }}
...
{% endraw %}{% endhighlight %}

위처럼 action에 `/register`를 등록한다.

## Sentinel User 변경
위의 과정 이후 실제 유저를 등록하면 처음 등록했던 location 속성때문에 에러가 발생한다.
Sentinel의 User 클래스를 수정해줘야 한다.

#### config/cartalyst.sentinel.php
{% highlight php %}
...
'users' => [
    'model' => 'Cartalyst\Sentinel\Users\EloquentUser',
],
...
{% endhighlight %}
부분을 확인할 수 있다. 저 경로대로 `EloquentUser` 모델을 커스터마이징을 해줘야 한다.

{% highlight php %}
...
protected $fillable = [
        'email',
        'password',
        'last_name',
        'first_name',
        'permissions',
        'location',      // 추가
    ];
...
{% endhighlight %}