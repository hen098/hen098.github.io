---
title : NPX
tags: ["nodejs"]
---
`CRA`(리액트에서 사용하는 CLI) github에 처음시작을 아래와 같이 가이드한다. 
기존 `npm install -g ~` 부분이 생략되었다.
```bash bash 
npx create-react-app my-app
cd my-app
npm start
```
`npx` 를 사용하면, 설치없이 패키지를 사용한다. 

보통 CLI, Webpack 같은 것들은 전역으로 설치했었다.

### 전역 설치 단점
* 전역으로 설치되어 용량을 잡지만, 자주 사용하지 않는다.
* 한번 설치하면 업데이트를 거의 하지 않는다. (최신 반영이 안됨)

# npx 란 
설치없이 패키지를 사용할 수 있다. 

## 장점 
* 로컬 용량을 잡지 않는다.
* 실제로 설치하여 사용하기전에 테스트해볼 수 있다. 
* 버젼 테스트가 용이하다.
* 사용할 때 최신 버젼을 쓰면되기 때문에 최신화 적용이 쉽다.

## 결론
기존에 전역 설치하여 사용하지만, 자주 사용하지 않던 것들은 `npx`를 사용하면 좋을 것 같다.
Vue cli, webpack 등

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNjg3MDQzNV19
-->