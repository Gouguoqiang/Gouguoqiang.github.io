---
title: pyhonAPI刷题笔记
tags:
  - python
  - 算法
categories:
  - 算法
  - 7刷题笔记
date: 2022-9-02 11:51:56
---

# API

## # Queue

```python
# SPFA
from queue import Queue
n, m = map(int, input().split())
inf = 0x3f3f3f3f
dist = [inf] * (n + 1)
g = [[] for _ in range(n + 1)]

for _ in range(m):
    x, y, z = map(int, input().split())
    g[x] += [(y,z)]
    
dist[1] = 0
st = [False] * (n + 1)

q = Queue()
q.put(1)
st[1] = True

while not q.empty():
    t = q.get()
    st[t] = False
    
    for ne in g[t]:
        b, w = ne
        if dist[b] > dist[t] + w:
            dist[b] = dist[t] + w
            if not st[b]:
                q.put(b)
                
if dist[n] > inf / 2: print("impossible")
else: print(dist[n])
```



## # heapq

```python
# dijkstra 堆优化
import heapq
n, m = map(int, input().split())
inf = 0x3f3f3f3f
g = [[] for _ in range(n + 1)]
for _ in range(m):
    x, y, z = map(int, input().split())
    g[x] += [(y,z)]
    
st = [False] * (n + 1)
dist = [inf] * (n + 1)
dist[1] = 0
q = []

heapq.heappush(q,(0,1)) #  默认第一个元素进行从小到大排序, 所以存储距离

while q:
    w, t = heapq.heappop(q)
    
    if st[t]: continue
    st[t] = True
    
    for ne in g[t]:
        j, d = ne
        if dist[j] > dist[t] + d:
            dist[j] = dist[t] + d
            heapq.heappush(q,(dist[t] + d,j))
            
if dist[n] == inf: print(-1)
else: print(dist[n])
    
    

    
```

## # set()

```python
#离散化
n, m = map(int, input().split())

g = set()
o = []
q = []

for i in range(n):
    x, c = map(int, input().split())
    o += [(x,c)]
    g.add(x)
    
for j in range(m):
    l, r = map(int, input().split())
    g.add(l)
    g.add(r)
    q +=[(l,r)]
    
a = list(g)
a.sort()

def find(t) -> int:
    l, r = 0, len(a) - 1
    while l <= r:
        mid = l + (r - l) // 2
        if a[mid] >= t:
            r = mid - 1
        else:
            l = mid + 1
    return l
    

lena = len(a) + 1
v = [0] * lena

for i in o:
    x, c = i
    v[find(x)] += c

s = [0] * lena

for i in range(1, lena):
    s[i] = v[i - 1] + s[i - 1]

for i in q:
    l, r = i
    print(s[find(r + 1)] - s[find(l)])
```

