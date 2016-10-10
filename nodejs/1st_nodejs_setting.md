# nodejs 설치
	
https://nodejs.org/ko/download/
에서 해당되는 OS를 선택하여 설치.

패키키 매니저를 사용하여 설치 하려면 
https://nodejs.org/ko/download/package-manager/
에서 설치하면 됩니다.


```{.javascript}
>> node --version
v6.7.0
>> node
> console.log('Hello world')
Hello world
```
설치가 완료됬으면 위와 같이 버전을 확인하고, 함수를 실행시켜서 제대로 동작하는지
확인할 수 있습니다.


# 기본 HTTP 서버 코드

```{.javascript}
const http = require('http');	# http모듈 인스턴스를 가져와서 상수에 넣음.

const hostname = '127.0.0.1';
const port = 3000;

# http 인스턴스의 메서드(createSever)를 사용하여 서버를 생성
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');	# 사용자가 해당 컴퓨터에 접속하게 되면 'Hello World'를 응답
});

# 서버가 Ip,port에 해당하는 컴퓨터에 listening하도록 설정
server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```
위에 보이는 소스는 공식 홈페이지에서 nodejs를 소개하는 웹서버 코드입니다.
문법은 아직 잘 모르기 때문에 큰 흐름만 주석으로 적어봤습니다.




