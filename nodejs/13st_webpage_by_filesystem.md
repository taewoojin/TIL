# 간단한 웹 페이지 구현

지금까지 배운 내용을 토대로 간단한 웹 페이지를 구현해보기로 했다.

데이터 저장은 DB를 사용하지 않고 파일로 저장하도록 실습해보았다.

아래는 간단한 설명이 포함된 소스이다.

```{.javascript}
var express = require('express');
var bodyParser = require('body-parser');
var fs = require('fs');
var app = express();

// urlencoded 방식으로 bodyParser 설정
app.use(bodyParser.urlencoded({ extended: false }));

// template dir은 views로 설정하고, engine은 jade로 설정
app.set('views', './views');
app.set('view engine', 'jade');

// static file dir을 public으로 설정
app.use('/static', express.static('public'));

app.get('/topic/new', function(req, res){
  res.render('new');
});

app.post('/topic', function(req, res){
  var _title = req.body.title;
  var _description = req.body.description;

  // file 생성(title:파일명, description: 파일내용)
  fs.writeFile('data/'+_title, _description, 'utf8', function(err){
    if(err){
      console.log(err);
      res.status(500).send('Iternal Server Error!');
    }
    res.redirect('/topic/'+_title);
  });
});

// route url을 배열로 여러개를 설정할 수 있다.
app.get(['/topic', '/topic/:id'], function(req, res){
  var id = req.params.id;

  fs.readdir('data', function(err, files){
    if(err){
      console.log(err);
      res.status(500).send('Iternal Server Error!');
    }

    if(id){
      // file을 string으로 읽으려면 'utf8' 옵션을 붙여야 한다.
      fs.readFile('data/'+id, 'utf8', function(err, data){
        if(err){
          console.log(err);
          res.status(500).send('Iternal Server Error!');
        }
        res.render('view', {title:id, description:data, topics: files});
      })
    }
    else{
      res.render('index', {topics: files});
    }
  });
});

// 어플리케이션을 3000번 포트에 연결
app.listen(3000, function(){
  console.log('connected 3000 port!!');
});

```