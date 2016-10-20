# '__'의 숨겨진(?) 기능


```{.python}

class Cal(object):
    def __init__(self, value):
        self.__value = value

    def getValue(self):
        return self.__value

cal = Cal(10)
print(cal.__value)      # error
print(cal.getValue())   # print 10

```

클래스의 속성 앞에 `__`를 붙이면 외부에서 직접 접근하지 못하고 메서드를 이용해서 접근해야한다. 외부에서 직접 접근하면 안되는 속성에 사용해야 한다. 
