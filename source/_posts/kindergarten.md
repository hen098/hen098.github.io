이 글은 Kindergarten github readme, 예제 영문 설명을 참고하여 작성한 내용이다. 예제설명은 번역한 내용이 대부분이다. 

# Kindergarten 
프론트에서 인증인가를 책임져 주는 플러그인이다. 
권한에 따라 버튼의 visible까지 구현할 수 있다. 
[vue-kindergarten github](https://github.com/JiriChara/vue-kindergarten)

## Perimeter
Perimeter는 어플리케이션 내부 권한별 기능, 메소드 범위를 정의하는 모듈이다. (특정 컴포넌트, 페이지, 버튼, 헤더 등..). 
특정 권한에 보여야 하는 메소드들, 따라야 하는 규칙들을 정의한다. 

각 Perimeter는 비즈니스 도메인을 나타낸다. 

## Sandbox
Perimeter는 Sandbox안에 연결되어있으며, 메소드들은 Sandbox 통해서 접근가능하다. Sandbox는 Governess에 의해서 통제된다. 규칙을 따르지 않았을 때 발생할 문제들을 예방한다.

## Governess
Sandbox의 보초이다. Child가 어떤 권한없는 활동을 하지 못하도록 한다. 

## Child
어플리케이션의 현재 사용자이다. 

## 예제 
Vue + Vuex + VueRouter + vue-kindergarten 
를 결합해서 사용한 예제를 보고 참고했다. github 소스 뿐 아니라, 자세하게 설명한 포스팅글도 있어서 도움이 많이 되었다. 
github url : https://github.com/martinhbramwell/vue-kindergarten-example.git
[Role Based Authorization 설명](https://codeburst.io/role-based-authorization-for-your-vue-js-and-nuxt-js-applications-using-vue-kindergarten-fd483e013ec5) 

### 예제 내용
localhost:3000 에서 
```
Hello 클릭 > Admin만 Secret 화면 접근 가능
Bye 클릭 > Admin만 특정 문구 보임 
```

### Perimeter 
두개의 Perimeter 작성
1. 어드민만 접근 가능한 "Secret" 페이지 
2. 어드민 권한만 특정 문장이 보이는 "bye" 페이지

perimeter 를 작성하는 방법은 작성자에게 달려있다. 
예를들어, 블로그 페이지를 작성하며 기사, 댓글, 태그를 각자의 perimeter로 작성하거나, 하나의 perimeter 에 모두 작성하거나.! 

예제에서는 두 perimeter에서 모두 admin인지를 체크한다. `BasePerimeger`를 만들어서 부모 perimeter로 두고, 목적에 따라 나머지를 작성한다. 

> 참고하면 Perimeter 작성 부분을 이해하는데 도움이 됨
[https://github.com/JiriChara/kindergarten/blob/master/src/Perimeter.js](https://github.com/JiriChara/kindergarten/blob/master/src/Perimeter.js)

```javascript 
// src/perimeters/BasePerimeter.js

import { Perimeter } from 'vue-kindergarten';

export default class BasePerimeter extends Perimeter {  
  isAdmin() {  
    return this.child && this.child.role === 'admin';  
  }  
}
```

```javascript 
// src/perimeters/byePerimeter.js

import BasePerimeter from './BasePerimeter';

export default new BasePerimeter({  
  purpose: 'bye',
  
  govern: {      
    'can route': () => true,

    'can viewParagraph': function () {  
      return this.isAdmin();  
    },  
  },  
});
```

### Child > 현재 사용자 
추출하는 방법이 필요하다. 예제에서는 store에 저장된 user 사용
```javascript 
// src/child.js

export default store => (store ? store.state.user.info : null);
```

###  Bye 페이지에서 admin만 특정 문구 보이기 
```html
// src/components/Bye.vue
<template>
    <p v-show="$isAllowed('viewParagraph')">This paragraph is secret!</p>  
 </template>
<script>  
  import byePerimeter from '../perimeters/byePerimeter';

  export default {  
    perimeters: [  
      byePerimeter,  
    ],  
  };  
</script>
```
혹은 아래처럼 작성할 수 있다. `$bye`는 perimeter의 purpose 이다. `viewParagraph`가 각각 정의된 여러 perimeter들을 로딩했다면 이처럼 사용한다. 

```vue
<p v-show="$bye.isAllowed('viewParagraph')">This paragraph is secret!</p>
```

### admin만 secret페이지 접근 가능 
#### router.js 
화면마다 만들어진 perimeter들을 사용
```javascript 
const  showTheBugInAction  =  false; // ******** Set this to true to see the error
// 										******** "Cannot read property 'from' of undefined"

router.beforeEach((to, from, next) => {
  const perimeter = perimeters[`${to.name}Perimeter`];
  if (perimeter) {
    let sandbox = null;

    if (showTheBugInAction) {	// 에러가 발생하면
      sandbox = createSandbox(child(store), {
        perimeters: [
          perimeter,
        ],
        governess: new RouteGoverness({ from, to, next }),
      });
      return sandbox.guard('route');
    }

    sandbox = createSandbox(child(store), {		// sandbox를 create한다.
      perimeters: [
        perimeter,
      ],
    });
    if (!sandbox.isAllowed('route')) {
      return next('/');
    }
  }
  return next();
});
```

gorverness는 사용자가 제한된 리소스에 접근하려고 할때 디폴트 행위이다. 아래는 라우트에 대한 governess 
```javascript 
// src/governesses/RouteGoverness.js

import { HeadGoverness } from 'vue-kindergarten';

export default class RouteGoverness extends HeadGoverness {  
  constructor({ from, to, next }) {  
    super();

    this.next = next;  
    this.from = from;  
    this.to = to;  
  }

  guard(action) {  
    this.next(super.isAllowed(action) ? undefined : '/');  
  }  
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg5MDE2ODI5MSwtMTE4MDIwOTU0Ml19
-->