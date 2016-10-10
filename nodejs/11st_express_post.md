# express-post 전송 방식

### template에서 post로 데이터를 전송
```{.javascript}
// form을 사용하여 post방식으로 데이터를 전송하는 코드

doctype html
html
  head
    meta(charset='utf-8')	// 태그에 속성을 추가하는 방식
  body
    form(action='/form_receiver' method='post')
      p
        input(type='text' name='title')
      p
        textarea(name='description')
      p
        input(type='submit')
        
```

form의 method는 기본적으로 get으로 설정되있다. get방식은 querystring을
사용하여 데이터를 전송한다. 

하지만 method를 post로 설정하여 데이터를 보내면 querystring을 사용하지 않고
내부적으로 데이터를 저장하게 된다.

### template에서 보낸 데이터를 서버에서 수신
- [req.body api](http://expressjs.com/ko/4x/api.html#req.body)

post로 전송한 데이터는 req.body 객체에 담겨 있는데, 기본적으로 request 
body는 정의되어 있지 않다. 이를 사용하기 위해서는 body-parser나 multer 
미들웨어를 설치 해야만 req.body가 정의되고 사용 가능하다.

[body-parser](https://www.npmjs.com/package/body-parser)는
post방식으로 전송한 데이터를 어플리케이션에서 사용하도록 해주는 일종의 
플러그	인이라고 생각하면 된다.

body-parser를 미들웨어라고 했는데, 미들웨어는 공항의 검색대에 비유할 수 있다.

post 방식으로 전송하게 되면 url routing을 하기 전에 미들웨어를 지나면서
body-parser에 걸리게 되고 req.body에 데이터의 name 값과 같은 이름의
프로퍼티를 생성해서 req.body를 사용할 수 있게 해준다.

- [body-parser 설치 및 사용법](https://www.npmjs.com/package/body-parser)


```{.javascript}
// body-parser example

var express = require('express')
var bodyParser = require('body-parser')
 
var app = express()
 
// parse application/x-www-form-urlencoded 
app.use(bodyParser.urlencoded({ extended: false }))
 
// parse application/json 
app.use(bodyParser.json())
 
app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  res.end(JSON.stringify(req.body, null, 2))
})
```
