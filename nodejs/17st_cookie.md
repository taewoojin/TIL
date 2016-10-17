# Cookie

### [쿠키의 개념](https://ko.wikipedia.org/wiki/HTTP_%EC%BF%A0%ED%82%A4)

cookie를 사용하기 위해서는 따로 [cookie-parser](https://www.npmjs.com/package/cookie-parser) 패키지를 설치 해야 한다.

```{.javascript}
var express      = require('express')
var cookieParser = require('cookie-parser')
 
var app = express()
app.use(cookieParser())		// cookie-parser를 미들웨어로 사용
 
app.get('/', function(req, res) {

  // request의 cookies에서 count을 가져옴
  // request로 전송받는 데이터는 문자열이기 때문에 counting할 경우, 숫자로 변환해야 한다.
  var count = parseInt(req.cookies.count);
  
  // resopnse의 cookie에 count값을 1로 설정하여 client로 보냄
  res.cookie('count', 1);
})
```
사용법은 body-parser와 비슷하다.


### 쿠키를 이용한 장바구니 구현

보통 쇼핑몰의 장바구니는 쿠키를 이용해 구현이 되는데, 쿠키의 사용법을 보여주는
간단한 장바구니 페이지를 구현해 본다.

```{.javascript}
var express = require('express');
var cookieParser = require('cookie-parser');
var app = express();

app.use(cookieParser());

// DB 대신 객체를 이용하여 예제 실습
// 간략한 실습을 위해 template는 구현하지 않음
carts = {
  1:{title:'product 1'},
  2:{title:'product 2'}
}

app.get('/cart', function(req, res){
  var output = ``;
  for(var item in carts){
    output += `
      <li>
        ${carts[item].title}
        <a href='/cart/add/${item}'>add</a>
      </li>
    `;
  }
  res.send(`<ul>${output}</ul><p><a href='/cart/add'>Cart</a></p>`);
});

app.get(['/cart/add', '/cart/add/:id'], function(req, res){
  var id = req.params.id;
  if(req.cookies.cart){
    var cart = req.cookies.cart;
  }
  else{
    var cart = {};
  }

  if(id){
    // id가 키값인 객체가 없으면 cart에 담는다
    if(!cart[id]){
      cart[id] = 0;
    }
    cart[id] = parseInt(cart[id]) + 1;
  }

  res.cookie('cart', cart);
  var output = ``;
  for(var item in cart){
    if(cart[item] != null){
      console.log(item);
      output += `
        <li>
          ${carts[item].title}(${cart[item]})
          <a href='/cart/delete/${item}'>Delete</a>
        </li>
      `;
    }
  }

  res.send(`<ul>${output}</ul><p><a href='/cart'>Home</a></p>`);
});

app.get('/cart/delete/:id', function(req, res){
  var id = req.params.id;
  var cart = req.cookies.cart;

  /* delete 는 단순히 객체와 속성과의 연결을 끊을 뿐 실제로 메모리에서 제거하는 것은 아니다
     delete 하고 싶은 delete 연산자를 사용하기보다 값을 null 이나 undefined 로 설정하는것을 추천한다
     이 경우 hashmap 에서 key가 삭제되는 것은 아니라서 key의 존재를 체크하는 경우에는,
     value === undefined 같은 식으로 체크하면 된다.
  */
  cart[id] = null;
  res.cookie('cart', cart);
  res.redirect('/cart');
});

app.listen(3000, function(){
  console.log('connected 3000 port!!');
});

```


## 쿠키 암호화

쿠키를 암호화 하는 방법은 두가지가 있다.
1. https 사용
2. `cookie-parser 패키지 사용시 암호화에 필요한 key값 설정`

두번째 방법으로 쿠키를 암호화하는 방법을 아래 코드에서 확인해 보자.

```{.javascript}
// cookieParser의 파라미터로 오는 값이 key값이다
app.use(cookieParser('ekwjflaksnnf')

// request에서 암호화된 cookie 사용법
var count = req.signedCookie.count;

// 암호화된 cookie를 response에 넣어 보내는 방법
res.cookie('count', 1, {signed:true});
```
기존의 쿠키 사용법과는 다르게 request에서는 `signedCookie`로 값을 가져오고,
response에는 `signed 옵션`을 준다.