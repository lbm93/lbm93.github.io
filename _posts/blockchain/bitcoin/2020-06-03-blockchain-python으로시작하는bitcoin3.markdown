---
title: "Python으로 시작하는 BitCoin [3탄]"
date: 2020-05-27 20:00:00 -0900
categories: bitcoin
tags: 
  - blockchain
  - bitcoin
  - 비트코인
  - 블록체인
lastmod: 2019-01-26 19:00:00 -0900
---

# 목록

1. [**Python으로 시작하는 BitCoin [1탄]-#1.개발환경 구성**](https://lbm93.github.io/bitcoin/blockchain-python으로시작하는bitcoin1/)
2. [**Python으로 시작하는 BitCoin [2탄]-#2.유한체란?**](https://lbm93.github.io/bitcoin/blockchain-python으로시작하는bitcoin2/)
3. [**Python으로 시작하는 BitCoin [3탄]-#3.유한체 코딩**](https://lbm93.github.io/bitcoin/blockchain-python으로시작하는bitcoin3/)  


# 시작하기 앞서
이번시간에는 저번 시간에 알아본 유한체를 코딩해본다. 유한체의 덧셈 ,뺄셈 ,나눗셈, 곱셈, 거듭제곱 까지 유한체의 연산식을 구현한다.

# 유한체 클래스 생성
파이썬으로 유한체 원소를 표현하기 위해서 먼저 유한체 원소 1개를 표현하는 클래스를 정의한다. 클래스 이름은 FieldElement이고 파일 명은 ecc.py로 만든다.여기서 만드는 클래스는 유한체 F(prime)에 있는 원소를 나타낸다. 클래스의 기본 뼈대는 다음과 같다.

```python
# tag::source1[]
class FieldElement:

    def __init__(self, num, prime):
        if num >= prime or num < 0:  # <1>
            error = 'Num {} not in field range 0 to {}'.format(
                num, prime - 1)
            raise ValueError(error)
        self.num = num  # <2>
        self.prime = prime

    def __repr__(self):
        return 'FieldElement_{}({})'.format(self.prime, self.num)

    def __eq__(self, other):
        if other is None:
            return False
        return self.num == other.num and self.prime == other.prime  # <3>
    # end::source1[]
```

코드 설명은 다음과 같다.  

```text
#<1> : 먼저 num과 prime을 인수로 받은 후 num 값이 경계값을 포함하여 0과 prime-1 값인지 조사한다. 그렇지 않으면 에러를 발생한다.  

#<2> : __init__ 메서드의 나머지 부분에서 조사된 인수 값으로 객체를 초기화 한다.  

#<3> : __eq__ 메서드는 FieldElement 클래스의 두 개체가 같은지 검사한다. 객체의 num과 prime 속성이 서로 같은 경우에만 True값을 반환한다.  
```

위에서 정의한 클래스는 아래와 같이 사용할 수 있다.

```python
from ecc import FieldElement
a = FieldElement(7,13)
b = FieldElement(6,13)

print(a==b) #False
print(a==a) #True
```

파이썬에서는 FieldElement 객체 간 등호 연산자(==)를 __eq__ 메서드를 통해 정의할 수 있었다.  
이를 **연산자 재정의**라고 한다. 앞으로도 다른 연산자(곱셈, 나눗셈, 덧셈, 뺄셈, 거듭제곱)를 재정의한다.  

# 연습문제 1.1
FieldElement의 두 객체가 서로 다른지 검사하는 != 연산자를 재정의 하도록 __ne__ 메서드를 만들어라  

```python
def __ne__(self,other):
    return not(self==other)
```
# 나머지 연산
나머지 연산으로 덧셈, 뺄셈, 곱셈, 나눗셈에 대해 닫혀 있는 유한체를 만들 수 있다. 나머지 연산에 대한 결과는 항상 나누는 수 의 범위 안에서 나오기 떄문이다.  

예를들어,  
  
13 % 60 = 13   
61 % 60 = 1 
99 % 60 = 39
  
이렇게 아무리 큰 숫자라도 나머지연산 후 비교적 작은 범위의 숫자로 변환되기 때문에 이것은 숫자 개수가 한정되어 있는 유한체에서 매우 적절한 속성이 된다.  

이제 다음 포스팅에서는 이 나머지 연산으로 유한체에서의 덧셈 뺄셈 나눗셈 곱셈 거듭제곱을 재정의 해본다.  

# 참고문헌
- *Programming Bitcoin - Jimmy Song*



