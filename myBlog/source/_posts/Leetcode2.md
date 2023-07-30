---
title: Leetcode热题:字母异位词分组
tags: [算法,哈希表，字符串]
categories: [学习]
cover: https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307301655995.png
swiper_index: 3 #置顶轮播图顺序，非负整数，数字越大越靠前
description: 字母异位词分组


---

记录一下学习算法的过程

<!-- more -->

# Solution

```java
package main.java.leetcode.July.demo2;

import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        //用于储存质数和以及对应的单词
        Map<Double, List<String>> stringMap = new HashMap<>();
        //用于储存生成的质数的结果(不重复)
        Set<Double> set = new HashSet<>();
        //以下是生成对应质数的字母表
        int[] PRIMES = {
                2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47,
                53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101
        };
        Map<Character, Integer> map = new HashMap<>();
        char currentChar = 'a';
        for (int prime : PRIMES) {
            map.put(currentChar, prime);
            currentChar++;
        }
        //开始遍历strs
        for (String word : strs) {
            char[] charArray = word.toCharArray();
            boolean isFirst = true;
            double val = 0;
            //拆解字符，计算质数积
            for (char ch : charArray) {
                if (isFirst) {
                    val = map.get(ch);
                    isFirst = false;
                } else {
                    val *= map.get(ch);
                }
            }
            //记录质数积
            set.add(val);
            //如果要实现一个key对应多个value，必须要将相同的value存入到集合中，如何判断质数和相同的集合是否存在？

            //添加到对应的哈希表中,创建相同key值对应的list，再把list拿出来对应添加
            //首先要判断是否存在对应key值的list
            if (stringMap.containsKey(val)) {
                //存在，拿出对应的list添加
                stringMap.get(val).add(word);
            } else {
                //不存在，先添加再拿出list添加
                stringMap.put(val, new ArrayList<>());
                stringMap.get(val).add(word);
            }
        }
        //根据set集合内的key值，分别输出
        //最终输出
        List<List<String>> list = new ArrayList<>();
        for (double key : set) {
            list.add(stringMap.get(key));
        }
        return (List<List<String>>) list;
    }
}

```

## 感想

先前一直在看算法入门书，感觉到有点乏力之后想来尝试一下leetcode的题目，果然对我是有难度的，这道题目我相当于是借鉴了一个思路，就是将每一个字母对应一个素数，这样子单词拆解之后对应字母的乘积就应该是唯一的，然后我就去写，刚开始对数据结构只是纸面上的了解，顶多就是自己手写过基本功能，这个熟练度导致我在写这种题目的时候对于很多的内置方法是很陌生的，所以我一边写一边问gpt，然后去网上查资料才拼凑出这个能过的解法（之所以说能过是因为空间复杂度被完爆，而且时间复杂度也没好到哪里去，现在至少解出来题目了还有点欣慰）。然后遇到了问题也蛮多，代码里的注释也有，就是一个key对应多个value的情况，在问过gpt后他说可以引入一个第三方库里面有MultiValueMap这个类，我寻思着这没必要于是采取了另一种方法，就是key值代表质数积，然后value是对应的唯一存在的list，然后list再去存储单词，这样的话就需要很多的map，而且我还用了set集合（可以存储不重复的key）值来保存这些质数积，在最后的时候遍历set，然后根据set里面的key值去查找对应的list，再把list遍历。

整体下来是能符合要求的，对于我这种小白，除了借鉴了质数和避免重复这个思路外，其余的思路都是比较自然的，所以我现在对自己的要求也只是能实现即可（但不能以答案为导向或者是那种...你懂的，就是代码超长...)，路漫漫其修远兮～～～

---

