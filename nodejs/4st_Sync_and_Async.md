# 동기 & 비동기 

### 개념
일상생활에서의 예를 들어, 
~~~
빨래 - 설겆이 - 청소 
~~~
위 3가지 일을 한다고 가정해 보겠습니다.

빨래(1시간) > 설겆이(1시간) > 청소(1시간) 순서로 차례대로 일을 해서 끝내면
**동기적**으로 일을 했다고 볼 수 있습니다. 이 경우에는 3시간이 걸리게 되죠.

하지만 각 해당 업체에 연락을 해서 일을 주문하면 업체가 작업을 진행하고 끝나면
연락을 주게 되는데, 이를 **비동기적**으로 일을 했다고 할 수 있습니다.
이 경우는 일 처리의 순서가 상관없을 때 사용합니다.


---
기술적인 영역인 email 서비스를 예를 들어보면, 

email 발송 시스템에서 10,000명에게 email을 **동기적**으로 보낼 경우, 
1인당 1초가 걸리면 총 10,000초가 걸리게 됩니다.

1인당 보내는 시간이나 사용자 수가 많아지게 되면 더 많은 시간이 걸리게 되겠죠.

하지만 email을 보내는 별도의 시스템에 위임을 하여 비동기적으로 일을 처리한다면, 
background에서 시스템이 일을 처리하는 동시에 foreground에서는 다른 일을
할 수 있습니다.



### 코드
[FileSystem](https://nodejs.org/dist/latest-v6.x/docs/api/fs.html) 
모듈을 예로 들어 보면,

```{.javascript}
// sync_async.js

const fs = require('fs');	# FileSystem load

// Sync
console.log(1);

var data = fs.readFileSync('data.txt', {encoding:'utf8'});
console.log(data);



// Async
console.log(2);

// 비동기로 작업할 경우, callback 필요
var data = fs.readFile(
  'data.txt', {encoding:'utf8'}, 
  function(err, data){
    console.log(3);
    console.log(data);
  }
);

console.log(4);
```
~~~
// data.txt

Hello sync Async~
~~~

~~~
// node sync_async.js 실행결과

1
Hello Sync Async~

2
4
3
Hello Sync Async~
~~~
위 출력결과의 순서에 주목.

비동기 작업인 readFile 메서드 실행이 끝나면 callback 안의 내용이 실행되기
때문에 console.log(4)가 먼저 실행됩니다.

