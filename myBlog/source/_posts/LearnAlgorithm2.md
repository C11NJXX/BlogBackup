---
title: 堆的数据结构与算法
tags: [算法,数据结构]
categories: [学习]
cover: https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307291722180.png
swiper_index: 2 #置顶轮播图顺序，非负整数，数字越大越靠前
description: 记录一下学习算法和数据结构的过程（堆）

---

记录一下学习算法和数据结构的过程（堆）

本博客参考**[《Hello算法》](https://www.hello-algo.com/chapter_tree/)**

<!-- more -->

# 堆（Heap）

---

## 相关概念

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307291729846.png)

## 学习思路

学习堆的时候，我自己写了一个类（MyHeap），内部由一个ArrayList维护，主要实现入堆和堆顶元素出堆的操作

后面还有一个相关的TOP-K问题

### Tips

堆有很多有意思的地方，就是内部用数组实现的时候，可以考虑进行下标的索引来对外实现为堆（或完全二叉树）

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/20230729172830.png)

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307291731572.png)

---

## 难点1:入堆

为了维护堆的性质，也就是任意的根节点都要大于其子树的元素

所以入堆的时候就要考虑进行重新整合，也就是堆化操作（heapify）

**MyHeap类（相关代码）**

```java
package main.java.learn.theme.heapdemo.demo1;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class MyHeap {
    //内部维护一个ArrayList，对外表现为堆
    private List<Integer> heap;

    //默认初始化
    public MyHeap() {
        heap = new ArrayList<>();
    }

    //根据传入的列表进行堆的初始化
    public MyHeap(List<Integer> array) {
        heap = array;
    }

    //判断堆是否为空
    public boolean isEmpty() {
        return heap.isEmpty();
    }

    //获取堆大小
    public int size() {
        return heap.size();
    }

    //获取堆顶元素（内部实现即为获取0索引元素）
    public int peek() {
        if (!isEmpty()) {
            return heap.get(0);
        } else {
            System.out.println("Empty!");
            return -1;
        }
    }

    //创建映射（获取传入节点的左，右，父节点索引)
    public int getLeft(int i) {
        return 2 * i + 1;
    }

    public int getRight(int i) {
        return 2 * i + 1;
    }

    public int getPre(int i) {
        return (i - 1) / 2;
    }

    //元素入堆操作
    public void push(int val) {
        //先将元素置于内部队列的尾部（即直接add）
        heap.add(val);
        //加入元素后，可能会不符合堆的性质，要进行堆化(Heapify)
        pushHeapify(size() - 1);
    }

    private void pushHeapify(int i) {
        while (true) {
            //从队尾开始，即下标为（size-1）处开始
            //获取父节点下标
            int pre = getPre(i);
            //如果父节点下标<0，则代表堆化完成，如果>=0，进行比较
            if (pre < 0) {
                System.out.println("堆化完成");
                return;
            } else {
                //获取父节点与当前节点value
                int preVal = heap.get(pre);
                int val = heap.get(i);
                //与父节点进行比较，如果比父节点小或等于父节点，堆化完成，否则交换值
                if (preVal >= val) {
                    System.out.println("堆化完成");
                    return;
                } else {
                    //交换值(注意这里要用下标设置，否则只交换值没有用，对应位置的值还是没变的）
                    int temp = preVal;
                    heap.set(pre, val);
                    heap.set(i, temp);
                    i = pre;
                }
            }
        }
    }

```



---

## 难点2:堆顶元素出堆

### 巧妙处理

利用先交换堆顶和堆尾元素，巧妙的避免了直接删除堆顶元素再堆化对堆的破坏性

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307291732179.png)

**MyHeap类（相关代码）**

```java

    //堆顶元素出列(采取先交换后堆化的操作)
    public void pop() {
        if (isEmpty()) {
            throw new ArrayIndexOutOfBoundsException("元素为空");
        } else {
            //先交换堆顶元素和堆底元素的值
            System.out.println("Swap!");
            int tmp = heap.get(0);
            heap.set(0, heap.get(size() - 1));
            heap.set(size() - 1, tmp);
            //删除堆底元素
            System.out.println("Delete!");
            heap.remove(size() - 1);
            //堆化操作
            popHeapify(0);
        }
    }

    private void popHeapify(int i) {
        while(true){
            //第一次进入下标为0
            //创建临时变量用于保存左右下标
            int l,r;
            l = getLeft(i);
            r = getRight(i);
            if(heap.get(l) > heap.get(i) && heap.get(l) >= heap.get(r)) {
                //交换左节点和父节点
                int tmp = heap.get(l);
                heap.set(l,heap.get(i));
                heap.set(i,tmp);
                i = l;
            }else if(heap.get(r) > heap.get(i) && heap.get(r) >= heap.get(l)) {
                //交换右节点和父节点
                int tmp = heap.get(r);
                heap.set(r,heap.get(i));
                heap.set(i,tmp);
                i = r;
            }else {
                System.out.println("堆化完成");
                return;
            }
        }
    }
```

---

## 测试

**MyHeap类（相关代码）**

```java
    //从堆顶开始遍历（测试用）
    public void printHeap() {
        for (int i : getHeap()) {
            System.out.print(i + " ");
        }
    }

    private List<Integer> getHeap() {
        return heap;
    }
```

### 入堆

**Test类**

```java
import java.util.Arrays;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        method1();
    }

    private static void method1() {
        Integer[] init = {9,8,6,6,7,5,2,1,4,3,6,2};
        List<Integer> arr = new ArrayList<>(Arrays.asList(init));
        MyHeap heap = new MyHeap(arr);
        System.out.println("初始状态");
        heap.printHeap();
        System.out.println("入堆");
        heap.push(7);
        System.out.println("入堆后");
        heap.printHeap();
    }
```

---

#### 初始状态

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307291742824.png)

#### 入堆后

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307291742824.png)

#### 输出结果

```markdown
初始状态
9 8 6 6 7 5 2 1 4 3 6 2 入堆
堆化完成
入堆后
9 8 7 6 7 6 2 1 4 3 6 2 5 
```

---

### 出堆

**MyHeap类（相关代码）**

```java
    //从堆顶开始遍历（测试用）
    public void printHeap() {
        for (int i : getHeap()) {
            System.out.print(i + " ");
        }
    }

    private List<Integer> getHeap() {
        return heap;
    }
```

**Test类**

```java
package main.java.learn.theme.heapdemo.demo1;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Test {
    public static void main(String[] args) {
        method2();
    }

    private static void method2() {
        //初始化，注意使用Integer包装类，并且使用Arrays的静态方法转换成List传入
        Integer[] init2 = {9,8,7,6,7,6,2,1,4,3,6,2,5};
        List<Integer> arr = new ArrayList<>(Arrays.asList(init2));
        MyHeap heap = new MyHeap(arr);
        System.out.println("初始状态");
        heap.printHeap();
        System.out.println("出堆");
        heap.pop();
        System.out.println("出堆后");
        heap.printHeap();
    }
}

```

---

#### 初始状态

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307291745616.png)

#### 出堆后

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307291745616.png)

#### 输出结果

```markdown
初始状态
9 8 7 6 7 6 2 1 4 3 6 2 5 出堆
Swap!
Delete!
堆化完成
出堆后
8 6 7 5 7 6 2 1 4 3 6 2
```

---





