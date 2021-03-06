---
layout: single
title: "leetcode121, 네트워크 계층"
toc: true
categories: TIL
tag: [Algorithmn, Network]
---

## 오늘 공부한 것
#### leetcode 121 - Best Time to Buy and Sell Stock
내가 푼 풀이
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = []
        
        for i, price in enumerate(prices):
            if i == 0:
                profit.append(-price)
                
            elif i > 0:
                profit.append(price - min(prices[:i]))
    
        return max(profit) if max(profit) > 0 else 0
```

정답
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        min_price = sys.maxsize
        
        for price in prices:
            min_price = min(price, min_price)
            profit = max(profit, price - min_price)
        return profit
```

#### 책 "모두의 네트워크" - 네트워크 계층
- 물리계층: 비트열을 전기신호로 바꿔주는 계층. 혹은 그 반대
- 데이터 링크 계층: 같은 네트워크 안의 노드간 데이터(프레임)를 전송하게 하는 계층
- 네트워크 계층: 라우터를 이용해서 네트워크간 통신(패킷)을 가능하게 하는 계층
- IP 주소: 어떤 네트워크의 어떤 컴퓨터인지 구분할 수 있도록 하는 주소
- 라우팅: ip 주소까지 어떤 경로로 데이터를 보낼지 결정하는 것
- 라우팅 테이블: 라우터에 있음. 경로 정보를 등록하고 관리
- ip헤더: 네트워크 계층에서는 ip 헤더를 데이터에 붙이는데 ip 헤더에는 "출발지 ip 주소" 와 "목적지 ip 주소"가 있음
- 패킷: ip헤더 + 데이터
- ipv4 주소: 32비트로 되어 있음 2^32(43억)개 만들 수 있는데 거의 고갈됐다고 함
- ipv6 주소: ipv4가 고갈될까봐 128비트 2*128(240간)개로 고갈날일이 거의 없을거 같음
- 공인 ip 주소: ISP가 제공. 인터넷에 직접 연결되는 컴퓨터나 라우터에 할당
- 사설 ip 주소: 회사나 가정의 랜에 있는 컴퓨터에 할당
- 공인 ip 주소와 사설 ip 주소를 쓰는 이유: ipv4 주소가 거의 고갈되어서 아껴쓸라고
- ip주소 표시하는법: 32비트의 ip주소를 옥텟(8개)으로 나눠서 10진수로 표시 ex) 192.168.1.10
- 네트워크 ID: 어떤 네트워크인지 나타냄. 
- 호스트 ID: 해당 네트워크의 어느 컴퓨터인지 나타냄
- IP주소 클래스: IPv4에서 사용하는 주소 그룹. A,B,C,D,E 5개인데 A,B,C를 주로 사용. B는 멀티캐스트 주소. E는 연구 및 특수용도 주소
- A클래스: 대규모 네트워크 주소에 사용. 네트워크ID가 8비트, 호스트ID가 24비트
- B클래스는 중형 네트워크 주소에 사용. 네트워크ID가 16비트, 호스트ID가 16비트
- C클래스는 소규모 네트워크 주소에 사용. 네트워크ID가 24비트, 호스트ID가 8비트
- 네트워크 주소: 전체 네트워크에서 작은 네트워크를 식별하는데 사용. 호스트ID가 10진수로 0인 주소. 자신의 IP 주소로 설정하면 안됨 ex) 192.168.1.0
- 브로드캐스트 주소: 네트워크에 있는 컴퓨터나 장비 모두에게 한번에 데이터를 전송하는데 사용되는 전용 IP주소. 호스트ID의 마지막 숫자가 255인 주소. 자신의 IP 주소로 설정하면 안됨 ex) 192.168.1.255
- 서브넷팅: 네트워크를 분할하는 것
- 서브넷: 분할된 네트워크. 서브넷팅을 하면 ip주소가 네트워크 ID, 서브넷 ID, 호스트 ID로 나누어지게 됨.
- 서브넷 마스크: 네트워크ID와 호스트ID를 식별하기 위한 값. IP주소의 네트워크 부분만 나타냄. ex) 255.255.0.0(B클래스), 255.255.255.240(28비트 -> 뒤가 11110000, 255.255.255.252(30비트 -> 뒤가 11111100)
- 프리픽스 표기법: 서브넷 마스크를 슬래시(/비트수) 로 나타냄 ex) /16, /24, /28, /30
- 라우터: 서로 다른 네트워크를 연결해주는 장치로 현재의 네트워크에서 다른 네트워크로 패킷을 전송할 수 있도록 함. 허브나 스위치를 사용하면 모두가 동일한 네트워크에 속하게 됨.
- 기본 게이트웨이: 네트워크 출입구. 다른 네트워크로 데이터를 전송하려면 라우터의 ip주소를  설정해야함
- 기본 게이트웨이를 설정하는 이유: 컴퓨터는 네트워크로 데이터를 보낼때 어디로 전송해야하는지 모른다. 그래서 네트워크 출입구를 지정하고 일단은 라우터로 데이터를 전송. 그다음 라우팅테이블에 있는 라우팅 경로 정보를 기반으로 라우팅함
![KakaoTalk_Photo_2022-01-30-20-44-19](https://user-images.githubusercontent.com/74276716/151698256-38018035-21be-4945-b002-2827f713b090.jpeg)
- 자동으로 라우팅 테이블 등록하는법: 라우터 간에 경로 정보를 서로 교환하여 테이블 정보를 자동으로 수정
- 라우팅 프로토컬: 라우터 간의 라우팅 정보를 교환하기 위한 프로토콜. 대표적인 라우팅 프로토콜에는 RIP, OSPF, BGP 등이 있음
