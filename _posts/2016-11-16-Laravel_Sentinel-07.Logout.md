---
layout: post
title:  "Laravel with Sentinel - 07. Logout & show content to logged users only"
categories: PHP
tags: php laravel sentinel
---
[Laravel 5.3 advanced Authentication #7 Logout & show content to logged users only](https://www.youtube.com/watch?v=mkW2EJVeyAg&list=PL3ZhWMazGi9KB9PajJHWvV2NJ1ITNoNGp&index=7) 참조

## logout 구현

#### web.php
```php
Route::post('/logout', 'LoginController@logout');
```

#### LoginController
```php
public function logout()
{
    Sentinel::logout();
    return redirect('/login');
}
```

#### top.menu.php
{% highlight html %}{% raw %}
<div class="header clearfix">
  <nav>
    <ul class="nav nav-pills pull-right">
      @if(Sentinel::check())
        <li role="presentation">
          <form action="/logout" method="POST" id="logout-form">
            {{ csrf_field() }}
            <a href="#" onclick="document.getElementById('logout-form').submit()">Logout</a>
          </form>
        </li>
      @else
        <li role="presentation"><a href="/login">Login</a></li>
        <li role="presentation"><a href="/register">Register</a></li>
      @endif
    </ul>
  </nav>
  <h3 class="text-muted">
    @if(Sentinel::check())
      Hello, {{ Sentinel::getUser()->first_name }}
    @else
      Authentication with Sentinel
    @endif
  </h3>
</div>
{% endraw %}{% endhighlight %}
