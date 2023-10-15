---
title: 数据结构-力扣
tags:
  - leetCode
categories:
  - 算法
  - 3数据结构
date: 2022-9-02 11:51:56
---




## [547. 省份数量](https://leetcode.cn/problems/number-of-provinces/)

难度中等873

有 `n` 个城市，其中一些彼此相连，另一些没有相连。如果城市 `a` 与城市 `b` 直接相连，且城市 `b` 与城市 `c` 直接相连，那么城市 `a` 与城市 `c` 间接相连。

**省份** 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 `n x n` 的矩阵 `isConnected` ，其中 `isConnected[i][j] = 1` 表示第 `i` 个城市和第 `j` 个城市直接相连，而 `isConnected[i][j] = 0` 表示二者不直接相连。

返回矩阵中 **省份** 的数量。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

```
输入：isConnected = [[1,1,0],[1,1,0],[0,0,1]]
输出：2
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

```
输入：isConnected = [[1,0,0],[0,1,0],[0,0,1]]
输出：3
```

 

**提示：**

- `1 <= n <= 200`
- `n == isConnected.length`
- `n == isConnected[i].length`
- `isConnected[i][j]` 为 `1` 或 `0`
- `isConnected[i][i] == 1`
- `isConnected[i][j] == isConnected[j][i]`



```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        UF uf = new UF(n);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (isConnected[i][j] == 1) {
                    uf.union(i,j);
                }
            }
        }
        return uf.sz;

    }
}
class UF {
    public int[] id;
    public int sz; // 维护有几个
    // 只有根的值 id[i] = i
    public UF(int n) {
        id = new int[n];
        sz = n;
        for (int i = 0; i < n; i++) {
            id[i] = i;
        }
    }
    public void union(int a, int b) {
        if (! connected(a,b)) sz--;
        int i = root(a);
        int j = root(b);
        id[i] = j;
    }
    public boolean connected(int a, int b) {
        return root(a) == root(b);
    }
    public int root(int a) {
        while (id[a] != a) {
            id[a] = id[id[a]];
            a = id[a];
        }
        return a;
    }
}
```

## 