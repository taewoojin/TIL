# callback 함수

```{.javascript}
var a = [1,3,2];

# callback 함수
function b(v1, v2){
  return v1-v2;
}

a.sort(b);		# 함수
console.log(a);
```
callback 함수 - 다른 함수에게 호출 당할 함수. callback 함수를 이용하여 
다른(예를 들면 sort) 함수의 기본적인 방법을 확장할 수 있다.

```{.javascript}
a.sort(function(v1, v2){return v1-v2;});
```
callback 함수인 b를 한번만 사용할 경우에는 위와 같이 사용할 수 있는데,
이를 익명함수라고 한다.

