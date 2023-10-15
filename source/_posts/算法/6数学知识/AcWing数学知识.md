

## 埃氏筛优化 nloglogn

```python
n = int(input()) + 1
st = [False] * (n)
cnt = 0
for i in range(2,n):
    if not st[i]:
        for j in range(i + i, n, i):
            st[j] = True
        cnt += 1
print(cnt)
```

## 线性筛

```python
n = int(input()) + 1
st = [False] * n
primes = [0] * n
cnt = 0
for i in range(2,n):
    if not st[i]:
        primes[cnt] = i
        cnt += 1
    # 用已有的质数与当前因子的乘积(一定不是质数)筛
  
    j = 0
    while primes[j] <= (n-1)/i:
        st[primes[j] * i] = True
        # 保证每个数只筛一次
        if i % primes[j] == 0:
            break
        j = j + 1
    
print(cnt)
```

