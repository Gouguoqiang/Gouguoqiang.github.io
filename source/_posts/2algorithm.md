---
title: 算法随记
tags:
categories:
  - 算法
keywords: 
description: 未整理成体系的算法随记
date: 2022-9-01 11:51:56
---





# 基础知识



> 力扣周赛java选手阿全,天堂



# 实用技巧

## Map.putIfAbsent();

## 随机数

常用的两种

Random random = new Random（）;

random.nextInt(l.size())

Math.random*l.size()   Math.randow [0,1)

## HashMap getOrDefault() 方法

getOrDefault() 法获取指定 key 对应的 value，如果找不到 key ，则返回设置的默认值。

hashMap.containsKey();

string.substring();

## HashMap遍历



###  先取出 所有的 Key 

```java
Set  keyset = map.keySet();



for(Object key: keyset){
	map.get(key);
}


Iterator iterator= keyset.iterator();

while(iterator.hasNext()){

Object key = iterator.next();

map.get(key);
}
```



### 取出所有value

Collection values = map.values();

### EntrySet 获取k-v

Set entrySet = map.entrySet();

```java
Set entrySet = map.entrySet();
class Solution {
    public int mostFrequentEven(int[] nums) {
        Map<Integer,Integer> map = new HashMap<>();
        for (int num : nums) {
            if (num % 2 == 0) {
                map.put(num,map.getOrDefault(num,0)+1);
            }
        }
        Set<Map.Entry<Integer,Integer>> entrySet = map.entrySet();

        int ans = -1;
        int max = 0;
        for (Map.Entry<Integer,Integer> entry : entrySet) {
            if (entry.getValue() > max) {
                ans = entry.getKey();
                max = entry.getValue();
            }
        }
        return ans;
    }
}

/(1) 增强 for
System.out.println("----使用 EntrySet 的 for 增强(第 3 种)----");
for (Object entry : entrySet) {
//将 entry 转成 Map.Entry
Map.Entry m = (Map.Entry) entry;
System.out.println(m.getKey() + "-" + m.getValue());
}
//(2) 迭代器
System.out.println("----使用 EntrySet 的 迭代器(第 4 种)----");
Iterator iterator3 = entrySet.iterator();
while (iterator3.hasNext()) {
Object entry = iterator3.next();
//System.out.println(next.getClass());//HashMap$Node -实现-> Map.Entry (getKey,getValue)
//向下转型 Map.Entry
Map.Entry m = (Map.Entry) entry;
System.out.println(m.getKey() + "-" + m.getValue());
}


```

# 一些值得学习的处理

## [147. 对链表进行插入排序](https://leetcode.cn/problems/insertion-sort-list/)

### 方法一 直观模拟

```java
//方法一 直观模拟
class Solution {
    public ListNode insertionSortList(ListNode head) {
        
        //分割成单结点,去除链表的影响 所有都是单节点操作
        //不分割可能会成环
        if (head == null) return null;
        ListNode newHead = head;
        head = head.next;
        ListNode p = newHead;
        newHead.next = null;
        ListNode q = head;
        // 梳理下思路
        //遍历输入结点,找输入结点在新链表的位置,小于等于新结点的头结点 直接头插 大于则往后遍历,
        // 直到 p.next.val > q.val
        // 如果 p.next == null 直接尾插
        while (q != null) {
            //头插
            ListNode t = q.next;
            q.next = null;
            if (q.val < newHead.val) {
                //头插法
                q.next = newHead;
                newHead = q;

                q = t;
                p = newHead;
                continue;
            }
            while (p == null || q.val > p.val) {
                if (p.next == null || p.next.val > q.val ) {
                    break;
                }
                p = p.next;
            }
            // 插入
            ListNode temp = p.next;
            p.next = q;
            q.next = temp;
            q = t;
            p = newHead;
        }
        return newHead;
    }
}
```

### 方法二

```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        //在一条环上处理
        ListNode dummy = new ListNode(-1,head);
        ListNode newLast = dummy.next;
        ListNode cur = newLast.next;
        while (cur != null) {
            if (cur.val >= newLast.val) {
                newLast = cur;   
            }else {
                //从头找到p.next.val > cur.val
                //因为last.val > cur.val 所以.next不会为空的
                ListNode p = dummy;
                while (p.next.val <= cur.val) {
                    p = p.next;
                }
                newLast.next = cur.next;
                cur.next = p.next;
                p.next = cur;
            }
            cur = newLast.next;
        }
        return dummy.next;
    }
}
```
