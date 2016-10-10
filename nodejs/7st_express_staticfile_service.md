# express - static file 서비스하는 법

[정적파일에 대한 공식문서](http://expressjs.com/ko/starter/static-files.html)


```{.javascript}
// public이라는 이름의 디렉토리에 포함된 정적 파일을 제공하도록 설정
app.use(express.static('public'));
```
여러 개의 디렉토리를 지정하여 서비스 할 수도 있다.

또한 아래와 같이 prefix를 붙여 설정할 수 있다.
```{.javascript}
app.use('/static', express.static('public'));
```
prefix와 함께 설정해주면, 
`localhost:3000/static/(file name)` 
이렇게 호출해야 한다.


