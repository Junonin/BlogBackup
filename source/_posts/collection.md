---
title: Collection
abbrlink: fc4d6532
date: 2021-01-16 17:33:58
description: Java Collection
categories:
  - JAVA
  - Collection
top: 
tags: Collection
---

## 前言

> Java Collection学习记录

### Java对数组进行倒序排序

```java

 public class Test {
     public static void main(String[] args) {
         // 注意，要想改变默认的排列顺序，不能使用基本类型（int,double, char）
         // 而要使用它们对应的包装类
         Integer[] a = { 9, 8, 7, 2, 3, 4, 1, 0, 6, 5 };
         // 定义一个自定义类MyComparator的对象
         Comparator cmp = new MyComparator();
         //Arrays.sort(数组[],比较器);
         Arrays.sort(a, cmp);
         //Collections.reverseOrder()也是一个逆序比较器，倒叙
         Arrays.sort(a, Collections.reverseOrder());
         for (int x : a) {
             System.out.print(xw + " ");
         }
     }
 }
 
 // Comparator是一个接口比较器
 // Comparator中的compare可以将传入进行比对，按照返回的参数大于(1)等于(0)小于(-1)进行排序
 // 默认情况下返回1的在后，返回-1的在前
 // 如果我们需要逆序，只要把返回值-1和1的换位置即可。
 class MyComparator implements Comparator<Integer> {
     public int compare(Integer o1, Integer o2) {
         // 如果o1小于o2，我们就返回正值，如果n1大于n2我们就返回负值，倒叙
         if (o1 < o2) {
             return 1;
         } else if (o1 > o2) {
             return -1;
         } else {
             return 0;
         }
     }
 }

```
