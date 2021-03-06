---
title: list 이해하기
date: '2019-12-16T01:01:03.284Z'
template: "post"
draft: false
slug: "python-list"
category: "Python"
tags:
  - "python"
description: "얼마 전 스터디에서 개별 변수를 사용하지 않고 리스트를 굳이 왜 사용해야 하느냐는 질문을 받았다"
socialImage: ""
---

얼마 전 스터디에서 개별 변수를 사용하지 않고 리스트를 굳이 왜 사용해야 하느냐는 질문을 받았다.

# Why should we use List?

곧 크리스마스 시즌이다. 크리스마스에 할 일도 만날 사람도 있는 Louie 는 이번 크리스마스는 좀 색다르게 지내고자 크리스마스 파티를 계획한다.

파티에 사람을 초대하기 위해 크리스마스에서 혼자서 궁상 떨만한 사람들에게 연락을 돌리려고 한다.

전화번호가 가득 적혀있는 수첩을 펼쳐 전화를 걸려는데, 앗 이제보니 펜이 없다. 파티 참석자 목록 작성은 펜을 찾을 때까지 미루기로 한다.

결국 전화번호가 있는 수첩 귀퉁이를 접는 것으로 참석 여부를 나누기로 결정했다.

일단 가장 먼저 떠오르는 Claude 에게 연락을 한다. 할 일이 당연히 없는 그였기에 파티에 참석하기로 한다. 수첩 귀퉁이를 접자.

이번에는 Ta-Gong 에게 연락한다. 예상대로 선약이 있다고 한다. 전화를 끊고 Tylo 에게 연락을 하니 역시나 참석 확정이다. 또다시 귀퉁이를 접자.

그렇게 스무명에게 연락을 돌렸다. 그리고 다음날, Ta-Gong 에게 연락이 와서 아마 참석이 가능할 거 같다고 한다. 부랴부랴 연락처가 적힌 수첩 페이지로 가서 귀퉁이를 접었다.

그러면 총 몇명이나 오는거지? 라고 생각하며 수첩 귀퉁이를 하나씩 세어본다. 그러는 사이 Tylo 에게 연락이 와서 갑자기 참석이 힘들 거 같다고 이야기한다.

다시 또 연락처가 적힌 페이지로 가서 접힌 귀퉁이를 펴준다. 그렇게 그날 파티 참석에 대해 여러 사람들이 번복한다. 접었다 폈다가를 반복하면서 수첩은 너덜너덜해진다.

어쨌든 파티 참석자가 정해졌으니, 총 몇명이나 오는지 세어보려고 한다. 귀퉁이가 접힌 부분을 세는 중에 Claude 에게 연락이 온다.

이 친구는 절대로 파티에 참석하지 않는다고 하지 않을텐데? 라고 생각하며 전화를 받으니, 파티에 오는 사람이 누구누구있는지 알려달라고 부탁한다.

잠시만 기다려 달라고 하며 수첩 페이지를 여기저기 옮기며 이름을 불러주니, 왜 이렇게 느리게 알려주냐며 상대방이 뭐라고 한다.

그러더니만 수첩에는 없는 친구들의 이름을 몇몇 말하더니 이 친구들도 함께 갈거라고 이야기한다. 여전히 펜이 없으므로 수첩 귀퉁이만 일단 접은 채로 펜을 찾는대로 거기에다 이름을 넣으려고 한다.

그런데 이런 전화들이 또 여러개가 온다. 그 친구들의 이름을 다 기억하지 못할 것을 직감한 Louie 는 결국 밖으로 나가 펜을 구입한다.

펜으로 크리스마스 파티 참석자 목록을 바라보며 몇명이 오는지, 누가 오는지 확연하게 알 수 있게 되었다. 또 뒤늦게 못 갈 거 같다고 연락오는 친구의 이름에는 빗금을 살짝 쳐주는 것으로 목록에서 제거하는 것이 가능해졌다. 편하다!

# 코드로 보는 list

다소 과장된 위의 이야기는 단순히 개별적인 변수로 값들을 저장하는 것보다 리스트를 사용하는 편이 더 나은 경우이다.

변수를 개별적으로 사용하는 것은 변수를 일일히 선언하고 정의하여 값을 할당해주어야 하는 문제가 발생한다.

```python
name_1 = 'Claude'
name_2 = 'Ta-Gong'
name_3 = 'Tylo'
# 이런 식으로
```

게다가 위의 경우처럼 수정 사항이 생길 때 마다 해당하는 변수를 직접 찾아야 하는 경우가 생긴다.

예를 들어 `Tylo` 라는 값을 가지고 있는 변수를 찾는다고 해보자.

```python
name_1 = 'Claude'
name_2 = 'Ta-Gong'
name_3 = 'Tylo'

if name_1 == 'Tylo':
    print('name_1 is Tylo')

if name_2 == 'Tylo':
    print('name_2 is Tylo')

if name_3 == 'Tylo':
    print('name_3 is Tylo')
```

`Tylo` 란 이름을 찾는 것만으로도 벌써 조건문을 세번이나 사용해야 했다. 게다가 만약 이름을 할당하는 변수들이 늘어난다면 조건문은 그 수만큼 더 늘어나게 된다.

하지만 `list` 를 이용하면 반복문을 사용하여 아주 쉽게 찾을 수가 있다.

```python
name_list = ['Claude', 'Ta-Gong', 'Tylo']

for name in name_list:
    if name == 'Tylo':
        print('Tylo is here')
``` 

또한 'Tylo'를 목록에서 지우는 것도 아주 쉽다.

```python
name_list = ['Claude', 'Ta-Gong', 'Tylo']
print(name_list)

name_list.remove('Tylo')
print(name_list)
```

```bash
['Claude', 'Ta-Gong', 'Tylo']
['Claude', 'Ta-Gong']
```

그런데 뭔가 이상한 것이 보인다. 지금까지 보아오던 함수와 약간 모양새가 다른 함수인 `name_list.remove('Tylo')` 가 있다. 

`name_list` 는 친구 이름 리스트인데 함수처럼 사용한다고? 게다가 `remove` 라는 함수는 우리가 정의내린 적이 없는 함수이다.

이전에 봤던 예시 중에 `friend_list.append(new_friend)` 라는 것도 있었다. 이것의 성격 역시 `remove()` 와 성격이 비슷하다.

이처럼 `append()` 나 `remove()` 같은 함수는 `메소드(method)` 라고 부른다. 

일반 함수와 다른 점은, 이 메소드들은 `list` 라는 **자료형태**, 즉 *리스트(list) 타입에 종속되어있는 함수*이다.

리스트 타입에 종속되어 있기 때문에 이 메소드들은 list 타입에서만 사용이 가능하다. 즉 `num = 10` 이라고 선언한 변수 `num` 이 있다고 할 때, `num.append(1)` 나 `num.remove(10)` 와 같은 방식으로는 사용할 수 없다는 의미이다. 변수 `num` 은 정수 타입이지 리스트 타입이 아니기 때문이다. `band_list = ['opeth', 'coldplay', 'radiohead']` 라는 리스트 타입의 변수가 있을 때 `band_list.append('nell')` 처럼은 사용이 가능하다.

# 마치며

리스트의 개념과 사용 목적에 대해 대략적으로 알아보았다. 리스트의 메소드들은 위에서 이야기 한 두 메소드 뿐만 아니라 상당히 다양하게 있으며, 또한 다수의 리스트를 사용해서 새로운 리스트를 만들어내는 것 역시 가능하다. 하지만 리스트에 익숙해지기 위해서는 앞서 예시들에도 봤듯 반복문과 조건문도 능숙하게 사용할 줄 알아야 한다. 반복문과 조건문 없이 리스트를 사용한다면 리스트의 값을 다루는 데 한계가 많기 때문이다. 따라서 이후에는 리스트의 값을 좀 더 복잡하게 조작하고 가공하는 법을 익혀볼 예정이다.

