---
layout: post
title:  "Laravel with Sentinel - 02. Setting Up"
categories: PHP
tags: php laravel sentinel
---
`Sentinel` 은 `auth` 관련 패키지이다. 이것을 Laravel 5.3에서 사용하는 방법을 정리해본다.

[Laravel 5.3 advanced Authentication #2 Setting up Sentinel](https://www.youtube.com/watch?v=964-xdghBOY&list=PL3ZhWMazGi9KB9PajJHWvV2NJ1ITNoNGp&index=2) 참조

Sentinel By Cartalyst 검색

## Install by Composer
```bash
composer require cartalyst/sentinel "2.0.*"
```

## Integration

#### config/app.php
{% highlight php %}
$providers
...
Cartalyst\Sentinel\Laravel\SentinelServiceProvider::class,

$aliases
...
'Activation' => Cartalyst\Sentinel\Laravel\Facades\Activation::class,
'Reminder'   => Cartalyst\Sentinel\Laravel\Facades\Reminder::class,
'Sentinel'   => Cartalyst\Sentinel\Laravel\Facades\Sentinel::class,
{% endhighlight %}

## Assets
{% highlight bash %}
php artisan vendor:publish --provider="Cartalyst\Sentinel\Laravel\SentinelServiceProvider"
{% endhighlight %}

이후 config/cartalyst.sentinel.php 와 migration file 추가

## Migrations
```bash
php artisan migrate
```
이 때 default user migration이 있었다면 rollback 후 다시 할 것
