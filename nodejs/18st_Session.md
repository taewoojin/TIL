# Sesison

### session과 cookie의 차이

쿠키에 값을 저장해서 전달하는 방식과는 다르게 세션은 쿠키에 connect.sid라는
사용자의 식별자를 저장해서 전달한다. 서버는 sid를 가지고 해당하는 데이터를 가져와서
핸들링하게 된다. 쿠키와는 다르게 무슨 데이터가 있는지 모르도록 서버에 저장되기
때문에 보안에 좀 더 안정적이다.

---
### express에서 session 사용하기

express에서 session을 사용하기 위해서는 세션기능을 추가하는 
`express-session` 패키지를 설치해야 한다.


##### session을 메모리에 저장([express-session](https://www.npmjs.com/package/express-session#installation) 패키지)

~~~
npm install express-session --save
~~~

아래는 세션 사용법을 위해 간단히 구현한 코드이다.
```{.javascript}
var express = require('express');
  var session = require('express-session');
  var app = express();
  
  // resave, saveUninitialized 는 기본값인 false, true를 권장
  app.use(session({
    secret: '1234DSFs@adf1234!@#$asd',		// 비밀키
    resave: false,		// 세션값이 변경될 경우, 재저장 여부
    saveUninitialized: true
  }));
  
  app.get('/count', function(req, res){
    if(req.session.count) {
      req.session.count++;
    } else {
      req.session.count = 1;
    }
    res.send('count : '+req.session.count);
  });
  
  app.listen(3000, function(){
    console.log('Connected 3000 port!!!');
  });
```


##### session을 file로 저장([express-file-store](https://www.npmjs.com/package/session-file-store) 패키지)

session 정보는 메모리에 저장을 하기때문에 어플리케이션을 재구동하면 session 
정보가 날라가게 된다.
때문에 메모리 외에도 파일로 session 데이터를 저장하는 session-file-store
라는 모듈을 사용하기도 한다.

```{.javascript}
var session = require('express-session');
var FileStore = require('session-file-store')(session);
 
app.use(session({
  secret: '1234DSFs@adf1234!@#$asd',
  resave: false,
  saveUninitialized: true,
  store: new FileStore(options),	// session store 설정
}));
```
session 모듈을 사용하던 프로젝트에서 위와 같은 설정만 변경해주면 session
데이터가 file로 저장된다. 
저장되는 위치는 기본으로 sessions라는 이름의 디렉토리에 저장된다.

##### OrientDB를 이용해 session 저장([connect-oriento](https://www.npmjs.com/package/connect-oriento) 패키지)

```{.javascript}
var session = require('express-session'),
varOrientoStore = require('connect-oriento')(session);
 
// DB config data
var config = {
  session: {
    server: "host=localhost&port=2424&username=dev&password=dev&db=test"
  }
};
 
app.use(session({
  secret: '1234DSFs@adf1234!@#$asd',
  resave: false,
  saveUninitialized: true,
  store: new OrientoStore(config.session)
}));
```
OrientDB에 DB 생성시 기본적으로 session 테이블이 존재하는데, 세션을 저장하면 이 테이블에 저장이 된다.

contoller에서 session을 저장할 때, 주의할 점이 있다.

```{.javascript}
app.get('/auth/logout', function(req, res){
  delete req.session.displayName;
  
  // session 데이터 처리가 완료된 후, redirect 되게 해야 한다.
  req.session.save(function(){
    res.redirect('/welcome');
  });
});
```
session을 저장하거나 삭제할 경우, redirect 같이 페이지를 넘기는 동작을
하게되면 session 저장 및 삭제가 되기도 전에 동작될 수가 있다.

그러기 때문에 req.session.save() 메서드에 callback함수를 사용함으로써
session 처리가 끝나고 동작이 실행되도록 해야 안전하다.
