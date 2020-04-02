---
layout: post
title:  "easy_tornado"
date:   2020-03-29 01:01:01 +0530
categories: 3월 월간보고서 easy_tornado
---
- 2020.03.29.일~

![/assets/upload_image/Untitled.png](/assets/upload_image/Untitled.png)

![/assets/upload_image/Untitled%201.png](/assets/upload_image/Untitled%201.png)

- 문제에 접속하면 3개의 파일이 존재한다.

![/assets/upload_image/Untitled%202.png](/assets/upload_image/Untitled%202.png)

- `hint.txt`에 들어가보니 `filename` 파라미터와 `filehash` 파라미터를 전달받는다.
- `filehash`는 `md5(cookie_secret+md5(filename))`으로 이뤄지는 것 같다.

![/assets/upload_image/Untitled%203.png](/assets/upload_image/Untitled%203.png)

- filename에 ../../../../../../를 입력해서 전송해보았다.

![/assets/upload_image/Untitled%204.png](/assets/upload_image/Untitled%204.png)

- Error 페이지가 출력된다.

![/assets/upload_image/Untitled%205.png](/assets/upload_image/Untitled%205.png)

- msg 파라미터를 다른 text로 바꾸면 해당 text가 출력된다.

![/assets/upload_image/Untitled%206.png](/assets/upload_image/Untitled%206.png)

- template injection을 예상하여 `{{7*7}}`을 입력해 보았다.
- 필터링이 되어있는 듯 하다.

![/assets/upload_image/Untitled%207.png](/assets/upload_image/Untitled%207.png)

- `{{3^4}}`를 입력해 보니 3+4의 값이 출력되었다. `{{  }}`안의 식이 계산되어 출력됨을 확인할 수 있다.

![/assets/upload_image/Untitled%208.png](/assets/upload_image/Untitled%208.png)

- `SSTI 취약점`이 존재하므로, 해당 취약점을 악용해 내부 파일또는 정보를 획득할 수 있을 것이라고 생각하였다.

### SSTI 취약점이란?

---

**Server-Side Template를 사용하는 이유**

- 동적 웹페이지를 통해 한번 가공한 결과를 웹페이지로 유저에게 보여줄 수 있음. 템플릿을 사용하면 간단한 문법으로 쉽게 제작할 수 있고, 유지보수도 편리함

**종류**

- Ruby, Java, Twig, Smarty, Freemarker, Jade/Codepen, Velocity, Mako, Jinja2

**예시**

- [https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server Side Template Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)
- `{{7*7}}` 을 입력해서 결과 값이 화면에 출력되는지 확인해본다.

**취약점 발생 원인**

- Remote Code Excute가 템플릿의 파일위치로 들어가게 된다면, 인젝션이 가능하다. eval 함수와 동일하게 작동되기 때문에, 악의적인 데이터를 주입하여 다양한 영향을 끼칠 수 있다.

### Tornado란?

---

- 해당 문제의 이름은 Tornado이다. SSTI와 Tornado를 검색해봤다. Tornado는 웹 프레임워크로, 파이썬으로 이벤트를 처리할 때 쓰여진다. 웹소켓 방식에 잘 활용되고, 비동기식 웹페이지를 위한 프레임워크이다.

    from tornado_jinja2 import Jinja2Loader

- 위와 같이 사용할 수 있다.

### Tornado 사용법

---

- 간단한 Tornado 사용법을 알아보았다.

    import tornado.httpserver
    import tornado.ioloop
    import tornado.webclass

- URL 또는 URL 패턴을 tornado.web.RequestHandler의 서브클래스로 매핑해서 사용한다.

[https://blog.naver.com/mildnightjin/221882536165](https://blog.naver.com/mildnightjin/221882536165)

![/assets/upload_image/Untitled%209.png](/assets/upload_image/Untitled%209.png)



### 지한별

1. 지한별 
2. 지한별
3. 지한별

`<?php echo hi;?>`

![/assets/upload_image/image-20200401105439047.png](/assets/upload_image/image-20200401105439047.png)

위 그림과 같다.

