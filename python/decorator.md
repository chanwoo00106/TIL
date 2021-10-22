# python decorator

데코레이터가 너무 ㅈ같아서 정리하려고 한다

> 데코레이터는 말 그대로 무언가를 꾸밀 때 사용한다<br>
> 아래 예제를 보면

```py
def hello():
    print('hello 함수 시작')
    print('hello')
    print('hello 함수 끝')
 
def world():
    print('world 함수 시작')
    print('world')
    print('world 함수 끝')
 
hello()
world()
```

> 함수에 시작과 끝을 출력하는 함수가 있다. 하지만 모든 함수에 저렇게 `print(시작/끝)`를 하기에는 너무 불편하다<br>
> 그래서 다음 예제를 보면 

```py
def trace(func):                             # 호출할 함수를 매개변수로 받음
    def wrapper():                           # 호출할 함수를 감싸는 함수
        print(func.__name__, '함수 시작')    # __name__으로 함수 이름 출력
        func()                               # 매개변수로 받은 함수를 호출
        print(func.__name__, '함수 끝')
    return wrapper                           # wrapper 함수 반환
 
def hello():
    print('hello')
 
def world():
    print('world')
 
trace_hello = trace(hello)    # 데코레이터에 호출할 함수를 넣음
trace_hello()                 # 반환된 함수를 호출
trace_world = trace(world)    # 데코레이터에 호출할 함수를 넣음
trace_world()
```

> `hello`함수를 실행하기 전에 `trace`함수에 `hello`함수를 넣어서 `trace_hello`로 받은 후 `trace_hello`함수를 호출했다<br>
> 지금 `trace`함수에 매개변수로 들어온 것은 `hello`이고 `trace`함수에선 `hello`함수를 `func`로 받은 후에<br>
> `wrapper`함수를 `wrapper`함수는 함수에 시작과 끝을 출력하는 코드를 넣는다. 마지막으로 `trace` 함수는 `wrapper`를 반환해 준다.<br>
> 반환된 `wrapper`는 `trace_hello`로 받게되고 `trace_hello`를 호출하게 된다.

> 이 작업을 데코레이션으로 하면 더 쉽게 할 수 있는데

```py
def trace(func):                             # 호출할 함수를 매개변수로 받음
    def wrapper():
        print(func.__name__, '함수 시작')    # __name__으로 함수 이름 출력
        func()                               # 매개변수로 받은 함수를 호출
        print(func.__name__, '함수 끝')
    return wrapper                           # wrapper 함수 반환
 
@trace    # @데코레이터
def hello():
    print('hello')
 
@trace    # @데코레이터
def world():
    print('world')
 
hello()    # 함수를 그대로 호출
world()    # 함수를 그대로 호출
```

> 저 데코레이션은 2번째 예제에 `trace_hello = trace(hello)`와 같은 작업을 한다<br>
> 함수 위에 `@함수이름`과 같이 적어주면 `hello`함수가 `trace`함수에서 꾸며져서 나중에 `hello`함수를 호출하면<br>
> 시작과 끝이 붙은 채로 출력이 된다