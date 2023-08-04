---
title: Leetcode热题:最长连续序列
tags: [算法,哈希表，数组]
categories: [学习]
cover: https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307301655995.png
swiper_index: 3 #置顶轮播图顺序，非负整数，数字越大越靠前
description: 最长连续序列



---

记录一下学习算法的过程

<!-- more -->

# Solution

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        //用于记录每个连续序列
        List<Integer> counts = new ArrayList<>();
        //用于去除重复数据并排序
        Set<Integer> set = new TreeSet<>();
        //添加数据到set中
        for (Integer num : nums) {
            set.add(num);
        }
        //设计算法遍历set集合，记录连续序列的长度，间断后存储到counts集合中
        //Q1:如何实现在间断后，下一次遍历从上一次的后面开始？
        //Q2:如何记录每一次连续序列的长度？
        //Q3:如何判断是否连续？

        //标记是否为第一次，否则preNum为null
        boolean isFirst = true;
        int preNum = 0;
        //记录每一个分段的长度
        int length = 0;
        for(Integer num : set) {
            if(isFirst) {
                //第一次判断，先记录值，并且增加length,isFirst置反
                preNum = num;
                length++;
                isFirst = false;
                continue;
            }
            //判断是否连续
            if(num - preNum == 1) {
                //连续,记录值，增加length
                preNum = num;
                length++;
                continue;
            }else {
                //不连续,先添加当前连续的length值，重置length为1，防止下一次记录的时候少1
                counts.add(length);
                length = 1;
                //继续遍历接下来的set集合,记录值
                preNum = num;
                continue;
                //这里有个问题，如果一直连续到最后，就会跳过这里的添加步骤，所以要在最后兜底
            }

        }
        //兜底
        if(length != 0) {
            counts.add(length);
        }
       //最后返回连续序列中的最大值
        counts.sort(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 > o2 ? -1 : 1;
            }
        });
       if(!counts.isEmpty()){
            return counts.get(0);
        }else{
            return 0;
        }
    }
}
```

# 心得

写这道题，刚开始的时候思路就是想着怎么样判断连续，然后就用做差判断，有一点小问题都解决了没啥好讨论，然后尝试**第一次提交**，提交后发现有一部分数据没法通过，**原因是我只记录了第一次连续序列的长度之后就直接return了**，没有考虑后面还可能出现更长的连续序列，所以我想着这个跟分段有点类似，但这个分段又比较特殊，既要连续，又要记录长度，所以我就想着用一个集合记录每个分段的长度，然后最后自定义sort一下，返回最大值，由于时间复杂度有要求所以要一次遍历到底，于是我就设计了上面这个循环，**第二次提交的时候**又出现了问题，就是我的一些值总是比答案小1，后来发现是**如果一直连续到最后，就会跳过else的添加步骤，所以要在最后兜底**，最后也是成功的提交了，算是有些欣喜吧哈哈哈，毕竟是leetcode上刷的第三道，也是比较独立解决的

debug真的越来越体会到他的实用性，在我盯着代码死死看不出问题的时候，也许一个断点就能帮助我很多！

附上一张图

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202308031645001.png)

---

