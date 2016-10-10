# Express-템플릿 엔진(jade)

[템플릿 엔진 설치 및 사용법(공식문서)](http://expressjs.com/ko/guide/using-template-engines.html)


---
### jade 문법

[jade 공식 문서](jade-lang.com)

```{.xml}
// index.jade
// indent로 태그를 구분하기 때문에 주의 해야한다.

html
  head
  body
    h1 Hello Jade		// 태그 안에 내용을 넣으려면 줄바꿈을 하지 않는다
    ul
      -for(var i=0; i<5; i++)	// 로직은 앞에 '-'를 붙여야 한다
        li coding
    div= time		// rendering할 때, 함께 보낸 time을 div에 넣는다
```

`res.render('index', {time: Date()});` 
            -------
               |
이와 같이 파라미터를 페이지에 보내 동적으로 렌더링할 수 있다.


---
##### tip
`app.locals.pretty = True` 

express에 위 옵션을 설정하면 개발자도구에서 정돈된 html 태그들을 볼 수 있다.