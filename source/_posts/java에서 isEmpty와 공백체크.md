---
title: java에서 isEmpty와 공백체크
tags: ["java"]
---

String의 `isEmpty`사용
```
"".isEmpty();					// true
" ".isEmpty();					// false
null.isEmpty();					// false
```
- null 체크 안됨 > null point exception 발생

Apache Commons 의 `isEmpty` 사용
```
StringUtils.isEmpty(""); 		 // true
StringUtils.isEmpty(" "); 		 // false
StringUtils.isEmpty(null);		 // true
```
- null 체크됨 
- 공백 체크 안됨

Apache Commons 의 `isBlank` 사용
```
StringUtils.isBlank(""); 		// true
StringUtils.isBlank(" "); 		// true
StringUtils.isBlank(null);		// true
```
- null 체크됨
- 공백 체크됨
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE1NDEwNjA5NywyOTU5MzIwN119
-->