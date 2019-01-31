---
title : vue router에 컴포넌트 지연로딩 (Lazy Load)
tags: ["vue"]
---

# Lazy Load 
처음 웹 페이지를 로드할 때 접근하지 않고, 실제 라우팅이 발생할 때 로드되도록 컴파일하는 방법이다. 빠른 초기 로딩을 도와준다. 
`dynamic imports`와 `webpack suppoerts` 로 지연로딩을 쉽게 구현할 수 있다. 

## 방법 1
```javascript js
{
    path: '/messages/',
    name: 'messages-main',
    component: () => import(/* webpackChunkName: "messages-main" */ './views/messages-main.vue')
},
```
## 방법 2
```javascript js
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

function loadView(view) {
  return () => import(/* webpackChunkName: "view-[request]" */ `@/views/${view}.vue`)
}

export default new Router({
  routes: [
    {
      path: '/',
      name: 'home',
      component: loadView('Home')
    },
    {
      path: '/about',
      name: 'about',
      component: loadView('About')
    }
  ]
})
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc4NjMyMjk5MV19
-->