#모듈

모듈 == 부품
~~~
# http 모듈 인스턴스를 호출하는 방식
const http = require('http');
~~~
http도 nodejs에서 제공하는 모듈의 한 종류로 이와 같이 nodejs에서 제공하는
모듈을 사용하는 방법을 배우는게 이번 장의 목표입니다.

http와 같이 nodejs에 내장된 api들은 [api 공식 문서](https://nodejs.org/ko/docs/) 에서 찾아서 사용법을 볼 수 있습니다.


#NPM(Node Package Manager)
npm은 nodejs 및 javascript가 제공하는 모듈 이외에도 타인의 모듈을 설치,
삭제, 업그레이드, 의존성 관리 등을 해주는 관리툴입니다.

nodejs를 위해 만들어진 패키지 관리툴이지만 nodejs를 기반으로 만들어진 개발도구들
을 설치해서 관리할 수 있습니다.

npm으로 모듈을 설치하는 방식은 아래와 같이 두가지가 있습니다.
~~~
# 전역에서 사용하는 독립적인 소프트웨어로 설치
npm install uglify-js -g

# 프로젝트안에서 사용되는 모듈로 설치
npm install uglify-js
~~~

### 독립적인 소프트웨어로 설치하여 사용

```{.javascript}
# pretty.js

function hello(name){
  console.log('Hi' + name);
}
hello('taewoo')
```

~~~
# uglifyjs [input files] [options]
# -o, --output: Output file
# -m, --mangle: 바껴도 상관없는 변수명을 한문자로 변경(최소화)

>> uglifyjs pretty.js
function hello(name){console.log("Hi"+name)}hello("taewoo");
>>uglifyjs pretty.js -o pretty.min.js -m
>>
~~~

```{.javascript}
# pretty.min.js

function hello(o){console.log("Hi"+o)}hello("jason");
```

### 프로젝트 안에서 모듈로 설치하기
프로젝트 안에 설치할 경우에는 약간의 복잡한 절차를 가집니다

#####1.프로젝트 디렉토리를 npm 패키지로 지정한다.

~~~
# npm config 환경 설정(package.json 생성)

>> npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: nodejs					# 프로젝트 name
version: (1.0.0)				
description: nodejs_test		
entry point: 					# 패키지를 구동시키는 javascript
test command: 					# test 실행시 명령어
git repogitory: 				# git 주소
keywords:
author:
license:
~~~

~~~
# package.json

{
  "name": "js",
  "version": "1.0.0",
  "description": "js",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
~~~

#####2.모듈을 설치 한다.

모듈을 설치할 때 중요한 점은 --save 옵션에 따라 다른 의존성을 가지게 된다는
것입니다.
~~~
# 개발할 때 잠깐만 사용하는 모듈인 경우.
>> npm install underscore

# 해당 프로젝트에서 항상 필요한 모듈인 경우.
>> npm install underscore --save
~~~

--save 옵션을 붙여 설치하게 되면 아래와 같이 
package.json(npm 환경설정파일)에 의존성이 추가되는걸 볼 수 있습니다.
~~~
# package.json

{
  "name": "js",
  "version": "1.0.0",
  "description": "js",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
  "dependencies": {
    "underscore": "^1.8.3"
}
~~~

#####3.모듈을 사용한다

아래는 설치한 underscore 모듈을 사용한 코드입니다.
```{.javascript}
# underscore는 굉장이 많이 사용되는 모듈로 변수명은 보통 _를 사용한다고  함.
const _ = require('underscore');
var arr = [3,6,9,1,12];
console.log(_.first(arr));	# underscore 모듈의 first 메서드 사용
console.log(_.last(arr));	# last 메서드 사용
```