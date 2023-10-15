---
title: 力扣刷题笔记
tags:
  - 杂记
  - 算法
  - 力扣
categories:
  - 算法
  - 7刷题笔记
date: 2022-9-02 11:51:56
---

# 12.18 || 324场周赛

## 统计相似字符串对的数目

```python
class Solution {
    public int similarPairs(String[] words) {
        int n = words.length;
        int[] map = new int[n];
        for (int i = 0; i < n; i ++) {
            String s = words[i];
            for (int j = 0; j < s.length(); j ++) {
                int a = s.charAt(j) - 'a';
                if ((map[i] & (1 << a)) == 0) {
                    map[i] += (1 << a);
                }
            }
        }
        for (int i = 0; i < n; i++) System.out.println(map[i]);
        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j ++) {
                if (map[i] == map[j]) ans ++;
            }
        }
        return ans;
    }
}
```

## 使用质因数之和替换后可以取到的最小值

```python
class Solution {
    public int smallestValue(int n) {
        int ans = divide(n);
        if (ans == n) return ans;
        return smallestValue(ans);
    }
    
    public int divide(int x) {
        int ans = 0;
        for (int i = 2; i <= x / i; i ++ )
            if (x % i == 0)
            {
                int s = 0;
                while (x % i == 0) {
                    x /= i;
                    s ++;
                }
                ans += i * s;
                
            }
        if (x > 1) ans += x;
        return ans;
    }
}   


```

## \6267. 添加边使所有节点度数都为偶数

```java
class Solution {
    public boolean isPossible(int n, List<List<Integer>> edges) {
        Set<Integer>[] g = new Set[n + 1];
        for (int i = 1; i <= n; i ++) {
            g[i] = new HashSet<>();
        }
        int[] d = new int[n + 1];
        for (List<Integer> edge: edges) {
            int x = edge.get(0), y = edge.get(1);
            g[x].add(y);
            g[y].add(x);
            g[x].add(x);
            g[y].add(y);
            d[x] ++;
            d[y] ++;
        }
        List<Integer> ji = new ArrayList<>();
        for (int i = 0; i <= n; i++) {
            // System.out.println(i + " " + d[i]);
            if (d[i] % 2 == 1) {
                ji.add(i);
            }
        }
        // System.out.println(ji);
        int sz = ji.size();
        // System.out.println(sz);
        if (sz == 0) return true;
        if (sz > 4 || sz % 2 == 1) return false;
        if (sz == 2) {

            int x = ji.get(0), y = ji.get(1);
            if (!g[x].contains(y)) return true;
            for (int i = 1; i <= n; i ++) {
                if (!g[x].contains(i) && !g[y].contains(i)) return true;
            }

            return false;
        }

        int x = ji.get(0), y = ji.get(1);
        int a = ji.get(2), b = ji.get(3);
        int flag = 0;
        if (!g[x].contains(y) && !g[a].contains(b)) return true;
        
        for (int i = 1; i <= n; i ++) {
            if (!g[x].contains(i) && !g[y].contains(i)) {
                flag ++;
                break;
            }
        }
        for (int i = 1; i <= n; i ++) {
            if (!g[a].contains(i) && !g[b].contains(i)) {
                flag ++;
                break;
            }
        }
        if (flag == 2) return true;

        x = ji.get(0);
        y = ji.get(2);
        a = ji.get(1);
        b = ji.get(3);
        flag = 0;
        if (!g[x].contains(y) && !g[a].contains(b)) return true;

        for (int i = 1; i <= n; i ++) {
            if (!g[x].contains(i) && !g[y].contains(i)) {
                flag ++;
                break;
            }
        }
        for (int i = 1; i <= n; i ++) {
            if (!g[a].contains(i) && !g[b].contains(i)) {
                flag ++;
                break;
            }
        }
        if (flag == 2) return true;

        x = ji.get(0);
        y = ji.get(3);
        a = ji.get(1);
        b = ji.get(2);
        flag = 0;
        if (!g[x].contains(y) && !g[a].contains(b)) return true;

        for (int i = 1; i <= n; i ++) {
            if (!g[x].contains(i) && !g[y].contains(i)) {
                flag ++;
                break;
            }
        }
        for (int i = 1; i <= n; i ++) {
            if (!g[a].contains(i) && !g[b].contains(i)) {
                flag ++;
                break;
            }
        }
        if (flag == 2) return true;

        return false;

    }
}
```



# 12.11

连续内存分配

计数

```java
class Allocator {
    int[] a;
    int s;
    int m;
    public Allocator(int n) {
        m = n;
        a = new int[n];
        // map = new HashMap<>();
        
    }
    
    public int allocate(int size, int mID) {
        for(int i = 0; i < m; i++) { 
            if (a[i] == 0) {
                int cnt = -1;
                for (int j = i; j <= m; j++) {
                    cnt ++;
                    if (j == m || a[j] != 0 ) {
                        // if (j == m) cnt --;
                        if (cnt >= size) {
                            for (int k = i; k < i + size; k ++) {
                                a[k] = mID;
                            }
                            return i;
                        }
                        i = j;
                        break;
                    }
                }
            } 
            
        }
        return -1;
        
        
    }
    
    public int free(int mID) {
        int res = 0;
        for (int i = 0; i < m; i++) {
            if (a[i] == mID) {
                res ++;
                a[i] = 0;
            }
        }
        return res;
    }
}

/**
 * Your Allocator object will be instantiated and called as such:
 * Allocator obj = new Allocator(n);
 * int param_1 = obj.allocate(size,mID);
 * int param_2 = obj.free(mID);
 */
```



#  2022.11.20/ 第320场周赛

## \6243. 到达首都的最少油耗

给你一棵 `n` 个节点的树（一个无向、连通、无环图），每个节点表示一个城市，编号从 `0` 到 `n - 1` ，且恰好有 `n - 1` 条路。`0` 是首都。给你一个二维整数数组 `roads` ，其中 `roads[i] = [ai, bi]` ，表示城市 `ai` 和 `bi` 之间有一条 **双向路** 。

每个城市里有一个代表，他们都要去首都参加一个会议。

每座城市里有一辆车。给你一个整数 `seats` 表示每辆车里面座位的数目。

城市里的代表可以选择乘坐所在城市的车，或者乘坐其他城市的车。相邻城市之间一辆车的油耗是一升汽油。

请你返回到达首都最少需要多少升汽油。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2022/09/22/a4c380025e3ff0c379525e96a7d63a3.png)

```
输入：roads = [[0,1],[0,2],[0,3]], seats = 5
输出：3
解释：
- 代表 1 直接到达首都，消耗 1 升汽油。
- 代表 2 直接到达首都，消耗 1 升汽油。
- 代表 3 直接到达首都，消耗 1 升汽油。
最少消耗 3 升汽油。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2022/11/16/2.png)

```
输入：roads = [[3,1],[3,2],[1,0],[0,4],[0,5],[4,6]], seats = 2
输出：7
解释：
- 代表 2 到达城市 3 ，消耗 1 升汽油。
- 代表 2 和代表 3 一起到达城市 1 ，消耗 1 升汽油。
- 代表 2 和代表 3 一起到达首都，消耗 1 升汽油。
- 代表 1 直接到达首都，消耗 1 升汽油。
- 代表 5 直接到达首都，消耗 1 升汽油。
- 代表 6 到达城市 4 ，消耗 1 升汽油。
- 代表 4 和代表 6 一起到达首都，消耗 1 升汽油。
最少消耗 7 升汽油。
```

**示例 3：**

![img](https://assets.leetcode.com/uploads/2022/09/27/efcf7f7be6830b8763639cfd01b690a.png)

```
输入：roads = [], seats = 1
输出：0
解释：没有代表需要从别的城市到达首都。
```

 

**提示：**

- `1 <= n <= 105`
- `roads.length == n - 1`
- `roads[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `roads` 表示一棵合法的树。
- `1 <= seats <= 105`

```java
class Solution {
    int[][] g;
    boolean[] st;
    int[] dist;
    int INF = Integer.MAX_VALUE / 2;
    public long minimumFuelCost(int[][] roads, int seats) {
        // 尽量做一辆车 每个点到达 首都的最短路 前seats个结点只耗这一条路径的油 进行标记
        // 记录每个点走的pre 对每一个点遍历下路径
        // dj后 从最远的开始遍历 遍历到前seate个点移出set不在计算油耗
         
        int n = roads.length() + 1;
        g = new int[n+1][n+1];
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                g[i][j] = g[j][i] = i == j ? 0 : INF;
            }
        }
        for (int[] t : roads) {
            g[t[0]][t[1]] = 1;
            g[t[1]][t[0]] = 1;
        }
        dijkstra(n,k);
        
        
    }
    public void dijkstra(int n, int k) {
        dist = new int[n+1];
        st = new boolean[n+1];
        Arrays.fill(dist,INF);
        dist[k] = 0;
        
        for (int i = 1; i <= n; i++) {
            int t = -1;
            for (int j = 1; j <= n; j++) {
                if (!st[j] && (t == -1 || dist[t] > dist[j])) {
                    t = j;
                }
            }
            st[t] = true;
            for (int j = 1; j <= n; j++) {
                dist[j] = Math.min(dist[j],dist[t] + g[t][j]);
            }
            
        }
        
    }
}
```

## \6244. 完美分割的方案数

给你一个字符串 `s` ，每个字符是数字 `'1'` 到 `'9'` ，再给你两个整数 `k` 和 `minLength` 。

如果对 `s` 的分割满足以下条件，那么我们认为它是一个 **完美** 分割：

- `s` 被分成 `k` 段互不相交的子字符串。
- 每个子字符串长度都 **至少** 为 `minLength` 。
- 每个子字符串的第一个字符都是一个 **质数** 数字，最后一个字符都是一个 **非质数** 数字。质数数字为 `'2'` ，`'3'` ，`'5'` 和 `'7'` ，剩下的都是非质数数字。

请你返回 `s` 的 **完美** 分割数目。由于答案可能很大，请返回答案对 `109 + 7` **取余** 后的结果。

一个 **子字符串** 是字符串中一段连续字符串序列。

 

**示例 1：**

```
输入：s = "23542185131", k = 3, minLength = 2
输出：3
解释：存在 3 种完美分割方案：
"2354 | 218 | 5131"
"2354 | 21851 | 31"
"2354218 | 51 | 31"
```

**示例 2：**

```
输入：s = "23542185131", k = 3, minLength = 3
输出：1
解释：存在一种完美分割方案："2354 | 218 | 5131" 。
```

**示例 3：**

```
输入：s = "3312958", k = 3, minLength = 1
输出：1
解释：存在一种完美分割方案："331 | 29 | 58" 。
```

 

**提示：**

- `1 <= k, minLength <= s.length <= 1000`
- `s` 每个字符都为数字 `'1'` 到 `'9'` 之一。

```java
class Solution {
    public int beautifulPartitions(String s, int k, int minLength) {
        // 记忆化搜索 前i个字符分成一组后 剩下的 分成k-1组
        // f[i][j] 前i个字符分成j组的方案数 
        // 区间dp 
        int n = s.length();
        
    }
}
```





# 2022.11.13/ 第319场周赛

## \6235. 逐层排序二叉树所需的最少操作数目

给你一个 **值互不相同** 的二叉树的根节点 `root` 。

在一步操作中，你可以选择 **同一层** 上任意两个节点，交换这两个节点的值。

返回每一层按 **严格递增顺序** 排序所需的最少操作数目。

节点的 **层数** 是该节点和根节点之间的路径的边数。

 

**示例 1 ：**

![img](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174006-2.png)

```
输入：root = [1,4,3,7,6,8,5,null,null,null,null,9,null,10]
输出：3
解释：
- 交换 4 和 3 。第 2 层变为 [3,4] 。
- 交换 7 和 5 。第 3 层变为 [5,6,8,7] 。
- 交换 8 和 7 。第 3 层变为 [5,6,7,8] 。
共计用了 3 步操作，所以返回 3 。
可以证明 3 是需要的最少操作数目。
```

**示例 2 ：**

![img](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174026-3.png)

```
输入：root = [1,3,2,7,6,5,4]
输出：3
解释：
- 交换 3 和 2 。第 2 层变为 [2,3] 。 
- 交换 7 和 4 。第 3 层变为 [4,6,5,7] 。 
- 交换 6 和 5 。第 3 层变为 [4,5,6,7] 。
共计用了 3 步操作，所以返回 3 。 
可以证明 3 是需要的最少操作数目。
```

**示例 3 ：**

![img](https://assets.leetcode.com/uploads/2022/09/18/image-20220918174052-4.png)

```
输入：root = [1,2,3,4,5,6]
输出：0
解释：每一层已经按递增顺序排序，所以返回 0 。
```

 

**提示：**

- 树中节点的数目在范围 `[1, 105]` 。
- `1 <= Node.val <= 105`
- 树中的所有值 **互不相同** 。

**Solution：**

```java
class Solution {
    public int minimumOperations(TreeNode root) {
		// todo: minimum number of swaps required to sort an Array
    }
}
```



## \6236. 不重叠回文子字符串的最大数目



给你一个字符串 `s` 和一个 **正** 整数 `k` 。

从字符串 `s` 中选出一组满足下述条件且 **不重叠** 的子字符串：

- 每个子字符串的长度 **至少** 为 `k` 。
- 每个子字符串是一个 **回文串** 。

返回最优方案中能选择的子字符串的 **最大** 数目。

**子字符串** 是字符串中一个连续的字符序列。

 

**示例 1 ：**

```
输入：s = "abaccdbbd", k = 3
输出：2
解释：可以选择 s = "abaccdbbd" 中斜体加粗的子字符串。"aba" 和 "dbbd" 都是回文，且长度至少为 k = 3 。
可以证明，无法选出两个以上的有效子字符串。
```

**示例 2 ：**

```
输入：s = "adbcda", k = 2
输出：0
解释：字符串中不存在长度至少为 2 的回文子字符串。
```

 

**提示：**

- `1 <= k <= s.length <= 2000`
- `s` 仅由小写英文字母组成

**Solution：**

```java
class Solution {
    public int maxPalindromes(String s, int k) {
		// 求出各种分割方案的(贪心: 一个回文串的长度尽可能短,满足条件即分割
        // 贪心的证明: 
        // 最大满足条件的子串个数
    }
}
```



# 周赛 318 

## \2460. 对数组执行操作

题目描述

给你一个下标从 **0** 开始的数组 `nums` ，数组大小为 `n` ，且由 **非负** 整数组成。

你需要对数组执行 `n - 1` 步操作，其中第 `i` 步操作（从 **0** 开始计数）要求对 `nums` 中第 `i` 个元素执行下述指令：

- 如果 `nums[i] == nums[i + 1]` ，则 `nums[i]` 的值变成原来的 `2` 倍，`nums[i + 1]` 的值变成 `0` 。否则，跳过这步操作。

在执行完 **全部** 操作后，将所有 `0` **移动** 到数组的 **末尾** 。

```python
# 模拟题
class Solution:
    def applyOperations(self, a: List[int]) -> List[int]:
        n = len(a)
        for i in range(n-1):
            if a[i] == a[i+1]:
                a[i] *= 2
                a[i+1] = 0
        # 学下下面这两句的语法
        b = [x for x in a if x > 0] # 访问数组元素 满足条件才赋值
        b += [0] * (n - len(b)) #扩容
        return b
#降低空间复杂度
class Solution:
    def applyOperations(self, a: List[int]) -> List[int]:
        n = len(a)
        j = 0
        for i in range(0,n-1):
            if a[i] == a[i+1]:
                a[i] *= 2
                a[i+1] = 0
            if a[i] != 0:
                a[j] = a[i]
                j += 1
        if a[n-1] != 0:
            a[j] = a[n-1]
            j += 1
        for i in range(j,n):
            a[i] = 0
        return a
```

## \2461. 长度为 K 子数组中的最大和

给你一个整数数组 `nums` 和一个整数 `k` 。请你从 `nums` 中满足下述条件的全部子数组中找出最大子数组和：

- 子数组的长度是 `k`，且
- 子数组中的所有元素 **各不相同 。**

返回满足题面要求的最大子数组和。如果不存在子数组满足这些条件，返回 `0` 。

**子数组** 是数组中一段连续非空的元素序列。

```python
class Solution:
    def maximumSubarraySum(self, nums: List[int], k: int) -> int:
        ans = 0
        cnt = Counter(nums[:k-1]) #hash 频次表
        s = sum(nums[:k-1]) # [:k-1]是左闭右开
        for i, j in zip(nums[k-1:],nums):
            cnt[i] += 1
            s += i
            if len(cnt) == k:
                ans = max(ans,s)
            
            cnt[j] -= 1
            if cnt[j] == 0:
                del cnt[j]
            s -= j
        
        return ans

```

## \2462. 雇佣 K 位工人的总代价

```java
class Solution {
    public long totalCost(int[] costs, int k, int candidates) {
        //维护两个堆
        long ans = 0;
        int n = costs.length;
        PriorityQueue<Integer> pre = new PriorityQueue<>(candidates);
        PriorityQueue<Integer> suf = new PriorityQueue<>(candidates);
        if (candidates * 2 < n) {
            for (int i = 0; i < candidates; i++) {
                pre.offer(costs[i]);
                suf.offer(costs[n-i-1]);
            }
            for (int i = candidates, j = n - candidates - 1; i <= j && k > 0; k--) {
                if(pre.peek() <= suf.peek()) {
                    ans += pre.poll();
                    pre.offer(costs[i++]);
                } else {
                    ans += suf.poll();
                    suf.offer(costs[j--]);
                }
            }
            // 这一步和 下面else 那一步 python可以简化为一步
            // python的堆可以直接拼接 然后在排序即可 
            while (k-- > 0) {
                if (pre.isEmpty()) {
                    ans += suf.poll();
                } else if (suf.isEmpty()) {
                    ans += pre.poll();
                }else if(pre.peek() <= suf.peek()) {
                    ans += pre.poll();      
                } else {
                    ans += suf.poll();
                }
            }

        } else {
            Arrays.sort(costs);
            for (int i = 0; i < k; i++) {
                ans += costs[i];
            }
        }
        // i > j 刚好重叠 对剩下的元素进行排序 || 一开始就大于= n

        return ans;
    }
}
```

```python
from heapq import heapify, heapreplace
class Solution:
    def totalCost(self, costs: List[int], k: int, candidates: int) -> int:
        ans = 0
        n = len(costs)
        if candidates * 2 < n:
            pre = costs[:candidates]
            heapify(pre)
            suf = costs[-candidates:]
            heapify(suf)
            i, j = candidates, n - candidates - 1
            while k and i <= j:
                if pre[0] <= suf[0]:
                    # 返回栈顶并将costs[i]交换为栈顶 比直接pop效率高
                    ans += heapreplace(pre,costs[i]) 
                    i += 1
                else:
                    ans += heapreplace(suf,costs[j])
                    j -= 1
                k -= 1
            costs = pre + suf
        
        costs.sort()
        return ans + sum(costs[:k])

```

## \2463. 最小移动总距离

X 轴上有一些机器人和工厂。给你一个整数数组 `robot` ，其中 `robot[i]` 是第 `i` 个机器人的位置。再给你一个二维整数数组 `factory` ，其中 `factory[j] = [positionj, limitj]` ，表示第 `j` 个工厂的位置在 `positionj` ，且第 `j` 个工厂最多可以修理 `limitj` 个机器人。

每个机器人所在的位置 **互不相同** 。每个工厂所在的位置也 **互不相同** 。注意一个机器人可能一开始跟一个工厂在 **相同的位置** 。

所有机器人一开始都是坏的，他们会沿着设定的方向一直移动。设定的方向要么是 X 轴的正方向，要么是 X 轴的负方向。当一个机器人经过一个没达到上限的工厂时，这个工厂会维修这个机器人，且机器人停止移动。

**任何时刻**，你都可以设置 **部分** 机器人的移动方向。你的目标是最小化所有机器人总的移动距离。

请你返回所有机器人移动的最小总距离。测试数据保证所有机器人都可以被维修。

```python
class Solution:
    def minimumTotalDistance(self, robot: List[int], factory: List[List[int]]) -> int:
        robot.sort()
        factory.sort(key=lambda a:a[0])

        # 前i个工厂处理j个工厂的最短步数
        n = len(factory)
        m = len(robot)
        f = [0] + [inf] * m
        # 前i个工厂修复j个机器人的最小步数
        for pos, limit in factory:
            # 倒着填前i个工厂维修(m->1]个机器人的属性
            for j in range(m,0,-1):
                t = 0
                # 划分集合 修理1个修理2个修理3个到极限, 为什么没有不修: 压缩后直接从上一个状态转移过来了 f[j] = f[j]会增加代码的复杂度
                for k in range(1,min(j,limit) + 1):
                    t += abs(pos - robot[j-k])
                    f[j] = min(f[j],f[j-k]+t)  
        return f[-1]
```



# 周赛317 

## \2456. 最流行的视频创作者

给你两个字符串数组 `creators` 和 `ids` ，和一个整数数组 `views` ，所有数组的长度都是 `n` 。平台上第 `i` 个视频者是 `creator[i]` ，视频分配的 id 是 `ids[i]` ，且播放量为 `views[i]` 。

视频创作者的 **流行度** 是该创作者的 **所有** 视频的播放量的 **总和** 。请找出流行度 **最高** 创作者以及该创作者播放量 **最大** 的视频的 id 。

- 如果存在多个创作者流行度都最高，则需要找出所有符合条件的创作者。
- 如果某个创作者存在多个播放量最高的视频，则只需要找出字典序最小的 `id` 。

返回一个二维字符串数组 `answer` ，其中 `answer[i] = [creatori, idi]` 表示 `creatori` 的流行度 **最高** 且其最流行的视频 id 是 `idi` ，可以按任何顺序返回该结果*。*

```python
class Solution:
    def mostPopularCreator(self, creators: List[str], ids: List[str], views: List[int]) -> List[List[str]]:
        # 统计作者对应的播放量和 播放量最大的id
        # 记录一个全局最大
        m = {} # name : [view_sum, max_view, id]
        max_view_sum = 0
        for name, i, view in zip(creators, ids, views):
            if name in m:
                t = m[name]
                t[0] += view
                if view > t[1] or view == t[1] and i < t[2]:
                    t[1], t[2] = view, i
            else:
                m[name] = [view, view, i]
            max_view_sum = max(max_view_sum, m[name][0])

        ans = []
        for name ,(view_sum, _, i) in m.items():
            if view_sum == max_view_sum:
                ans.append([name,i])

        return ans
```

# 周赛98

## 替换一个数字后的最大差值

```java
class Solution {
    public int minMaxDifference(int num) {
        // 将最高位换成9 换成0 相减
        Integer max = h1(num);
        Integer min = h2(num);
        return max - min;
    }

    private Integer h2(Integer x) {
        // 最高位肯定不是0
        char[] s = x.toString().toCharArray();
        int n = s.length;
        char start = s[0];
        for (int i = 0; i < n; i++) {
            if (s[i] == start) {
                s[i] = '0';
            }
        }
        StringBuilder sb = new StringBuilder();
        for (char a : s) {
            sb.append(a);
        }
        return Integer.parseInt(sb.toString());
    }

    public int h1(Integer x) {
        char[] s = x.toString().toCharArray();
        int n = s.length;

        // 最高位不一定不是9
        char start = 'a';
        for (int i = 0; i < n; i++) {
            if (s[i] == '9') continue;
            else {
                start = s[i];
                break;
            }
        }

        if (start == 'a') return x;

        for (int i = 0; i < n; i++) {
            if (s[i] == start) {
                s[i] = '9';
            }
        }
        StringBuilder sb = new StringBuilder();
        for (char a : s) {
            sb.append(a);
        }
        return Integer.parseInt(sb.toString());

    }
}
```

## \6360. 最小无法得到的或值

## \6361. 修改两个元素的最小分数

## \6358. 更新数组后处理求和查询

不会做