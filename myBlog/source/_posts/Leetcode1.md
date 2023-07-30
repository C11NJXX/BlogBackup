---
title: Leetcode热题:两数之和
tags: [算法,哈希表]
categories: [学习]
cover: https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307301655995.png
swiper_index: 3 #置顶轮播图顺序，非负整数，数字越大越靠前
description: 两数之和，梦的开始

---

记录一下学习算法的过程

<!-- more -->

# 解法1

两个for循环的解法实在是根深蒂固了，拿到就想用

```java
package main.java.leetcode.July.demo1;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            int remain = target - nums[i];
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] == remain) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[0];
    }
}
```

# 解法2（参考）

```java
package main.java.leetcode.July.demo1;

import java.util.HashMap;
import java.util.Map;

public class Solution1 {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int remain = target - nums[i];
            if (map.containsKey(remain)) {
                return new int[]{map.get(remain), i};
            }
            map.put(nums[i], i);
        }
        throw new RuntimeException("没有匹配的数字");
    }
}

```

第一次用哈希表解题，这个解法真的优化了时间复杂度,比我之前只用c好多了哈哈哈

---



