---
title: Vue, Vuex에서 computed 사용
tags: ["vue"]
---
## computed 
계산된 속성 이라고 한다. 

템플릿에 표현식을 넣으면 편리하지만, 코드가 비대해지고 유지보수가 어려워진다. 
복잡한 로직은 `computed`을 사용한다.

`computed`에 정의된 값은 그 안에 의존하는 부분이 있다. 

```javascript js
var vm = new Vue({  
 el: '#example',  
 data: {  
 message: '안녕하세요'  
 },  
 computed: {  
	 // 계산된 getter  
	 reversedMessage: function () {  
		 // `this` 는 vm 인스턴스를 가리킵니다.  
		 return this.message.split('').reverse().join('')  
	 }  
 }  
})
```

`reversedMessage`는 `message`값에 항상 의존적이다. 

### Vuex 에서 computed 사용
Vuex는 store에 값을 저장한다. 이를 사용할때, `computed`에 정의해서 원래 정의된 state에 의존하도록한다. 

#### `mapGetters` 헬퍼 
store에 getter를 로컬의 `computed`에 매핑해준다. 
```javascript js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
    // getter를 객체 전개 연산자(Object Spread Operator)로 계산하여 추가합니다.
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg5NjQ0ODc2Ml19
-->