# express url 

### querystring 알아보기
route path 뒤에 ? 이후로 나오는 것들은 querystring이라고 한다.

querystring은 &로 연결하여 여러개의 데이터를 함께 요청할 수 있다.
~~~
req.pathname
    |
    |
 ----- 
/shoes?order=desc&shoe[color]=blue&shoe[type]=converse
       -----                       ----------
         |                             |
         |                             |
 req.query.order                       |
                             req.query.shoe.type
                         
~~~                      

```{.javascript}
app.get('/topic', function(req, res){
  res.send(req.query.order+ ', '+req.query.shoe.type);
});

// 출력
desc, converse
```

---

### querystring url 사용하기

```{.javascript}
app.get('/topic', function(req, res){
  var topics = [
    'Javascript is....',
    'Nodejs is...',
    'Express is...'
  ];
  var output = `
  <a href="/topic?id=0">JavaScript</a><br>
  <a href="/topic?id=1">Nodejs</a><br>
  <a href="/topic?id=2">Express</a><br><br>
  ${topics[req.query.id]}		// req.query 객체 사용
  `
  res.send(output);
})
```

### semantic url(의미론적 url) 사용하기

```{.javascript}
app.get('/topic/:id', function(req, res){
  var topics = [
    'Javascript is....',
    'Nodejs is...',
    'Express is...'
  ];
  var output = `
  <a href="/topic?id=0">JavaScript</a><br>
  <a href="/topic?id=1">Nodejs</a><br>
  <a href="/topic?id=2">Express</a><br><br>
  ${topics[req.params.id]}		// req.params 객체 사용
  `
  res.send(output);
})
app.get('/topic/:id/:mode', function(req, res){
  res.send(req.params.id+','+req.params.mode)
})
```

semantic url은 직관적이고 깔끔한 url을 사용할 수 있게 만들어 준다.
최근에는 querystring 보다 semantic url을 사용하는 추세이다.

semantic url의 구조와 예시는 [위키피디아](https://en.wikipedia.org/wiki/Semantic_URL)에서 확인해 볼 수 있다.

