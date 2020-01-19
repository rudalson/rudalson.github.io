---
layout: post
title:  "vue router 기초"
categories: frontend
tags: vue router frontend
---

`vue-router`가 `SPA`에서 다른 경로의 `component`를 가르키는 기본 형태는 아래와 같다.

*router/index.js*
```js
{
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About')
  }
```
위의 주석을 살펴보면 SPA이므로 처음 로딩부터 모든 component 요소를 가지고 있기보다는 `lazy-loaded`를 하기 위해 `() function`으로 감싼 것을 볼수 있다. 그리고 그 내용은 `router/index.js` 경로에서 부터 상대경로를 넣어주면 된다. 그리고 저기서 또 하나 살펴 볼 것은 `name`이다. 저 name이 의 components 로 맵핑 시켜두고 실제 vue 에서는 `$route.push()` 로 가르키면 된다. 아래와 같다.


```js
<v-list-item link @click="$router.push({name: 'about'})">
    <v-list-item-action>
        <v-icon>mdi-home</v-icon>
    </v-list-item-action>
    <v-list-item-content>
        <v-list-item-title>Home</v-list-item-title>
    </v-list-item-content>
</v-list-item>
```