# file upload 

express에는 자체적으로 file upload를 하는 기능이 없다.
때문에 따로 모듈을 설치해야 되는데, file upload 모듈 중 
**multer**라는 모듈을 설치해서 사용해보도록 한다.

[multer 설치 및 사용법](https://github.com/expressjs/multer)


```{.javascript}
// template

doctype html
html
  head
    meta(charset='utf-8')
  body
    // enctype을 꼭 'multipart/form-data'로 설정해야 한다.
    form(action='upload' method='post' enctype='multipart/form-data')
    input(type='file' name='userfile')
```
`file을 전송할 때는 꼭 enctype을 'multipart/form-data'로 설정하는 것을
잊지 말아야 한다.`


```{.javascript}
// controller

var express = require('express')
var multer  = require('multer')

// file upload할 디렉토리 설정
var upload = multer({ dest: 'uploads/' })

var app = express()

// upload.single('userfile'): upload 미들웨어 설정
// body-parser와 같이 upload 미들웨어를 설정해야 req.file 객체가 생성됨
app.post('/upload', upload.single('userfile'), function (req, res, next) {
  // req.file is the `userfile` file
  // req.body will hold the text fields, if there were any
})
```

### multer 자세히 들여다보기

multer 옵션들을 이용해 커스터마이징 할 수 있다. 
파일 저장시 장소와 파일이름을 설정하는 코드를 들여다 보자.

[storage 옵션 메뉴얼](https://github.com/expressjs/multer#storage)

```{.javascript}
var storage = multer.diskStorage({

  // 파일 업로드할 디렉토리 설정
  destination: function (req, file, cb) {
  
    // 파일 형식으로 분기하여 저장하는 것도 가능하다.
    if(req.file.type.match('*.png'){
      cb(null, '/upload_image')
    }
    else{
      cb(null, '/upload_other')
    }
  },
  
  // 업로드시 파일명 설정
  filename: function (req, file, cb) {
    cb(null, file.originalname  + '-' + Date.now())
  }
})

var upload = multer({ storage: storage })
```