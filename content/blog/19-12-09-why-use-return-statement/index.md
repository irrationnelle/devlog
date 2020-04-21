---
title: return 을 사용하는 이유
date: '2019-12-09T01:01:03.284Z'
draft: false
slug: "python-why-use-return"
category: "Python"
tags:
  - "python"
description: "소설 '폴라리스 랩소디' 에는 다음과 같은 구절이 나온다"
socialImage: ""
---

소설 '폴라리스 랩소디' 에는 다음과 같은 구절이 나온다.

> "정말 다 돌려줄 수 있을까요? 우리가 세상에서 받은 것을 다 돌려주려면 우리는 세상 전체를 볼 줄 알아야 해요.
> 빵 한 조각을 들어올리며 농부와 수레꾼과 방앗간 일꾼과 제빵사와 ... 아아, 이건 끝도 없어요.
> 어쨌든 그 모든 것을 볼 수 있을까요?"

이 구절의 서사적 의미와 철학적 고찰에 대해서는 다음으로 미루자.

내용만을 집중하면 다음과 같은 점을 알 수 있다.

> 농부는 밀을 수확한다. 수레꾼은 밀을 옮긴다. 방앗간에서는 탈곡을 한다. 제빵사는 빵을 굽는다.

우리는 그럼 농부가, 제빵사가 작업을 수행했다는 것을 어떻게 알 수 있을까?

그야 그들이 결과물을 우리에게 **직접 제공**하기 때문이다.

만약 그들이 우리에게 작업물을 제공하지 않고 어딘가 불특정한 장소에다가 던져둔다고 하면 우리에겐 할 일이 생긴다.

1. 정말로 작업을 완료 했는지 알 수가 없다.
2. 때로는 그들의 작업물을 가져다가 쓰려고 할 때 작업물이 없을 수 있다.

여기에는 몇가지 중요한 개념들이 있다. 바로 `완료` 와 `작업물이 없을 수 있다` 라는 점이다.

대략적으로 return 의 목적에 대해 예시를 들어 이야기해봤다.

하지만 비유는 언제나 오해를 불러일으킬 수 있는 법. 이제부터 코드를 보자.

# 함수, 다시

일단은 함수를 사용하는 이유에 대해서 다시 한 번 생각해보자.

학습 단계에서 함수가 값을 return 하는 것이 의미없게 느껴지는 경우 중

첫번째로 함수의 목적이 `재사용` 에 있다는 점이 크게 와닿지 않을 때가 있다.

다음 덧셈 코드를 보자.

```python
# 함수 정의 없이 덧셈 작업
num1 = 10
num2 = 20
result1 = num1 + num2
print(result1)

# 함수를 정의하여 덧셈 작업
def add_numbers(num1, num2):
    return num1 + num2

result2 = add_numbers(10, 20)
print(result2)
```

결과는 당연히 동일하다.

```bash
30
30
```

그럼 함수를 사용해서 `return` 값으로 전달받는 게 뭔가 웃긴다. 그냥 함수 안 사용하고 위에서 하듯 더하면 되잖아?

어차피 같은 결과를 얻을 뿐인데 return 으로 굳이 번거롭게 함수 바깥에다가 값을 전달해주는 의미는 무엇이란 말인가?

하지만 함수는 재사용성에 목적이 있다. 그런 의미에서 함수의 `return` 값은 의미를 가진다. 덧셈을 10번, 20번 반복한다면?

그럼 함수를 10번, 20번 호출하면 되고, 그 반환값을 이용하면 된다.

반대로 말하자면 return 값이 없다면 함수는 재사용이 어려워진다

# 네임스페이스와 mutable , 그리고 side effect

앞서서 [네임스페이스에 대해서](https://rase.dev/19-12-08-namespace-and-scope/) 대략 설명했다.

그리하여 return 값이 없어도 사실 함수는 재사용이 가능하다. 함수 내부에서 로컬 네임스페이스를 가진 변수가 아닌 글로벌 네임스페이스이나 논로컬(nonlocal) 네임스페이스에 접근해서 수정하면 함수 외부의 값이 함수 내부의 작업에 영향을 받기 때문이다.

글로벌 네임스페이스를 가지는 변수를 *수정*하기 위해서는 `global` 이나 `nonlocal` 이라는 키워드가 필요하다고 했지만, 사실 이것은 `immutable` 변수에 한해서만 그렇다.

`mutable` 변수는 `global` 이나 `nonlocal` 같은 키워드가 없어도 함수 내부에서 수정이 가능하다는 의미이다.

다음 예시를 보자

```python
def add_friend():
  new_friend = 'Yena'
  friend_list.append(new_friend)
  
friend_list = ['Louie', 'Tylo', 'Ashley']

print(friend_list)
add_friend()
print(friend_list)
```

```bash
['Louie', 'Tylo', 'Ashley']
['Louie', 'Tylo', 'Ashley', 'Yena']
```

함수 내부에서 글로벌 네임스페이스를 가지는 `friend_list`  리스트에 `Yena` 를 추가할 수 있었다. 추가 뿐 아니라 수정, 삭제까지 가능하다.

```python
def modify_and_delete_friend():
    friend_list[1] = 'Brad'
    friend_list.remove('Louie')
  
  
friend_list = ['Louie', 'Tylo', 'Ashley', 'Yena']

print(friend_list)
modify_and_delete_friend()
print(friend_list)
```

```bash
['Louie', 'Tylo', 'Ashley', 'Yena']
['Brad', 'Ashley', 'Yena']
```

하지만 이런 방식으로 함수를 사용하는 것은 굉장히 안 좋은 방식이다. 부수효과, 즉 side effect 가 발생하기 때문이다.

부수효과는 방금까지 본 것처럼 함수가 *자신이 다룰 수 없는 외부세계에* 영향을 미치는 것을 의미한다.

위 `modify_and_delete_friend` 함수는 어떤 입력도 없다. 또한 출력도 없다. 여기서 말하는 입력은 매개변수를 통해 인자를 받는 것을 의미하며, 출력은 return 으로 값을 반환하는 것을 의미한다.

입출력이 없는 함수임에도 해당 함수를 호출하면 `friend_list` 리스트의 상태가 변하게 된다.

또한 입력은 있지만 출력이 없는 함수도 외부세계에 영향을 미칠 수 있으며 이 역시 부수효과이다. 다음 코드를 보자.

```python
def modify_and_delete_friend(f_list):
    f_list[1] = 'Brad'
    f_list.remove('Louie')
  
friend_list = ['Louie', 'Tylo', 'Ashley', 'Yena']

print(friend_list)
modify_and_delete_friend(friend_list)
print(friend_list)
```

```bash
['Louie', 'Tylo', 'Ashley', 'Yena']
['Brad', 'Ashley', 'Yena']
```

이 코드에서 `modify_and_delete_friend` 함수는 리스트 타입의 인자를 받는다. 입력이 존재하는 함수인 것이다. 하지만 출력이 없는 함수이다. 출력이 없는데도 불구하고, 이 함수를 호출하면 `friend_list` 값이 변하게 된다. 부수효과가 발생한 것이다.

# 부수효과가 야기하는 문제점

사실 **side effect** 는 보통 번역할 때 부작용이라고 많이 이야기한다.

부작용이란 무엇인가, 원래 목적 외의 다른 효과를 나게 하는 것을 말한다. 하지만 부작용이라는 것이 일반적으로 부정적인 의미를 가지고 있는 것을 볼 때, 부작용의 실제적인 의미는 *다른 효과로 인해 기대하지 않았던 결과를 발생시키는 것* 을 이야기할 것이다.

그런데도 사이드 이펙트를 '부작용'이 아닌 '부수효과'라고 이야기하는 것은, 개발 작업에는 코드 작업 뿐만 아니라 네트워크 통신이나 데이터베이스 저장 및 불러오기 등의 작업도 포함이 되기 때문이다. 네트워크 응답이나 데이터베이스 불러오기 같은 것들은 개발자가 *출력값을 예측할 수 없으며* 그 작업이 언제 *완료*되는지 예상하기도 어렵다.

앞서 강조했던 *작업물이 없는 상황(출력값을 예측할 수 없기 때문에)* 과 *완료* 라는 개념이 다시 등장했다. 이 두 가지 개념은 추후에 다시 다른 개발용어로 설명할 수 있을 것이다.

어쨌든 부수효과는 필수 불가결하지만, 개발자가 제어할 수 있는 부분마저도 부수효과를 방치해버리면 다음과 같은 문제가 발생한다.

```python
def func1(n):
    global y
    y = n+1
    print(y)

# 굉장히 많은 코드들이 있다고 가정하자 

def func2(a):
    return y * a

# 굉장히 많은 코드들이 있다고 가정하자 

y = 7

# 굉장히 많은 코드들이 있다고 가정하자 

func1(50)

# 굉장히 많은 코드들이 있다고 가정하자 

result = func2(100)
print(result)
```

일단 이 코드는 부수효과 말고도 문제점이 제법 있는데, 변수명이나 함수이름을 아무 생각없이 지었다는 것을 알 수 있다.

도대체 y가 의미하는 값은 무엇이며 func1 와 func2 가 하는 일이 무엇인지도 알기가 힘들다.

그나마 마지막에 가서는 정신을 차렸는지 result 라는 변수명을 지어주었으니 마지막에는 결과를 print 함수로 출력한다는 것은 알 수 있다.

이 코드를 실행한 사람은 아마 700 이라는 값을 기대하겠지만, 실제로는 5100 이라는 값이 나온다. 깜짝 놀라서 `func2` 함수의 정의를 살펴보았더니 분명 y의 값에 인자값만큼 곱하는 함수이다. 그런데 잘못 작동하고 있다는 건, 어디선가 y 값을 바꾼다는 것인데, 문제는 y 는 7이라는 값을 할당한 이후로 다시 값을 수정하는 일이 없다. 여기서 그나마 상황이 낫다면 y를 인자로 사용하는 함수가 있는 것은 아닌가 찾아볼 수도 있을 것이다. 하지만 y 는 `immutable` 한 변수이기 때문에 인자로 주었다고 한듯 그 값이 바뀔리도 없다. 그렇다면 상황은 한가지이다. 누군가가 부수효과를 발생시키는 코드를 작성했고 그게 y에 영향을 미치는 것이다.

이런 상황이 되면 y 의 값이 어떻게 변하는지 추적하기 위해서 어떻게 해야할까? 바로 모든 함수와 코드를 다 뒤져서 부수효과가 발생하는 지점을 찾아내는 방법 밖에는 없다.

물론 코드를 작성하는 게 익숙해지고 다른 사람의 코드를 읽는 것도 익숙해지면 이런 상황에서 어떤 키워드를 이용해서 코드를 검색하고 어디쯤에서 부수효과가 발생했을지 유추해서 쉽게 찾아낼 수는 있다. 빈번하게 일어나는 일은 아니지만, 굉장히 드문 경우도 아니라는 것이다. 하지만 그렇다고 해서 이렇게 막무가내로 부수효과를 발생시키는 코드를 작성하는 것은 무책임한 행위이다.

하지만 `return` 문을 사용해서 함수들이 부수효과를 가지지 않게 한다면, y의 값을 추적하는 것은 한결 수월해진다. y = func1(50) 이런 식으로 재할당이 이루어지는 지점만 찾으면 되기 때문이다. 여기서 재할당을 한다는 것은 무엇을 의미하는가? **바로 완료가 보장된 함수의 작업물을 직접 건네준다** 이다. 즉 맨앞의 서론에서 했던 개념으로 다시 돌아가서 이해해보자. `return` 문이 없다는 건 **어딘가 불특정한 장소에다가 던져둔** 변수들의 변화를 직접 추적하고 완료 여부까지 확인해야 한다는 것이다.
 
 또한 함수들이 부수효과가 없다면 함수의 역할을 직관적이고 분명하게 나타내는 것이 가능해진다. 예를 들어 부수효과가 없는 함수라면 함수 내부에서 더하고 빼고 곱하는 작업들이 어지럽게 엉켜 있으면 이 작업들을 각각 더하는 함수, 빼는 함수, 곱하는 함수로 분리해서 다룰 수 있다.

# 마치며

이렇듯 함수의 `return` 문을 사용하는 것으로, 개발자는 코드 가독성을 높일 수 있고 함수를 보다 체계적으로 관리하고 조합할 수 있는 역량을 갖출 수 있다.

또한 매개변수와 `return` 문을 활용한 함수 사용법은 훗날 자동화 테스트를 작성하는 데에도 큰 도움이 되며, 리팩토링을 진행하는데에도 훨씬 수월하다는 것을 깨달을 수 있다.

그리고 함수를 보다 체계적으로 관리하고 조합한다는 개념은 *함수형 프로그래밍* 이 지향하는 방법론인데, 함수형 프로그래밍은 `return` 문이 없이는, 즉 출력이 없는 함수로는 결코 불가능하다.

파이썬은 객체지향 프로그래밍 언어이지만, 함수형 프로그래밍의 몇몇 요소들을 가지고 있는 언어이기도 하기 때문에 이런 `return` 에 사용과 목적에 대해서 확실하게 알아두는 것이 분명 큰 도움이 될 것이다. 