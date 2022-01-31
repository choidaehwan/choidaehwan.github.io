---
layout: single
title: "암호화"
toc: true
categories: TIL
tag: [Computer Science]

---


[노마드 코더 - 암호화](https://www.youtube.com/watch?v=67UwxR3ts2E)
[암호화 블로그](https://velog.io/@jinatra/Bcrypt-JWT)
[안전한 패스워드 저장](https://d2.naver.com/helloworld/318732)

데이터베이스에 비밀번호값을 쌩으로 넣어버리면 개발자나 직원들이 볼 수 있게 때문에 그렇게 넣으면 안된다. 비밀번호는 항상 암호화하여서 데이터베이스에 넣어줘야한다

### hash 함수
해쉬 함수는 입력한 값을 랜덤한 값으로 치환시켜준다
12345 -> hash F(x) -> 97gn!%#!0gh42o48@$n9W@$#glw138g#gt8*4*

해쉬함수 특징
1.hash 함수는 동일한 input에 대한 동일한 output을 가지고 있음. 
즉, 입력값이 바뀌지 않으면 출력값도 바뀌지 않음

2.input이 조금만 변경되어도 output은 어마어마하게 달라져서 나옴
12345 -> hash F(x) -> 97gn!%#!0gh42o48@$n9W@$#glw138g#gt8*4*
1234  -> hash F(x) -> @hg3#Glglsl#NLJ@!#nkjfg3#kn@#$ljnf3t51

3.hash 함수는 항상 같은 방향(단방향)으로만 움직인다
12345 -> hash F(x) -> 97gn!%#!0gh42o48@$n9W@$#glw138g#gt8*4*
~~97gn!%#!0gh42o48@$n9W@$#glw138g#gt8*4* -> hash F(x) -> 12345~~

이렇게 데이터 베이스에 넣으면 개발자나 직원들도 데이터베이스에서 비밀번호를 볼 수 없다.

*다이제스트: 해시 함수라는 수학적인 연산을 통해 생성된 암호화된 메시지를 의미

### salt 함수
>![](https://images.velog.io/images/gigymi2005/post/1c8fd011-b039-48b9-adcb-30a4738cf473/image.png)
<p style=font-size:15px;>이미지 참조: https://velog.io/@jinatra/Bcrypt-JWT</p>

하지만 이렇게만 넣으면 레인보우 테이블 공격(Rainbow Table Attack)이라고 불리는 해킹에 취약하다.
여기서 레인보우 테이블은 input값과 hash된 output값을 연결하여 저장해놓은 테이블로 이걸 활용해서 해커들이 비밀번호를 해킹할 수 도 있다.

Salt는 아주 작은 랜덤한 텍스트인데, 유저가 계정을 만들면 그 패스워드를 salt와 함께 hash하면 아주 랜덤한 결과값을 만들 수 있다.
salt = KVJ$33!

12345 -> hash F(x) + salt -> $97gn!%#!0gh42K3o48@$n9W@$V#glw138g#gt8*4*3

이렇게 하면 레인보우 테이블을 가지고 있는 해커로부터 보호할 수 있다.

### key Stretching
>![](https://images.velog.io/images/gigymi2005/post/95ec686e-3e91-41b0-9d89-59622f56920d/image.png)
<p style=font-size:15px;>이미지 참조: https://velog.io/@jinatra/Bcrypt-JWT</p>
입력한 패스워드의 다이제스트를 생성하고, 생성된 다이제스트를 입력 값으로 하여 다이제스트를 생성하고, 또 이를 반복하는 방법으로 다이제스트를 생성할 수도 있다. 이렇게 하면 입력한 패스워드를 동일한 횟수만큼 해시해야만 입력한 패스워드의 일치 여부를 확인할 수 있다. 이것이 기본적인 키 스트레칭 과정이다.
최근에는 일반적인 장비로 1초에 50억 개 이상의 다이제스트를 비교할 수 있지만, 키 스트레칭을 적용하여 동일한 장비에서 1초에 5번 정도만 비교할 수 있게 한다

키를 늘린다. 즉, 여러번 반복한다는 의미
단방향 해쉬값을 계산 한 후 그 해쉬값을 또 해쉬 하고, 또 이를 반복하는 것
Key Stretching을 한다면 같은 시간에 컴퓨터가 비교할 수 있는 횟수가 기하급수적으로 줄어들어 보안에 효과적이다.

### bcrypt
Salting과 Key Stretching을 구현한 함수로써, 처음부터 비밀번호를 단방향 해쉬화하기 위해서 만들어졌다.

### Bcrypt를 통한 암호화 과정
대부분의 프로그래밍 언어에서 라이브러리를 사용할 수 있음
bcrypt 라이브러리의 내장함수(bcrypt.hashpw)를 이용해 암호화하고자 하는 정보를 암호화시킬 수 있다.

```python
# pip install bcrypt (in terminal)

import bcrypt

password = 'dontkillmyvibe'
hashed_password = bcrypt.hashpw(password.encode('utf-8'), bcrypt.gensalt())

print(hashed_password)

>>> b'$2b$12$itUvCIfHC0YAjNfjpBjFlOtAv1fPDiiALuuQqlI.CkwUVg863JAF6'
```

`import bcrypt` : 먼저 bcrypt 라이브러리를 설치한 후 파이썬에 들어가 import
`hashed_password = bcrypt.hashpw(password.encode('utf-8'), bcrypt.gensalt())` : 해싱을 위해 password를 bytes 타입으로 인코드해주고, salt를 추가

bcrypt의 경우엔 str이 아닌 bytes 데이터를 암호화 하기에 항상 bytes 타입으로 지정이 되었는지 확인해주어야 한다.

### Adaptive Key Derivation Functions
adaptive key derivation function은 다이제스트를 생성할 때 솔팅과 키 스트레칭을 반복하며 솔트와 패스워드 외에도 입력 값을 추가하여 공격자가 쉽게 다이제스트를 유추할 수 없도록 하고 보안의 강도를 선택할 수 있다.

이 함수들은 GPU와 같은 장비를 이용한 병렬화를 어렵게 하는 기능을 제공한다. 이와 같은 기능은 프로그램이 언어에서 제공하는 라이브러리만으로는 구현하기 어렵다.

bcrypt

PBKDF2
PBKDF2는 NIST(National Institute of Standards and Technology, 미국표준기술연구소)에 의해서 승인된 알고리즘이고, 미국 정부 시스템에서도 사용자 패스워드의 암호화된 다이제스트를 생성할 때 사용한다.

scrypt
scrypt는 PBKDF2와 유사한 adaptive key derivation function이며 Colin Percival이 2012년 9월 17일 설계했다. scrypt는 다이제스트를 생성할 때 메모리 오버헤드를 갖도록 설계되어, 억지 기법 공격(brute-force attack)을 시도할 때 병렬화 처리가 매우 어렵다. 따라서 PBKDF2보다 안전하다고 평가되며 미래에 bcrypt에 비해 더 경쟁력이 있다고 여겨진다. scrypt는 보안에 아주 민감한 사용자들을 위한 백업 솔루션을 제공하는 Tarsnap에서도 사용하고 있다. 또한 scrypt는 여러 프로그래밍 언어의 라이브러리로 제공받을 수 있다.


