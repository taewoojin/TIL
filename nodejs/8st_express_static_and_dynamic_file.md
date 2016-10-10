# 웹페이지 정적/동적 구현
---
### 정적 구현
##### 장점
- 코드 수정시, 서버를 재구동할 필요가 없다.

##### 단점
- 페이지에 로직을 넣을 수 없다.

---
### 동적 구현
##### 장점
- html 페이지를 프로그래밍 적으로 구현 가능하다.
- javascript 함수를 페이지에 사용할 수 있다.

##### 단점
- 코드 수정 시, 서버를 재구동 해야한다. 
- 코드가 복잡해진다.

##### 구현방법

```{.javascript}
// app.js 일부


app.get('/dynamic', function(req, res){
  var lis = '';
  for(var i=0; i<5; i++){
    lis = lis + '<li>coding</li>';
  }
  var time = Date();
  var output = `		// 여러줄의 문자열은 ``로 감싸줘야 한다.
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8">
      <title></title>
    </head>
    <body>
        Hello, Dynamic!
        <ul>
          ${lis}		// 변수 사용법: ${변수명}
        </ul>
        ${time}			// javascript 함수 사용
    </body>
  </html>`;
  res.send(output);
});
```

---
# 결론
`두가지 모두 장단점이 있는데 각 장점만을 사용하기 위해서는 Template 엔진이 필요하다.`
