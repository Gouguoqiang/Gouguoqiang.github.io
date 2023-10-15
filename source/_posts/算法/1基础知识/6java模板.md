---
title: 一. 基础知识Java模板
tags:
  - 算法基础
  - 排序
categories:
  - 算法
  - 1基础知识
date: 2022-9-02 11:51:56
---



# 快速排序

```java
public void quick_sort(int[] q, int l, int r) {
        if (l >= r) return ;
        int x = q[l], i = l - 1, j = r + 1;
        while (i < j) {
            do i ++; while(q[i] < x);
            do j --; while(q[j] > x);
            if (i < j) {
                int t = q[i];
                q[i] = q[j];
                q[j] = t;
            }
        }
        quick_sort(q,l,j);
        quick_sort(q,j+1,r);
    }
```



# 归并排序

```java
public static void merge_sort(int[] q, int l, int r) {
        if (l >= r) return ;
        int mid = l + (r - l) / 2;
        merge_sort(q,l,mid);
        merge_sort(q,mid + 1,r);
        int k = 0, i = l, j = mid + 1;
        while (i <= mid && j <= r) {
            if (q[i] < q[j]) {
                temp[k++] = q[i++];
            } else {
                temp[k++] = q[j++];
            }
        }
        while (i <= mid) temp[k++] = q[i++];
        while (j <= r) temp[k++] = q[j++];

        for (i = l, j = 0; j < k; j++, i++) q[i] = temp[j];
    }
```

# 二分

```java
 // 二分check
    public static boolean check(int mid, int target) {
        return q[mid] >= target;
    }
    // 二分模板 找target 有多个 返回第一个的下标
    public static int bin_search(int target) {
        int n = q.length;
        int l = 0, r = n - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (check(mid,target)) {
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
        if (l >= n || q[l] != target) return -1;
        return l;
    }
```

# 离散化







