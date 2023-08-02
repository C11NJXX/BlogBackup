---
title: Java集合
tags: [Java，集合,List,Map]
categories: [学习]
cover: https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202308021344646.png
swiper_index: 2 #置顶轮播图顺序，非负整数，数字越大越靠前
description: Java学习记录



---

集合

<!-- more -->

# 集合

## List

---
List
ArrayList
LinkedList
List.of()
迭代器遍历

---

```java
package main.java.learn.date.August2023.nd2.demo1._1;

import java.util.Iterator;
import java.util.List;


public class Test {
    public static void main(String[] args) {
       //创建List并使用List.of(),用迭代器遍历
        List<String> list = List.of("a","b","c");//阅读源码发现，在元素数量为10之前它都有分别的方法，超过10之后方法统一
        for(Iterator<String> it = list.iterator() ; it.hasNext() ; ) {
            System.out.println(it.next());//相当于获取完next之后，直接下一个next
        }
        //第二种直接使用for each
        for(String str : list) {
            System.out.println(str);
        }
    }
}

```

### List和Array的转化

#### List to Arrat

```java
package main.java.learn.date.August2023.nd2.demo1._2;

import java.util.Arrays;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        //把List转化成Array
        List<String> list = List.of("apple","banana","grape");
        method1(list);
        method2(list);
        method3(list);
        //Object[] array = list.toArray(Integer[]::new);
        //上面的看不懂，函数式写法
    }

    private static void method3(List<String> list) {
        //传入刚好的大小
        String[] array = list.toArray(new String[list.size()]);
        for (String str : array) {
            System.out.println(str);
        }
    }

    private static void method2(List<String> list) {
        //传入相同存储相同类型的Array，如果不够则扩容，如果超过则多余的填为null
        list.toArray(new String[3]);
    }

    private static void method1(List<String> list) {
        //利用toArray返回Object数组
        Object[] array = list.toArray();
        for(Object o : array) {
            System.out.println(o);
        }
    }
}

```

#### Array to List

```java
package main.java.learn.date.August2023.nd2.demo1._2;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class ArrayToList {
    public static void main(String[] args) {
        //使用List.of(),传入对应的Array
        Integer[] nums = {1,2,3};
        List<Integer> list = List.of(nums);
        //JDK11前
        List<Integer> list1 = Arrays.asList(nums);
        //由于返回的是List，而不是实现类ArrayList或者LinkedList所以无法进行修改
        try {
            list.add(5);
        } catch (UnsupportedOperationException e) {
            e.printStackTrace();
        }
    }
}

```

### Solution(*找出从**start**到**end**的缺失的数字并返回*)

```java
package main.java.learn.date.August2023.nd2.demo1._3;


import java.util.*;

public class Main {
    public static void main(String[] args) {
        // 构造从start到end的序列：
        final int start = 10;
        final int end = 20;
        List<Integer> list = new ArrayList<>();
        for (int i = start; i <= end; i++) {
            list.add(i);
        }
        // 洗牌算法suffle可以随机交换List中的元素位置:
        Collections.shuffle(list);
        // 随机删除List中的一个元素:
        int removed = list.remove((int) (Math.random() * list.size()));
        int found = findMissingNumber(start, end, list);
        System.out.println(list.toString());
        System.out.println("missing number: " + found);
        System.out.println(removed == found ? "测试成功" : "测试失败");
    }

    /**
     * TODO: 找出从start到end的缺失的数字并返回
     */
    static int findMissingNumber(int start, int end, List<Integer> list) {
        int preSum = 165;
        int sum = 0;
        for (int i : list) {
            sum += i;
        }
        return preSum - sum;
    }
}

```



