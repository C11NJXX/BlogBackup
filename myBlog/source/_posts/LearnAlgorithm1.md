---
title: 树的数据结构与算法
tags: [算法,数据结构]
categories: [学习]
cover: https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/20230630111230.png
swiper_index: 2 #置顶轮播图顺序，非负整数，数字越大越靠前
description: 记录一下学习算法和数据结构的过程（树）
---

记录一下学习算法和数据结构的过程（树）

本博客参考**[《Hello算法》](https://www.hello-algo.com/chapter_tree/)**

<!-- more -->

# 二叉树🌳



## 1.概念学习

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/20230630111354.png)

### 从两种角度去理解二叉树的高度，节点的深度和高度

从上图来看，这像一颗倒着长的树，那么理解**二叉树的高度**时，就要在脑海里**把这棵树立过来**（从根节点到最远节点）就是树的高度；

再理解**节点的深度和高度**，就要把这棵树想象成**埋在地里的根**，在比较深度的时候要和地面上的根节点比，在比较高度的时候，要和最深的根去比

## 2 二叉树的插入和删除操作

### 插入操作

**TreeNode类**

```java
package main.java.learn.theme.treedemo.demo1;

import com.sun.source.tree.Tree;

//创建节点类
public class TreeNode implements Comparable {
    private int value;
    private TreeNode left;
    private TreeNode right;

    public TreeNode() {
        this.value = 0;
        left = null;
        right = null;
    }

    public TreeNode(int value) {
        this.value = value;
        left = null;
        right = null;
    }

    public void setLeft(TreeNode left) {
        this.left = left;
    }

    public void setRight(TreeNode right) {
        this.right = right;
    }

    public static void showInfo1(TreeNode treeNode) {
        if (treeNode.left != null && treeNode.right != null) {
            System.out.println(treeNode.value + "节点的左节点为：" + treeNode.left.value + " 该节点的右节点为：" + treeNode.right.value);
        } else if (treeNode.left != null && treeNode.right == null) {
            System.out.println(treeNode.value + "节点的左节点为：" + treeNode.left.value + " 该节点的右节点为空 ");
        } else if (treeNode.left == null && treeNode.right != null) {
            System.out.println(treeNode.value + "节点的左节点为空" + " 该节点的右节点为:" + treeNode.right.value);
        } else {
            System.out.println(treeNode.value + "节点为叶节点");
        }
    }
    public static void showInfo2(TreeNode treeNode){
        if(treeNode != null) {
            //根节点不为空，先打印根节点情况,判断左右节点，按照先左后右顺序打印
            TreeNode.showInfo1(treeNode);
           //利用循环，当碰到叶节点时，停止该分支
           if(treeNode.left != null) {
               //左节点不为空，作为新的根节点
               Thread leftThread = new Thread(
                       new Runnable() {
                           @Override
                           public void run() {
                               TreeNode.showInfo2(treeNode.left);
                           }
                       }
               );
               leftThread.start();
           }
            if(treeNode.right != null) {
                //左节点不为空，作为新的根节点
                Thread rightThread = new Thread(
                        new Runnable() {
                            @Override
                            public void run() {
                                TreeNode.showInfo2(treeNode.right);
                            }
                        }
                );
                rightThread.start();
            }
        }else {

        }
    }
        public static boolean isLeaf(TreeNode treeNode) {
            if(treeNode.left == null && treeNode.right == null) {
                return true;
            }else {
                return false;
            }
        }
    @Override
    public int compareTo(Object o) {
        TreeNode treeNode = (TreeNode) o;
        if (this.value > treeNode.value) {
            return 1;
        } else if (this.value < treeNode.value) {
            return -1;
        } else {
            return 0;
        }
    }
}


```

这里我用了两个showInfo，第一个比较单纯，没有考虑到如何正确遍历树，后面一个我用了多线程和一个循环结构，配合第一个方法可以打印出正确的树的结构，不过后期应该有更好的遍历方法，这里只是我自己的写法

**Test**

```java
package main.java.learn.theme.treedemo.demo1;

import java.util.ArrayList;
import java.util.Collections;

public class Test {
    public static void main(String[] args) {
        //创建二叉树
        TreeNode treeNode1 = new TreeNode(1);
        TreeNode treeNode2 = new TreeNode(2);
        TreeNode treeNode3 = new TreeNode(3);
        TreeNode treeNode4 = new TreeNode(4);
        TreeNode treeNode5 = new TreeNode(5);
        //建立联系
        treeNode1.setLeft(treeNode2);
        treeNode1.setRight(treeNode3);
        treeNode2.setLeft(treeNode4);
        treeNode3.setRight(treeNode5);
        //创建集合存储节点
        ArrayList<TreeNode> list = new ArrayList<>();
        list.add(treeNode1);
        list.add(treeNode2);
        list.add(treeNode3);
        list.add(treeNode4);
        list.add(treeNode5);
//        for (TreeNode treeNode : list) {
//            TreeNode.showInfo1(treeNode);
//        }
        //TreeNode.showInfo2(treeNode1);

        exert(treeNode1, treeNode2, list);
    }

    private static void exert(TreeNode treeNode1, TreeNode treeNode2, ArrayList<TreeNode> list) {
        //测试插入节点
        //二节点的左节点插入6节点
        TreeNode treeNode6 = new TreeNode(6);
        list.add(treeNode6);
        treeNode1.setLeft(treeNode6);
        treeNode6.setLeft(treeNode2);

        System.out.println("插入6节点后" + "\n" + "-------------------");
        TreeNode.showInfo2(treeNode1);
    }
}

```

### 打印结果

---

1节点的左节点为：2 该节点的右节点为：3
2节点的左节点为：4 该节点的右节点为空 
3节点的左节点为空 该节点的右节点为:5
4节点为叶节点
5节点为叶节点

---

#### 插入6节点后

1节点的左节点为：6 该节点的右节点为：3
3节点的左节点为空 该节点的右节点为:5
6节点的左节点为：2 该节点的右节点为空 
2节点的左节点为：4 该节点的右节点为空 
4节点为叶节点
5节点为叶节点

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/%EF%BC%88%E6%A0%91%EF%BC%89%E6%8F%92%E5%85%A5%E8%8A%82%E7%82%B9.jpg)

### 删除节点

这个比较简单，只需要直接把2节点作为1节点的左节点，6就相当于被删除了～

## 二叉树的遍历（**广度遍历与深度遍历**）

### 层序遍历法（广度遍历优先）

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/20230716161656.png)

```java
package main.java.learn.theme.treedemo.demo2;
import main.java.learn.theme.treedemo.demo1.TreeNode;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Test {
    public static void main(String[] args) {
        //创建二叉树
        TreeNode treeNode1 = new TreeNode(1);
        TreeNode treeNode2 = new TreeNode(2);
        TreeNode treeNode3 = new TreeNode(3);
        TreeNode treeNode4 = new TreeNode(4);
        TreeNode treeNode5 = new TreeNode(5);
        //建立联系
        treeNode1.setLeft(treeNode2);
        treeNode1.setRight(treeNode3);
        treeNode2.setLeft(treeNode4);
        treeNode3.setRight(treeNode5);
        //------//
        List<Integer> result = method1(treeNode1);
        for(int i : result) {
            System.out.println(i + " ");
        }

    }

    private static List<Integer> method1(TreeNode root) {
        //层序遍历法，遍历的同时获取节点值
        //用queue存储每个节点(FIFO)
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        //创建一个List存储每个节点值
        List<Integer> list = new ArrayList<>();
        //循环判断是否为空
        while(!queue.isEmpty()) {
            //非空，队首出列（第一次就是根节点）
            TreeNode node = queue.poll();
            list.add(node.getValue());
            //分别判断左右节点(利用queue FIFO的特性，保证遍历顺序)
            if(node.getLeft() != null ) {
                queue.add(node.getLeft());
            }
            if(node.getRight() != null) {
                queue.add(node.getRight());
            }
        }
        //在每一次遍历的时候都会记录当前传入节点的值，并且如果有左右节点，都会按照顺序入队，循环结束证明所有的节点都已经遍历完毕了
        return list;
    }
}

```

---

**打印结果**

1 
2 
3 
4 
5

---

### 前序，后序，中序遍历法(深度遍历优先)

**先来看看我和gpt的一些问答**

**Q1:**前序，中序，后序遍历是什么，为我详细解答一下

**A1:** 前序遍历、中序遍历和后序遍历是二叉树遍历的三种常见方式，用于按照特定顺序访问二叉树中的节点。

1. 前序遍历（Preorder Traversal）：
   前序遍历先访问根节点，然后按照先左后右的顺序递归地遍历左子树和右子树。即根节点 -> 左子树 -> 右子树。

2. 中序遍历（Inorder Traversal）：
   中序遍历先递归地遍历左子树，然后访问根节点，最后递归地遍历右子树。即左子树 -> 根节点 -> 右子树。

3. 后序遍历（Postorder Traversal）：
   后序遍历先递归地遍历左子树，然后递归地遍历右子树，最后访问根节点。即左子树 -> 右子树 -> 根节点。

这三种遍历方式都是通过递归或栈的辅助实现的。它们可以用来处理二叉树的节点，进行节点值的访问、打印、复制等操作。

需要注意的是，每种遍历方式在访问节点的顺序和操作时机上有所区别，因此选择适当的遍历方式取决于具体的需求和问题。例如，前序遍历适用于创建二叉树的复制、打印树的结构等；中序遍历适用于搜索树中的节点或按节点值的大小进行排序；后序遍历适用于释放二叉树的内存等操作。

对于一个给定的二叉树，可以根据需求选择合适的遍历方式来处理树的节点。

---

**Q2:** 这些顺序的命名由遍历根的顺序决定吗？ 

**A2:** 是的，这些遍历方式的命名是根据**根节点的访问顺序**来命名的。

- 前序遍历："前"指的是根节点的访问顺序位于左子树和右子树之前。
- 中序遍历："中"指的是根节点的访问顺序位于左子树和右子树之间。
- 后序遍历："后"指的是根节点的访问顺序位于左子树和右子树之后。

这样的命名方式有助于描述节点的访问顺序，并且在讨论二叉树遍历时能够更清晰地表达所采用的遍历策略。

---

**Q3:**不太理解这段代码

```java
/* 前序遍历 */
void preOrder(TreeNode root) {
    if (root == null)
        return;
    // 访问优先级：根节点 -> 左子树 -> 右子树
    list.add(root.val);
    preOrder(root.left);
    preOrder(root.right);
}
```

**A3:**  这段代码实现了二叉树的前序遍历算法。让我逐步解释它的工作原理：

1. 首先，我们判断根节点是否为空，如果为空，则直接返回。
2. 如果根节点不为空，我们首先访问当前节点（即将当前节点的值添加到列表中）。
3. 然后，递归地对左子树进行前序遍历，即调用 `preOrder(root.left)`。
4. 接着，递归地对右子树进行前序遍历，即调用 `preOrder(root.right)`。

这个过程可以形象地描述为"根节点 -> 左子树 -> 右子树"的访问顺序。每个节点的访问顺序是先访问根节点，然后递归地按照前序遍历顺序访问左子树和右子树。

通过递归调用，我们可以遍历整棵二叉树，并将节点的值按前序遍历的顺序添加到列表中。

---

**Q4:**  这里的递归，相当于把左右节点当成一个新的根，左右子树看成新的树，再先遍历根，直到最后传入的左右节点为空为止是吗

**A4:** 正解！在前序遍历算法中，我们首先访问当前节点，然后递归地对左子树和右子树进行前序遍历。每次递归调用时，左子节点和右子节点都被视为新的根节点，然后继续按照前序遍历的方式遍历它们的左右子树。这个过程会一直进行下去，直到遇到空节点为止，然后回溯到上一层继续遍历其他子树。

可以将递归过程看作是对每个子树的遍历，每个子树的根节点会作为新的起点进行遍历。递归调用的结束条件是遇到空节点，表示已经遍历完该子树。

通过不断地将左右子节点作为新的根节点进行遍历，我们可以遍历整棵二叉树的所有节点，并按照前序遍历的顺序访问它们。

---

**代码实现**

```java
package main.java.learn.theme.treedemo.demo2;

import main.java.learn.theme.treedemo.demo1.TreeNode;

import java.util.ArrayList;
import java.util.List;

public class Test02 {

    static List<Integer> list1 = new ArrayList<>();
    static List<Integer> list2 = new ArrayList<>();
    static List<Integer> list3 = new ArrayList<>();

    public static void main(String[] args) {
        //创建二叉树
        TreeNode treeNode1 = new TreeNode(1);
        TreeNode treeNode2 = new TreeNode(2);
        TreeNode treeNode3 = new TreeNode(3);
        TreeNode treeNode4 = new TreeNode(4);
        TreeNode treeNode5 = new TreeNode(5);
        TreeNode treeNode6 = new TreeNode(6);
        TreeNode treeNode7 = new TreeNode(7);
        treeNode1.setLeft(treeNode2);
        treeNode1.setRight(treeNode3);
        treeNode2.setLeft(treeNode4);
        treeNode2.setRight(treeNode5);
        treeNode3.setLeft(treeNode6);
        treeNode3.setRight(treeNode7);
        //前序遍历法
        method1(treeNode1);
        System.out.println("前序遍历法");
        printList(list1);
        System.out.println("\n");
        //中序遍历法
        method2(treeNode1);
        System.out.println("中序遍历法");
        printList(list2);
        System.out.println("\n");
        //中序遍历法
        method3(treeNode1);
        System.out.println("后序遍历法");
        printList(list3);
        System.out.println("\n");

    }

    private static void printList(List<Integer> list) {
        for (int i : list) {
            System.out.print(i + "->");
        }
        System.out.print("End");
    }

    private static void method1(TreeNode root) {
        if (root == null) {
            return;
        } else {
            list1.add(root.getValue());
            method1(root.getLeft());
            method1(root.getRight());
        }
    }

    private static void method2(TreeNode root) {
        if (root == null) {
            return;
        } else {
            method2(root.getLeft());
            list2.add(root.getValue());
            method2(root.getRight());
        }
    }
    private static void method3(TreeNode root) {
        if (root == null) {
            return;
        } else {
            method3(root.getLeft());
            method3(root.getRight());
            list3.add(root.getValue());
        }
    }
}

```

结果打印：

前序遍历法
1->2->4->5->3->6->7->End

中序遍历法
4->2->5->1->6->3->7->End

后序遍历法
4->5->2->6->7->3->1->End

---

## 二叉搜索树（BinarySearchTree)

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/20230718142502.png)

### 二叉搜索树的构造

**BinarySearchTree类**

```java
package main.java.learn.theme.treedemo.demo3;

import main.java.learn.theme.treedemo.demo1.TreeNode;

import java.util.ArrayList;
import java.util.List;

public class BinarySearchTree {
    private TreeNode root;
    List<Integer> list = new ArrayList<>();
    public BinarySearchTree() {
        this.root = null;
    }

    public TreeNode getRoot() {
        return root;
    }

    private TreeNode insertNode(TreeNode root, int value) {
        //如果二叉树没有生成，传入的第一个值作为根值
        if (root == null) {
            return new TreeNode(value);
        }
        if (root.getValue() < value) {
            //如果传入的值大于根值，需要进入右节点,递归插入
            root.setRight(insertNode(root.getRight(),value));
        }else if(root.getValue() > value) {
            //同理，递归插入左节点
            root.setLeft(insertNode(root.getLeft(),value));
        }
        return root;
    }

    public void buildTree(int[] nums) {
        for(int val : nums) {
            root = insertNode(root,val);
        }
    }
    //中序遍历法
    public List<Integer> inorderTraversal(TreeNode root) {

        if(root == null) {
            return null;
        }else {
            inorderTraversal(root.getLeft());
            list.add(root.getValue());
            inorderTraversal(root.getRight());
        }
        return list;
    }
}

```

通过递归插入节点，例如当第一次插入节点时，该节点作为根节点，当插入下一个节点时，根据节点大小的比较，对应插入到当前根节点的左/右节点，并且利用递归的思想，将插入的新的节点作为新的根节点，再去进一步判断，这样，只需要知道一个根节点，就能获取到一颗二叉搜索树，并且通过中序遍历法验证，得到一个升序的序列

**通过逐步调试可以发现，这个构造原理就是通过不断的将新插入的值与当前节点值比较，一直向下寻找合适的位置，如果当前“根”节点不为空，那么就要继续向下，判断要插在左还是右，如果此时节点为null，那么就有了属于当前值的位置，插入即可**

---

**Test类**

```java
package main.java.learn.theme.treedemo.demo3;

import main.java.learn.theme.treedemo.demo1.TreeNode;

import java.util.ArrayList;
import java.util.List;

public class Test01 {
    public static void main(String[] args) {
        int[] arr = {8, 4, 12, 2, 6, 10, 14, 1, 3, 5, 7, 9, 11, 13, 15};
        int[] values = {5, 3, 8, 2, 4, 7, 10, 1, 6, 9, 12, 15, 11, 13, 14};
        BinarySearchTree binarySearchTree = new BinarySearchTree();
        binarySearchTree.buildTree(arr);
        List<Integer> list = binarySearchTree.inorderTraversal(binarySearchTree.getRoot());
        for (int i : list) {
            System.out.print(i + " ");
        }
    }
}

```

**输出结果:**

1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 

---

### 二叉搜索树的查找

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/20230722111952.png)

**代码实现**

**Test02**

```java
package main.java.learn.theme.treedemo.demo3;

import main.java.learn.theme.treedemo.demo1.TreeNode;

public class Test02 {
    public static void main(String[] args) {
        int[] arr = {8, 4, 12, 2, 6, 10, 14, 1, 3, 5, 7, 9, 11, 13, 15};
        BinarySearchTree tree = new BinarySearchTree();
        tree.buildTree(arr);
        TreeNode root = tree.getRoot();
        TreeNode resTreeNode = search(root, 7);
    }

    public static TreeNode search(TreeNode root, int num) {
        int count = 1;
        if (root != null) {
            while (true) {
                if (root.getValue() == num) {
                    System.out.println("find num " + root.getValue() + " in level " + count);
                    return root;
                } else if (root.getValue() < num) {
                    root = root.getRight();
                    count++;
                } else {
                    root = root.getLeft();
                    count++;
                }
            }
        } else {
            System.out.println("Root is null");
            return null;
        }
    }
}
```

---

输出结果

find num 7 in level 4

---

### 二叉搜索树的删除操作

删除比较复杂，需要分析所删除的节点的度数，进行分类讨论

---

#### 度数为0（叶节点)

当度数为0时，只需要通过查找的方法，找到节点，判断是否为叶子，然后根据其位于pre节点的左或右进行删除即可

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307231038366.png)

---

#### 度数为1

当度数为1时，需要根据左右位置，进行覆盖操作来达到删除效果

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307231136430.png)

---

#### 度数为2时

度数为2比较复杂，需要保证二叉搜索树，左根右的一个升序，所以在找到删除节点之后要去找到该节点的左子树的右节点，或者该节点右子树的左节点,并且先记录后递归删除，再进行值替换

![](https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202307231154332.png)

---

**整体代码**

**Test04**

```java
package main.java.learn.theme.treedemo.demo3;

import main.java.learn.theme.treedemo.demo1.TreeNode;

import java.util.ArrayList;
import java.util.List;

//  删除节点
public class Test04 {
    static List<Integer> list = new ArrayList<>();

    public static void main(String[] args) {
        TreeNode root = BinarySearchTree.getBuiltTree();

        //printTree(root);

        method1(root);
        method2(root);
        method3(root);
    }

    private static void method3(TreeNode root) {
        //测试是否能正常删除度数为2的节点
        deleteNode(root, 4);
        System.out.println("delete 4");
        printTree(root);
    }

    private static void method2(TreeNode root) {
        //测试是否能正常删除度数为1的节点
        deleteNode(root, 2);
        System.out.println("delete 2");
        printTree(root);
    }

    private static void method1(TreeNode root) {
        //测试是否能正常删除叶节点
        deleteNode(root, 1);
        System.out.println("delete 1");
        printTree(root);
    }

    private static void deleteNode(TreeNode root, int val) {
        TreeNode preNode = root;
        while (true) {
            if (root != null) {
                if (root.getValue() == val) {
                    //当前节点为所删除节点
                    if (isLeaf(root)) {
                        //如果是叶节点
                        //根据左右节点,直接删除
                        if (preNode.getValue() > val) {
                            preNode.setLeft(null);
                        } else if (preNode.getValue() < val) {
                            preNode.setRight(null);
                        }
                        return;
                    } else if (isOneLeaf(root)) {
                        //节点度数为1
                        //根据左右节点，覆盖
                        if (preNode.getValue() > val) {
                            //root在pre的左节点位置
                            //判断左右哪个为null
                            if (root.getLeft() == null) {
                                //使用右节点
                                preNode.setLeft(root.getRight());
                            } else if (root.getRight() == null) {
                                //使用左节点
                                preNode.setLeft(root.getLeft());
                            }
                            return;
                        } else if (preNode.getValue() < val) {
                            //root在pre的右节点
                            if (root.getLeft() == null) {
                                //使用右节点
                                preNode.setRight(root.getRight());
                            } else if (root.getRight() == null) {
                                //使用左节点
                                preNode.setRight(root.getLeft());
                            }
                            return;
                        }
                    } else if (isTwoLeaf(root)) {
                        //2度节点，采取右子树左节点替换法
                        //获取右子树左节点
                        TreeNode nex = root.getRight().getLeft();
                        //递归删除右子树左节点（不知道是叶还是1度还是2度）
                        deleteNode(root, nex.getValue());
                        //将nex节点替换到root节点
                        root.setValue(nex.getValue());
                        return;
                    }

                } else if (root.getValue() < val) {
                    //所要查找的节点在右边
                    preNode = root;
                    root = root.getRight();
                } else {
                    //所要查找的节点在左边
                    preNode = root;
                    root = root.getLeft();
                }
            } else {
                return;
            }
        }
    }

    private static boolean isLeaf(TreeNode root) {
        if (root.getLeft() == null && root.getRight() == null) {
            return true;
        } else {
            return false;
        }
    }

    private static boolean isOneLeaf(TreeNode root) {
        if (!isLeaf(root) && !isTwoLeaf(root)) {
            return true;
        } else {
            return false;
        }
    }

    private static boolean isTwoLeaf(TreeNode root) {
        if (root.getLeft() != null && root.getRight() != null) {
            return true;
        } else {
            return false;
        }
    }

    private static void printTree(TreeNode root) {
        if (list.isEmpty()) {
            emptyPrintTree(root);
        } else {
            list.clear();
            emptyPrintTree(root);
        }

    }

    private static void emptyPrintTree(TreeNode root) {
        list = inorderTraversal(root);
        for (int i : list) {
            System.out.print(i + " ");
        }
        System.out.print("\n");
    }

    public static List<Integer> inorderTraversal(TreeNode root) {

        if (root == null) {
            return null;
        } else {
            inorderTraversal(root.getLeft());
            list.add(root.getValue());
            inorderTraversal(root.getRight());
        }
        return list;
    }
}
```

打印结果

delete 1
2 3 4 5 6 7 8 9 10 11 12 13 14 15 
delete 2
3 4 5 6 7 8 9 10 11 12 13 14 15 
delete 4
3 5 6 7 8 9 10 11 12 13 14 15 

这里写的时候，中间遇到了一些与树无关的一些问题，比如循环，return，还有采取static list必须要每次判断是否为empty，不然打印效果不好

---

