---
layout: single
title: "Cap@ 교육 4일차, 백준 1260 - DFS BFS"
toc: true
categories: TIL
tag: [Algorithmn]
---

# 📚오늘의 공부📚
## ✅ cap@ 교육 4일차
#### 모의면접
cap@교육을 같이들었던 동기들과 모의면접을 하였다.
![](https://images.velog.io/images/gigymi2005/post/356677d5-66ee-4a3e-92e7-2c9d6dbcb869/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-02-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.21.40.png)
이런 도움되는 피드백을 받았고 앞으로 더 준비해서 와야겠다는 생각이 들었다.

#### 수업총평
뻔한 수업일꺼라는 나의 생각과 달리  오랜만에 나를 돌아볼 수 있었던 계기가 되었던 수업이었다. 꼭 취뽀해서 이런노력에 보답할수 있도록 해야겠다는 생각이들었다.
## ✅ 백준 1260번: DFS와 BFS
![](https://media.vlpt.us/images/lucky-korma/post/e2ef7ac3-14e6-42e7-a768-224c5f773e29/R1280x0-3.gif)
#### DFS
- DFS는 깊이 우선탐색 
- 현재 정점에서 갈 수 있는 점들까지 들어가면서 탐색
- 스택 또는 재귀함수로 구현
![](https://images.velog.io/images/gigymi2005/post/093b6caf-a870-499e-9934-c559c2d923a0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-02-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.26.43.png)

#### BFS
- BFS는 너비 우선탐색
- 현재 정점에 연결된 가까운 점들부터 탐색
- 큐를 사용해 구현
![](https://images.velog.io/images/gigymi2005/post/1c104f64-4220-4f16-9a2b-29fd025a75a1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-02-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.27.00.png)


#### 백준 1260번
![](https://images.velog.io/images/gigymi2005/post/b0bb6f21-2ba3-45f8-a4cd-e24d998ea5d9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-02-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.28.56.png)


```python
import sys
from collections import deque

n, m, v = map(int, sys.stdin.readline().split())
graph = [[] for _ in range(n + 1)]
visited = [False] * (n + 1)  # 정점의 숫자 + 1만큼 만듬

for _ in range(m):
    a, b = map(int, sys.stdin.readline().split())
    graph[a].append(b)
    graph[b].append(a) # graph [[], [2, 3, 4], [1, 4], [1, 4], [1, 2, 3]]


# 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문
for i in range(1, n + 1):
    graph[i].sort()


# 깊이 우선 탐색(재귀를 통한 구현)
def dfs(n):
    print(n, end=' ')
    visited[n] = True  # visited [False, False, False, False, False]
    for i in graph[n]:
        if not visited[i]:
            dfs(i)


# 너비 우선 탐색
def bfs(n):
    visited[n] = True
    queue = deque([n])
    while queue:    # 큐가 비어있지 않다면 반복 실행
        v = queue.popleft()
        print(v, end = ' ')
        for i in graph[v]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True


dfs(v)
visited = [False] * (n + 1)
print()
bfs(v)
```

# 🎯하루 회고🎯
cap@교육이 드디어 끝났다!!
이제 진짜 본격적으로 면접준비를 철저히만 하면된다.
확실히 블로그를 쓰다보니 오늘 한 내용들이 쭉 정리되는 느낌이다.
매일 꾸준하게 성장하는 개발자가 되자🔥🔥


[참조 영상](https://www.youtube.com/watch?v=-wsYtm0x3nw)    
[참고 블로그](https://velog.io/@lucky-korma/DFS-BFS%EC%9D%98-%EC%84%A4%EB%AA%85-%EC%B0%A8%EC%9D%B4%EC%A0%90)
