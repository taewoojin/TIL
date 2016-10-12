# Orientdb

---
### 장점
([공식 페이지](http://orientdb.com/why-orientdb/))

1.Built for speed
: 초당 40만건의 recode를 추가할 수 있는 성능

2.No more joins
: join 기능이 더이상 필요 없고, 더 좋은 방식을 제공한다.

---
### 특성

1.Graph Database: 관계형 DB보다 더 관계성을 더 잘 다루는 특징을 가짐.

2.Document Database: 문서의 형식으로 데이터를 저장. RDB보다 유연하다.

3.Object-Oriented Concepts: RDB에서의 table 대신에 class를 사용하고,
상속관계를 통해 구조를 재사용할 수 있는 특성을 가짐. 유사한 구조의 테이블이 많은
프로젝트에서는 상당히 유용한 특징이다.

4.Schema-full,Schema-less,Schema mix: 상황에 따라 구조를 가지거나 
가지지 않거나, 자유롭게 사용할 수 있다.

5.User and Role Security, Record Level Security: RDB에서는
table 단위로 인증 레벨을 지정하지만, 여기서는 행단위로 인증 레벨을 지정할 수
있다. 예를 들면 같은 테이블에서 자신이 작성한 레코드만 접근 가능한 인증 레벨을
지정할 수 있다.

6.SQL: orientDB는 Nosql로 분류되지만 sql도 사용할 수 있다.

7.Relationship Traversing: 관계성을 가지고 있는 데이터를 조회하는데 있어서
데이터가 아무리 많아도 O(1)의 속도를 가진다.

8.Multi-Master Replication: 쓰기 기능을 가진 여러 DB 사용 가능하다.

9.Sharding: 분산처리를 자동으로 해줌.

10.Native HTTP Rest/JSON: 미들웨어 없이 javascript를 이용해서 데이터를
핸들링할 수 있다.

11.Commercial Friendly License: 상업적으로 사용가능하다.

---
### 설치

OrientDB는 java로 만들어졌기 때문에 [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)를 먼저 설치 해야한다.

그 후에 [OrientDB](http://orientdb.com/download/)를 다운받아 압축을 풀었으면, 디렉토리 안에 bin이라는 디렉토리
내부로 들어간다. 보통 GUI에서 실행하지 않기 때문에 터미널로 접속하여 들어간다.

서버를 실행시키는 파일이 osx,linux는 `server.sh`이고, 윈도우는 `server.bat`이다.

~~~
// osx, linux
>> sudo ./server.sh

// window
>> server.bat
~~~

개발환경에서는 따로 환경설정을 하지 않겠지만, 실제 production 환경에서의
DB설정은 [공식문서](http://orientdb.com/docs/last/Tutorial-Installation.html)를 보고 설정해야 한다.

DB 서버를 실행하면,

~~~
2016-10-12 15:55:47:817 INFO  Listening http connections on 0.0.0.0:2480 (protocol v.10, socket=default) [OServerNetworkListener]
~~~

많은 내용 중 포트번호가 나오는 내용을 볼 수 있는데, 위와 같이 포트번호가 2480이면
`localhost:2480`로 접속하여 DB에 접속할 수 있는 관리자 페이지가 나온다.
여기서 DB를 생성하고 접속하면 된다.

---
### nodejs와 연결하여 사용하기(orientjs)

[orientjs github page](https://github.com/orientechnologies/orientjs)


##### orientjs을 이용한 CRUD

```{.javascript}
var OrientDB = require('orientjs')

// db 정보 설정
var server = OrientDB({
  host: 'localhost',
  port: 2424,				// defualt = 2424
  username: 'root',
  password: 'password'
});

// DB 가져오기
var db = server.use('mydb');

// record read
db.record.get('#21:0').then(function(results){
  console.log(results);
});

// CREATE
var sql = 'insert into topic (title, description) value(:title, :desc)';
var param = {
  params:{
    title: 'javascript',
    desc: 'javascript is very flexible'
  }
}
db.query(sql, param).then(function(results){
  console.log(results);
});

// READ
var sql = 'select from topic where @rid=:id';
var param = {
  params:{
    id: '#21:0'
  }
}
db.query(sql, param).then(function(results){
  console.log(results);
});

// UPDATE
var sql = 'update topic set title=:title where @rid=:id';
var param = {
  params:{
    title: 'python',
    id: '#21:0'
  }
}
db.query(sql, param).then(function(results){
  console.log(results);
});

// DELETE
var sql = 'delete from topic where @rid=:id';
var param = {
  params:{
    id: '#21:0'
  }
}
db.query(sql, param).then(function(results){
  console.log(results);
});


```



