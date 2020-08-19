---
title: "Python으로 시작하는 BitCoin [1탄]"
date: 2020-05-27 20:00:00 -0900
categories: bitcoin
tags: 
  - blockchain
  - bitcoin
  - 비트코인
  - 블록체인
lastmod: 2019-01-26 19:00:00 -0900
---

**BitCoin의 코어 프로그래밍을 하기 위해서 시작하는 포스팅으로 책 [Programming BitCoin](https://book.naver.com/bookdb/book_detail.nhn?bid=16242884) 을 기반으로 작성되었으며 언어는 Python으로 시작한다.**    
    
# 목록

1. [**Python으로 시작하는 BitCoin [1탄]-#1.개발환경 구성**](https://lbm93.github.io/bitcoin/blockchain-python으로시작하는bitcoin1/)
2. [**Python으로 시작하는 BitCoin [2탄]-#2.유한체란?**](https://lbm93.github.io/bitcoin/blockchain-python으로시작하는bitcoin2/)
3. [**Python으로 시작하는 BitCoin [3탄]-#3.유한체 코딩**](https://lbm93.github.io/bitcoin/blockchain-python으로시작하는bitcoin3/)

  
# 이번 포스팅의 목적  

이번 포스팅은 BitCoin을 시작하기 위해서 파이썬을 설치하고 pyenv로 파이썬을 버전별로 쉽게 관리하고 virtualenv를 실행하는 것 까지 
  
  
# mac에서 Python3설치  

기존에 mac에는 python2가 설치되어있다. 우리는 Python3를 설치한다.

```bash
$brew python
```

설치가 완료되면 아래처럼 버전을 출력해본다.

```bash
$python3 -V
Python 3.8.1
```

이러면 설치가 완료되었다.

# Python 패키지를 관리하는 pip  

Python2의 패키지를 관리해주는 pip이 있다면 Python3의 패키지를 관리해주는 pip3이 있다. 꼭 Python3의 패키지를 설치하려면 **pip3**를 사용해야한다.

```bash
$pip3 -V
pip 9.0.1 from /Users/byungmin/.pyenv/versions/3.6.2/lib/python3.6/site-packages (python 3.6)
```

요런식으로 나온다. 지금 pip3는 python 3.6의 패키지를 관리한다.  

# Python3를 Python으로 별명으로 칭하기  

Python2는 잘 사용하지 않고 없어지는 추세이기도 하다. 그러므로 Python3를 Python으로 바꿀 수 있도록 하고 이때 pip3도 pip으로 바꿔주면 좋다.

```bash
$echo "alias python=/usr/local/bin/python3" >> ~/.zshrc
$echo "alias pip=/usr/local/bin/pip3" >> ~/.zshrc
```

환경변수를 추가하고 쉘을 다시 시작한다.

```bash
$exec "$SHELL"
```

# Pyenv로 Python3 여러버전 관리하기  

먼저 Homebrew를 이용해서 Pyenv를 설치한다.

```bash
$brew install pyenv
```

다음 Pyenv가 활성화 되도록 ~/.zshrc에 환경변수를 추가한다.  

```bash
$echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bash_profile
```

추가했으면 다시 쉘을 시작한다.  

```bash
$exec "$SHELL"
```
  
**pyenv install** 명령으로 여러가지 Python 버전을 설치해본다.  

```bash
$pyenv install 3.6.2
$pyenv install 3.8.1
```
  
그리고 **pyenv versions** 명령으로 설치한 파이썬 버전을 확인한다.

```bash
$pyenv versions
  system
* 3.6.2 (set by /Users/byungmin/.pyenv/version)
  3.8.1
```

이렇게 여러가지 파이썬 버전을 관리할 수 있다.  

**pyenv global** 명령으로 자유자재로 파이썬 버전별로 넘나들 수 있다.  

```bash
$pyenv global 3.6.2
$python -V
Python 3.6.2
$pyenv global 3.8.1
$python -V
Python 3.8.1
```

*여기서 중요한 건 위에서 Python3를 Python으로 바꾸었는데 Pyenv가 관리하는 Python3.xx 여러 버전들까지 Python으로 바뀐 건 아니니 주의!! 물론 pip과 pip3도 마찬가지 !!*

# Virtualenv 설치
Virtualenv를 통해서 가상환경을 설치하고 각각 가상환경마다 pip을 모듈을 독립적으로 운행할 수 있다. 그러므로 현재 컴퓨터의 환경과 다른 독립적인 파이썬 환경을 구성할 수 있다.  

설치는 **brew**를 이용해서 **pyenv-virtualenv**를 설치한다.

```bash
$brew install pyenv-virtualenv
```

다음으로는 활성화가 되도록 환경변수를 추가한다.  

```bash
$eval "$(pyenv virtualenv-init -)" >> ~/.zshrc
```

다음으로는 쉘을 다시 시작한다.  

```bash
$source ~/.bash_profile
```
이렇게 설치는 끝났다. 이제 가상환경을 만들어 본다.  

# 가상환경 생성

가상환경은 아래 명령으로 생성이 가능하다.

```bash
$pyenv virtualenv [python version] [virtualenv name] 
```
```bash
$pyenv virtulenv 3.6.2 bitcoin
```

# 가상환경 들어가기/나가기

가상환경을 만들었으면 가상환경을 수동적으로 들어가고 나가야 한다.  

**가상환경 들어가기**  
```bash
$pyenv activate [virtualenv name]
```

가상환경에 들어서면 다음과 같이 쉘 아이콘 앞에 가상환경 명이 생긴다.  

![그림](/assets/images/img/blockchain-bitcoin/가상환경진입.png)
  
**가상환경 나가기**
```bash
$pyenv deactivate
```

# Jupyter notebook 설치

Jupyter notebook 설치는 brew로 설치해야한다.  

```bash
$brew install jupyter
```

Jupyter notebook 실행은 다음 명령으로 수행한다.  

```bash
$jupyter notebook
```
  
  

# 참고문헌  
- *programming Bitcoin - Jimmy Song*



