---
title: "[vue] 자식에서 부모 데이터 통신"
tags: ["vue"]
---

## 자식 > 부모 통신 

### `.sync` 수식어
> Vue2.3 +

자식 컴포넌트가 `.sync`를 가지는 속성을 변경하면 값의 변경이 부모에 반영할수 있다. 


### 부모
```html
<template>
	<component :visible.sync="visible">
	</component>
</template>

<script>
export default {
	data: () => {
		visible: false
	}
}
</script>
```

### 자식
```html
<template>
	<button @click.prevent="$emit('update:visible', !visible)">전달</button>
</template>

<script>
export default {
	props: {
		visible: {
			type: String
		}
	}
}
</script>
```
자식에서 데이터를 전달하고 싶은 순간
* 값이 변하는 순간 > watch 사용
* 버튼을 클릭 (`@click`)

에 emit을 시켜주면된다. `update:prop명`을 해주면 변경된 자식데이터를 부모로 전달할 수 있다.

참고

[https://medium.com/@jeongwooahn/vue-js-%EC%96%91%EB%B0%A9%ED%96%A5-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B0%94%EC%9D%B8%EB%94%A9-%ED%99%9C%EC%9A%A9-%EB%AA%A8%EB%8B%AC-%EB%A0%88%EC%9D%B4%EC%96%B4%ED%8C%9D%EC%97%85-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0%EB%A5%BC-%ED%86%B5%ED%95%98%EC%97%AC-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-sync-4e2a5cf70dc4](https://medium.com/@jeongwooahn/vue-js-%EC%96%91%EB%B0%A9%ED%96%A5-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B0%94%EC%9D%B8%EB%94%A9-%ED%99%9C%EC%9A%A9-%EB%AA%A8%EB%8B%AC-%EB%A0%88%EC%9D%B4%EC%96%B4%ED%8C%9D%EC%97%85-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0%EB%A5%BC-%ED%86%B5%ED%95%98%EC%97%AC-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-sync-4e2a5cf70dc4)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzE4MDU0NDQ1LDUwNTk1MzY2MywxMTQzNz
g3NjQyXX0=
-->