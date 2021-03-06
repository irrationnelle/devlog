---
title: 네임스페이스와 함수
date: '2019-12-08T01:01:03.284Z'
template: "post"
draft: false
slug: "namespace-function"
category: "Python"
tags:
  - "python3"
  - "namespace"
description: "의자 바꾸기 함수에는 `return` 이 없었다. 하지만 함수를 호출하고 나면 의자가 바뀐 것을 알 수 있었다"
socialImage: ""
---

원래는 `return` 의 사용목적에 대해서 쓰고 있었으나 내용이 길어지고 여러가지 개념들이 혼재되는 것 같아서

네임스페이스 부분을 잘라내어 따로 포스팅한다.

`return` 에 대한 언급이 간간히 나오는데 수정하지 않고 냅둔 것은,

`return` 의 사용에 대해 어렴풋이 느끼고 생각해볼 수 있는 기회라고 여겨서이다.


# 네임스페이스 이해하

의자 바꾸기 함수에는 `return` 이 없었다. 하지만 함수를 호출하고 나면 의자가 바뀐 것을 알 수 있었다.

여기서 알 수 있는 점은 `return` 문을 사용하지 않아도 *함수 바깥의 값을 변하게 할 수 있다* 는 것이다.

게다가 *더 충격적인 것은* 사실 인자를 전달하지 않더라도 

함수 외부의 값을 접근하여 값을 수정하는 것이 가능한 경우도 있다.

다음 코드를 보자.

```python
num = 10

def add_five():
    result = num + 5
    print(result)

add_five()
```

아마 `num 을 정의하지 않았습니다` 등의 에러를 예상할지 모르지만, 위의 코드를 실행하면 `15` 라는 값이 나온다.

> 분명 함수 내부와 함수 바깥은 스코프가 다르기 때문에 서로 다른 변수라고 했었는데?

이제 네임스페이스라는 개념을 이해할 때가 왔다.

위의 코드를 조금 변경해서 다음과 같은 코드가 있다고 하자

```python
num = 10

def add_five(num):
    result = num + 5
    print(result)

add_five()
```

이 코드를 실행하면 이번에는 `num` 매개변수에 인자 전달을 하지 않았다면서

이 함수에는 1개의 인자를 필요로 한다는 에러가 뜬다.

그래서 인자를 던져주자

```python
num = 10

def add_five(num):
    result = num + 5
    print(result)

add_five(20)
```

이러면 당연히 `25` 가 나온다. `num` 은 10 이기도 하고 20 이기도 한걸까?

일단 이런 의문을 가진 채 이번에는 함수 바깥에 있는 변수값을 수정하는 코드를 보자

```python
num = 10

def to_be_five():
    num = 5
    print(num)

to_be_five()
print(num)
```

결과를 예측해보자.

```python
5
10
```

이 경우 함수 정의에서 매개변수의 이름으로 `num` 을 지어주지 않았어도,

함수 내부에서 사용하는 `num` 은 함수 외부에서 사용하는 `num` 과 다른 것으로 간주한다.

이를 두고 함수 내부의 `num` 은 로컬(local) 네임스페이스를 가진다고 하고,

함수 바깥의 `num` 은 글로벌(global) 네임스페이스를 가진다고 한다. 참고사항으로 한가지 알아둘 것은 함수 바깥에 또 함수가 있는 것이 아닌 코드 실행 최상단이기 때문에 글로벌이라고 한다는 점이다.

이와 같이 여기서는 우리가 기존에 알고 있듯 함수 내부의 스코프와 함수 바깥의 스코프가 다르게 작동한다.

하지만 위와 비슷하지만 다르게 작동하는 코드를 보자.

```python
num = 10

def to_be_five():
    print(num) # 딱 이부분만 추가되었다
    num = 5
    print(num)

to_be_five()
print(num)
```

그럼 이번에는 다음과 같은 *에러*가 발생한다. 응? 어째서 에러?

```bash
Traceback (most recent call last):
  File "test04.py", line 8, in <module>
    to_be_five()
  File "test04.py", line 4, in to_be_five
    print(num) # 딱 이부분만 추가되었다
UnboundLocalError: local variable 'num' referenced before assignment
```

바로 추가된 부분인 `print(num)` 이 문제이다. 앞서 봤던 코드 중 이런 게 있었다.

```python
num = 10

def add_five():
    result = num + 5
    print(result)

add_five()
```

여기서 결과가 `15` 가 나온 것을 기억한다면

함수 정의 때 함수 내부에 있는 `num` 은 함수 외부에 있는 `num` 을 가리키는 것을 알 수 있다.

정확히는 *함수 정의 당시에 가장 가까운 위치의 네임스페이스를 가진 변수* 를 가리키는 것이지만

어쨌든 함수 내부에 있는 `num` 은 함수 바깥의 그 녀석이다.

그럼 다시 에러가 난 코드를 보자.

```python
num = 10

def to_be_five():
    print(num) # 딱 이부분만 추가되었다
    num = 5
    print(num)

to_be_five()
print(num)
```

이 코드에서도 마찬가지인데, 함수 내부에서 `num` 을 선언하지 않았기 때문에

`print(num)` 을 호출하는 상황에서 `num` 은 글로벌 네임스페이스의 `num` 이게 된다.

그런데 그 밑에서 `num` 을 수정하려고 한다. 아니다 말이 잘못 되었다. 수정이 아니라 *할당을 하려고 한다*

그래서 다음과 같이 에러를 말하는 것이다.

```bash
UnboundLocalError: local variable 'num' referenced before assignment
```

로컬 네임스페이스의 변수인 `num` 은 할당 이전에 이미 참조된 값이라고.

즉 로컬 네임스페이스의 변수로 `num` 을 `할당` 하려고 하는데, 이미 `num` 이라는 변수명이 글로벌 네임스페이스를 가지는 것이다.

그래서 오해할 수도 있지만 이것은 글로벌 네임스페이스를 가진 변수 `num` 을 *수정하기 때문*이 아니라,

로컬 네임스페이스를 가진 변수 `num` 으로 할당하려고 하기 때문에 발생하는 에러이다.

여기서 알 수 있는 것은 두 가지이다.

> 1. 글로벌 네임스페이스를 가진 변수는 수정할 수 없다
> 2. 글로벌 네임스페이스를 가진 변수를 다시 또 로컬 네임스페이스에서 `할당` 하는 것은 불가능하다.

# global 키워드

엄밀히 말해 글로벌 네임스페이스를 가진 변수는 수정이 가능하다. 하지만 그런 방법은 파이썬에서 권장하는 방식이 아니며,

보다 명확하고 직관적인 코드 이해를 위해서 파이썬은 글로벌 네임스페이스를 가진 변수를 수정할 수 있도록

`global` 키워드를 제공한다.

`global` 키워드를 사용하여 위의 코드는 아래와 같이 하면 에러없이 사용이 가능하다.

```python
num = 10

def to_be_five():
    global num # num 네임스페이스가 글로벌 네임스페이스라는 것을 명시적으로 나타낸다
    print(num)
    num = 5 # global 키워드를 통해 글로벌 네임스페이스의 num 이라는 것을 알고 할당이 아닌 재정의를 한다.
    print(num)

to_be_five()
print(num)
```

추가적으로 `global` 키워드 뿐만 아니라 `nonlocal` 키워드도 사용이 가능하다. 두 키워드의 차이점은 공식문서를 살펴보자. 

하지만 이 방식은 파이썬에서 권장하는 방식이 아니다.

매개변수로 전달한 인자도 아닐뿐더러, `return` 도 없는 함수이기 때문에

대다수의 개발자들은 이 함수의 정의를 직접 살펴보고 어떤 값을 변화시키는지 확인해야 하기 때문이다.

# 마치며

네임스페이스를 통해 함수가 함수 외부의 값에도 접근하어 값을 변경할 수 있다는 것을 알게 되었다.

하지만 이 방법은 되도록 지양하는 것이 좋으며, 함수는 매개변수에 인자를 전달하고 `return` 으로 값을 반환하는 것이

좀 더 바람직하다. 다음 포스팅에서는 왜 그러한지 알아보도록 하자.
