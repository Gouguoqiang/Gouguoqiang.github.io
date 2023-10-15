---
title: 一  . 基础知识LeetCode
tags:
  - 算法基础
  - 排序
categories:
  - 算法
  - 1基础知识
date: 2022-9-02 11:51:56

---



# API


## 6233. 


```java
class Solution {
    public double[] convertTemperature(double celsius) {
        Double res1 = celsius + 273.15;
        Double res2 = celsius * 1.80 + 32;
        String str1 = String.format("%.5f",res1);
        String str2 = String.format("%.5f",res2);
        
        return new double[] {Double.parseDouble(str1),Double.parseDouble(str2)};
            
    }
}
```

# 双指针



## 6234. 最小公倍数为 K 的子数组数目

```java
//错误代码
class Solution {
    public int subarrayLCM(int[] nums, int k) {
        int n = nums.length;
        int res = 0;
       
        
        for (int i = 0, j = 0; i < n; i++) {
            if (isLCM(nums,j,i,k)) res ++;
            else {
                while(++j < i && !isLCM(nums,j,i,k)) res++;
                
            }
            
        }
        return res;
    }
    public boolean isLCM(int[] nums, int j, int i, int k) {
        for (int m = 1; m <= k; m++) {
            boolean flag = false;
            for (int n = j; n <= i; n++) {
                if (m % nums[n] != 0) {
                    flag = true;
                    break;
                }
            }
            
            if (!flag && m == k) return true;
            if (!flag && m != k) return false; 
        }
        return false;
    }
} 
```



# 6250. 商店的最少代价

给你一个顾客访问商店的日志，用一个下标从 **0** 开始且只包含字符 `'N'` 和 `'Y'` 的字符串 `customers` 表示：

- 如果第 `i` 个字符是 `'Y'` ，它表示第 `i` 小时有顾客到达。
- 如果第 `i` 个字符是 `'N'` ，它表示第 `i` 小时没有顾客到达。

如果商店在第 `j` 小时关门（`0 <= j <= n`），代价按如下方式计算：

- 在开门期间，如果某一个小时没有顾客到达，代价增加 `1` 。
- 在关门期间，如果某一个小时有顾客到达，代价增加 `1` 。

请你返回在确保代价 **最小** 的前提下，商店的 **最早** 关门时间。

注意，商店在第 `j` 小时关门表示在第 `j` 小时以及之后商店处于关门状态。

 

**示例 1：**

```
输入：customers = "YYNY"
输出：2
解释：
- 第 0 小时关门，总共 1+1+0+1 = 3 代价。
- 第 1 小时关门，总共 0+1+0+1 = 2 代价。
- 第 2 小时关门，总共 0+0+0+1 = 1 代价。
- 第 3 小时关门，总共 0+0+1+1 = 2 代价。
- 第 4 小时关门，总共 0+0+1+0 = 1 代价。
在第 2 或第 4 小时关门代价都最小。由于第 2 小时更早，所以最优关门时间是 2 。
```

**示例 2：**

```
输入：customers = "NNNNN"
输出：0
解释：最优关门时间是 0 ，因为自始至终没有顾客到达。
```

**示例 3：**

```
输入：customers = "YYYY"
输出：4
解释：最优关门时间是 4 ，因为每一小时均有顾客到达。
```

 

**提示：**

- `1 <= customers.length <= 105`
- `customers` 只包含字符 `'Y'` 和 `'N'` 。

```java
class Solution {
    public int bestClosingTime(String customers) {
		// 前缀和统计1的个数 那么0的个数就是
    }
}
```

