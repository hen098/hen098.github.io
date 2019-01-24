---
title: Web Server와 Application Server(WAS)
tags: ["web"]
---

## Web Server
클라이언트가 서버에 페이지 요청하면, 요청을 받아서 정적 컨텐츠(html, css)를 제공하는 서버
정적 컨텐츠가 아니면, WAS에게 처리를 부탁하고, WAS에서 받은 컨텐츠를 응답 (response)해준다.
> 대표 : Apache, nginx
## Application Server (WAS)
동적 컨텐츠를 제공하기 위해 만들어진 어플리케이션 서버 (DB조회, 로직처리 컨텐츠)
asp, php, jsp 등 개발 언어를 읽고 처리
웹서버에서 요청이 오면, 요청을 처리한다. 전달받은 Response 객체를 HTTPResponse 형태로 바꿔서 웹서버에 전달한다.
> 대표 : Tomcat, Jeus, JBoss
기능은 나누어져 있지만, 요즘은 WAS가 Web Server가 포함하고 있는 경우가 많다.

**(톰캣에는 아파치의 기능을 포함하고 있다.)**
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMzQ1NDkxMThdfQ==
-->