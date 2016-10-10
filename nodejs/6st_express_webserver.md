# Express로 웹서버 구현

앞에서 nodejs로 구현된 웹서버를 express로 구현된 웹서버와 비교해보자.

예제 코드는 [expressjs 공식 가이드](http://expressjs.com/ko/starter/hello-world.html)에서 
옮겨 왔다.

```{.javascript}
// app.js

// var http = require('http');
var express = require('exdpress');
var app = express();

// var server = http.createServer(function(req, res){
//   res.end('Hello world!);
// });
app.get('/', function(req, res){
  res.send('Hello world!');
});

// server.listen(3000, function(){
//   console.log('Example app listening on port 3000!');
// });
app.listen(3000, function(){
  console.log('Example app listening on port 3000!');
});
```
주석 부분이 nodejs의 http 모듈을 사용하여 구현한 웹서버이다.

app.get()는 사용자가 get방식으로 요청할 경우 사용되는 메서드고, 
post방식으로 요청할 때는 app.post()를 사용한다.

app.get() 이외에도 다른 api사용법을 알고 싶으면 
[공식 api문서](http://expressjs.com/ko/4x/api.html)를 보면 
알수 있다.

