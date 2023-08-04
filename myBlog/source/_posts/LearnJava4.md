---
title: Java学习
tags: [Java，Set,Map]
categories: [学习]
cover: https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202308021631325.png
swiper_index: 2 #置顶轮播图顺序，非负整数，数字越大越靠前
description: Java学习记录




---

Queue的异常体系

<!-- more -->

# demo1(这个demo是关于queue的)

## _1.Queue的异常体系

```java
package main.java.learn.date.August2023.th4.demo1._1;

import java.util.LinkedList;
import java.util.Queue;

public class Test {
    public static void main(String[] args) {
        //LinkedList既实现了List接口又实现了Queue接口
        Queue<Integer> queue = new LinkedList<>();
        //异常体系
        //添加操作（尾部）
        try{
           queue.add(123);
            System.out.println("add successful");
        }catch (IllegalStateException e) {
            e.printStackTrace();
        }
        //获取头部元素
        try{
            Integer element = queue.element();
            System.out.println("element : " + element);
        }catch (IllegalStateException e) {
            e.printStackTrace();
        }
        //获取头部元素并删除
        try{
            Integer remove = queue.remove();
            System.out.println("remove : " + remove);
        }catch (IllegalStateException e){
            e.printStackTrace();
        }
        //再次获取和获取并删除都会抛出异常
        //获取头部元素
        try{
            Integer element = queue.element();
        }catch (IllegalStateException e) {
            e.printStackTrace();
        }
        //获取头部元素并删除
        try{
            Integer remove = queue.remove();
        }catch (IllegalStateException e){
            e.printStackTrace();
        }
    }
}

```

## _2返回false或null

 ```java
 package main.java.learn.date.August2023.th4.demo1._2;
 
 import java.util.LinkedList;
 import java.util.Queue;
 
 public class Test {
     public static void main(String[] args) {
         Queue<String> queue = new LinkedList<>();
         //返回false或null
         //添加操作
         boolean isOffered = queue.offer("abc");
         //获取首部元素
         String peek = queue.peek();
         System.out.println(peek);
         //获取首部元素并删除
         String poll = queue.poll();
         //再次获取为空
         System.out.println(queue.peek());
         System.out.println(queue.poll());
     }
 }
 
 ```

---

# demo2(PriorityQueue)

## _1(String实现了comparable接口，可以使用PriorityQueue)

```java
package main.java.learn.date.August2023.th4.demo2._1;

import java.util.PriorityQueue;
import java.util.Queue;

public class Test {
    public static void main(String[] args) {
        Queue<String> queue = new PriorityQueue<>();
        queue.add("apple");
        queue.add("peach");
        queue.add("banana");
        while (!queue.isEmpty()) {
            System.out.println(queue.poll());
        }
    }
}

```

## _2

---

与gpt的问答

用PriorityQueue实现银行排队业务

### Comparator 和 Comparable的区别

`Comparator` 和 `Comparable` 都是用于比较对象的接口，但它们有不同的用途和实现方式：

1. `Comparable` 接口：
    - 位于 `java.lang` 包下。
    - 实现了 `Comparable` 接口的类可以通过实现 `compareTo()` 方法来定义对象之间的自然顺序（默认顺序）。
    - `compareTo()` 方法返回一个整数，表示对象之间的比较结果。若返回负数，表示当前对象小于比较对象；若返回零，表示两个对象相等；若返回正数，表示当前对象大于比较对象。
    - 使用 `Collections.sort()` 或 `Arrays.sort()` 等方法时，如果集合中的元素实现了 `Comparable` 接口，会按照定义的自然顺序进行排序。

2. `Comparator` 接口：
    - 位于 `java.util` 包下。
    - 实现了 `Comparator` 接口的类可以通过实现 `compare()` 方法来定义不同的比较规则，实现自定义的排序方式。
    - `compare()` 方法返回一个整数，表示两个对象之间的比较结果，规则与 `compareTo()` 相同。
    - `Comparator` 接口适用于对已有的类进行比较操作，或者在不修改类本身的情况下，定义多种排序方式。

例如，假设有一个 `Person` 类，想对其进行排序，可以选择实现 `Comparable` 接口来定义对象之间的自然排序，或者使用 `Comparator` 接口来定义多种排序方式，如按照年龄、姓名等进行排序。

示例：

```java
class Person implements Comparable<Person> {
    private String name;
    private int age;

    // 构造方法等...

    // 实现 Comparable 接口的 compareTo 方法，按照年龄进行排序
    @Override
    public int compareTo(Person other) {
        return this.age - other.age;
    }
}

// 使用 Comparator 接口来定义按照姓名进行排序
class NameComparator implements Comparator<Person> {
    @Override
    public int compare(Person p1, Person p2) {
        return p1.getName().compareTo(p2.getName());
    }
}
```

在使用时，可以根据需求选择合适的比较方式来对对象进行排序。

**Question
就是如果我有一个Person类，然后我就可以在这个类实现comparable接口，如果我还想要有更多的排序方法，就另外再去写一个类实现comparator接口，然后在集合里传入这个类是吗？**

是的，你理解得很正确！

如果你在 `Person` 类中实现了 `Comparable` 接口，那么你可以定义对象之间的自然排序方式，这个排序方式将成为默认的排序方式。

如果你还想定义其他不同的排序方式，可以通过实现 `Comparator` 接口来创建新的比较器类。你可以根据不同的排序需求编写不同的比较器类，例如按照姓名、年龄、身高等属性进行排序。

然后，你可以在集合中使用 `Collections.sort()` 或 `Arrays.sort()` 方法时，传入对应的比较器对象，以选择特定的排序方式进行排序。

示例：

```java
class Person implements Comparable<Person> {
    // ... 其他属性和方法 ...

    // 实现 Comparable 接口的 compareTo 方法，按照年龄进行排序
    @Override
    public int compareTo(Person other) {
        return this.age - other.age;
    }
}

// 使用 Comparator 接口来定义按照姓名进行排序
class NameComparator implements Comparator<Person> {
    @Override
    public int compare(Person p1, Person p2) {
        return p1.getName().compareTo(p2.getName());
    }
}

// 使用 Comparator 接口来定义按照身高进行排序
class HeightComparator implements Comparator<Person> {
    @Override
    public int compare(Person p1, Person p2) {
        return p1.getHeight() - p2.getHeight();
    }
}
```

然后在使用时，你可以根据具体的排序需求选择对应的比较器类来进行排序：

```java
List<Person> people = new ArrayList<>();
// 添加人员信息...

// 按照年龄进行排序（使用对象的自然排序）
Collections.sort(people);

// 按照姓名进行排序
Collections.sort(people, new NameComparator());

// 按照身高进行排序
Collections.sort(people, new HeightComparator());
```

通过这样的方式，你可以灵活地实现多种排序方式，而不需要修改 `Person` 类的实现。这样的设计使得代码更加灵活、可维护性更高。

### gpt解释代码


这段代码实现了一个自定义的 `UserComparator` 类，该类实现了 `Comparator<User>` 接口，用于比较两个 `User` 对象的大小。比较的规则如下：

1. 如果 `o1` 的 `id` 字符串以 `'V'` 开头而 `o2` 的 `id` 字符串以 `'A'` 开头，则 `o1` 大于 `o2`，返回 `-1`，表示 `o1` 排在 `o2` 前面。

2. 如果 `o1` 的 `id` 字符串以 `'A'` 开头而 `o2` 的 `id` 字符串以 `'V'` 开头，则 `o1` 小于 `o2`，返回 `1`，表示 `o1` 排在 `o2` 后面。

3. 如果 `o1` 和 `o2` 的 `id` 字符串相同，那么比较它们的 `num` 字段（数字部分）。将 `num` 字符串转换为整数，并比较两个整数的大小。如果 `num1` 大于 `num2`，则 `o1` 大于 `o2`，返回 `1`，表示 `o1` 排在 `o2` 后面；如果 `num1` 小于 `num2`，则 `o1` 小于 `o2`，返回 `-1`，表示 `o1` 排在 `o2` 前面。

4. 如果 `o1` 和 `o2` 的 `id` 字符串既不是 `'V'` 开头也不是 `'A'` 开头，或者 `num` 字段无法转换为整数，则抛出运行时异常，表示无法进行比较。

这个自定义的比较器根据 `User` 对象的 `id` 字符串和 `num` 字段实现了多级排序，首先根据 `id` 的 `'V'` 和 `'A'` 字符进行等级判断，然后根据 `num` 字段进行数字大小比较。在使用这个比较器时，可以通过传入 `UserComparator` 对象来实现不同的排序需求。

**是不是比如说我比较的时候想把前者放在后面，就返回1，这样说明前者大于后者，这样就会把前者放到后者的后面？**

对的，你说得很对！在自定义的比较器中，返回 `1` 表示前者大于后者，这样在排序时，前者会被放到后者的后面。返回 `-1` 表示前者小于后者，这样在排序时，前者会被放到后者的前面。通过自定义比较器的返回值，可以控制排序的顺序，从而实现不同的排序需求。

---

**User类**

```java
package main.java.learn.date.August2023.th4.demo2._2;

public class User {
    private String name;
    private String id;

    public User(String name, String number) {
        this.name = name;
        this.id = number;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getNum() {
        //返回第一个元素之后的元素（数字）
        return id.substring(1);
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", id='" + id + '\'' +
                '}';
    }
}

```

**UserComparator**

```java
package main.java.learn.date.August2023.th4.demo2._2;

import java.util.Comparator;

public class UserComparator implements Comparator<User> {
    @Override
    public int compare(User o1, User o2) {
        //有两种等级，V大于A，先判断等级，在判断数字
        if (o1.getId().charAt(0) == 'V' && o2.getId().charAt(0) == 'A') {
            return -1;
        } else if (o1.getId().charAt(0) == 'A' && o2.getId().charAt(0) == 'V') {
            return 1;
        } else if (o1.getId().charAt(0) == o2.getId().charAt(0)) {
            //同级比较数字
            int num1 = 0;
            int num2 = 0;
            try {
                num1 = Integer.parseInt(o1.getNum());
                num2 = Integer.parseInt(o2.getNum());
                return num1 > num2 ? 1 : -1;
            } catch (NumberFormatException e) {
                throw new RuntimeException(e);
            }
        }
        throw new RuntimeException("Can not be compared,please check");
    }
}

```

**Test**

```java
package main.java.learn.date.August2023.th4.demo2._2;

import java.util.PriorityQueue;
import java.util.Queue;

public class Test {
    public static void main(String[] args) {
        //创建Queue集合，并且传入比较器
        Queue<User> userQueue = new PriorityQueue<>(new UserComparator());
        //添加数据
        userQueue.offer(new User("Alice","A1"));
        userQueue.offer(new User("Bob","A3"));
        userQueue.offer(new User("Cathy","A2"));
        userQueue.offer(new User("David","A5"));
        userQueue.offer(new User("Elle","A42"));
        userQueue.offer(new User("Frank","V2"));
        userQueue.offer(new User("George","V1"));
        userQueue.offer(new User("Harden","A12"));
        userQueue.offer(new User("Imy","A15"));

        while(!userQueue.isEmpty()) {
            System.out.println(userQueue.poll());
        }
    }
}

```

**测试结果**

```
User{name='George', id='V1'}
User{name='Frank', id='V2'}
User{name='Alice', id='A1'}
User{name='Cathy', id='A2'}
User{name='Bob', id='A3'}
User{name='David', id='A5'}
User{name='Harden', id='A12'}
User{name='Imy', id='A15'}
User{name='Elle', id='A42'}
```

---

# demo3(Deque)

## _1

---

Deque的基本操作  
明确的指出First和Last  
体会LinkedList这个具体实现类的强大性  
理解抽象编程，尽量持有接口

---

```java
package main.java.learn.date.August2023.th4.demo3._1;

import java.util.Deque;
import java.util.LinkedList;

public class Test {
    public static void main(String[] args) {
        //用Deque接口接收具体实现类实例
        Deque<String> deque = new LinkedList<>();
        method1(deque);
        //方法2:offer(F,L) peek(F,L),poll(F,L)
        //基本相似只是返回类型不同
    }

    private static void method1(Deque<String> deque) {
        //方法一:add , remove , get ，配合FirstLast
        //添加操作
        deque.addFirst("A");//A
        deque.addFirst("B");//B-A
        deque.addLast("C");//B-A-C
        deque.addLast("D");//B-A-C-D
        //打印查看
        //printDeque(deque);
        //get操作（查看不删除，类比element和remove）
        try {
            System.out.println("First = " + deque.getFirst());//B
            System.out.println("Last  = " + deque.getLast());//D
        } catch (IllegalStateException e) {
            throw new RuntimeException("无元素");
        }
        //获取并删除操作
        try {
            System.out.println(deque.removeFirst() + " \n");//移除B，A-C-D
            System.out.println(deque.removeLast() + "\n");//移除D，A-C
        } catch (IllegalStateException e) {
            throw new RuntimeException("无元素");
        }
        //打印查看，同时内部实现清空测试异常
        printDeque(deque);
        try {
            deque.getFirst();
        } catch (IllegalStateException e) {
            throw new RuntimeException("无元素");
        }
        try {
            deque.removeLast();
        } catch (IllegalStateException e) {
            throw new RuntimeException("无元素");
        }
    }

    public static void printDeque(Deque<String> deque) {
        while (!deque.isEmpty()) {
            System.out.println(deque.poll());
        }
    }
}

```

---

# demo4(Stack栈)

## _1

---

栈由deque实现，用的时候仅调用   
push（入栈）  
pop（出栈）  
peek（查看栈顶）

---

```java
package main.java.learn.date.August2023.th4.demo4._1;

import java.util.Deque;
import java.util.LinkedList;
import java.util.Stack;

public class Test {
    public static void main(String[] args) {
        //不要使用遗留类
        //Stack<Integer> stack = new Stack<>();
        //用Deque实现栈
        Deque<Integer> stack = new LinkedList<>();
        //入栈操作
        stack.push(1);
        //查看栈顶
        System.out.println(stack.peek());
        //弹出
        stack.pop();
    }
}

```

## _2(用栈实现整形的进制转换 10-16 )

---


 二进制是Binary，简写为B

八进制是Octal，简写为O

十进制为Decimal，简写为D

十六进制为Hexadecimal，简写为H

---

**Solution**

```java
package main.java.learn.date.August2023.th4.demo4._2;

import java.util.Deque;
import java.util.LinkedList;

public class Solution {
    public static void main(String[] args) {
        //创建栈
        Deque<String> stack = new LinkedList<>();
        //10->A,11->B,A的ASCII码是65，则10-> (char)55+10
        //利用循环先让商入栈，余数继续进行循环
        dToH(stack, 12500);
        while (!stack.isEmpty()) {
            System.out.print(stack.pop());
        }
    }

    public static void dToH(Deque<String> stack, int decimal) {
        int quotient = decimal;
        int mod = 0;
        while (quotient != 0) {
            mod = quotient % 16;
            if (mod >= 0 && mod < 10) {
                //数字直接入栈
                stack.push((String.valueOf(mod)));
            } else {
                //处理后入栈
                stack.push((String.valueOf((char) (mod + 55))));
            }
            quotient = quotient / 16;
        }
    }
}

```

## _4* (利用Stack把字符串中缀表达式编译为后缀表达式，然后再利用栈执行后缀表达式获得计算结果)

```java
package main.java.learn.date.August2023.th4.demo4._3;

import java.util.*;

public class Main {
    public static void main(String[] args) {
        String exp = "1+2*(9-5)";
        SuffixExpression se = compile(exp);
        int result = se.execute();
        System.out.println(exp + " = " + result + " " + (result == 1 + 2 * (9 - 5) ? "✓" : "✗"));
    }

    static SuffixExpression compile(String exp) {
        // TODO:
        // 创建一个存放运算符的栈
        Deque<Character> operatorStack = new LinkedList<>();
        // 创建一个存放后缀表达式的栈
        Deque<Character> postfixStack = new LinkedList<>();
        // 遍历中缀表达式的每个字符
        char[] charArray = exp.toCharArray();
        for (char ch : charArray) {
            // 如果遇到了数字
            if (ch <= '9' && ch >= '0') {
                // 添加到后缀表达式栈
                postfixStack.push(ch);
            } else if (ch == '(') {
                // 遇到左括号,压入运算符栈
                operatorStack.push(ch);
            } else if (ch == ')') {
                // 遇到右括号,利用循环弹出运算符栈的符号并存储到后缀运算符栈，直到遇到左括号，并将左括号弹出
                while (!operatorStack.isEmpty() && operatorStack.peek() != '(') {
                    postfixStack.push(operatorStack.pop());
                }
                // 弹出左括号
                operatorStack.pop();
            } else if (ch == '+' || ch == '-' || ch == '*' || ch == '/') {
                // 先简单处理四则运算,判断与栈顶的优先级
                // 如果为空，则直接压入
                if (operatorStack.isEmpty()) {
                    operatorStack.push(ch);
                } else {
                    while (!operatorStack.isEmpty() && operatorStack.peek() != '(' &&
                            priorityNum(ch) <= priorityNum(operatorStack.peek())) {
                        // 弹出栈顶运算符到后缀表达式栈，直到运算符栈为空或栈顶为左括号，忽略括号
                        postfixStack.push(operatorStack.pop());
                    }
                    // 将ch压入运算符栈
                    operatorStack.push(ch);
                }
            }
        }
        // 遍历完运算字符串后，将剩下的运算符取出并弹入后缀表达式栈
        while (!operatorStack.isEmpty()) {
            postfixStack.push(operatorStack.pop());
        }
        return new SuffixExpression(postfixStack);
    }

    public static int priorityNum(char c) {
        if (c == '+' || c == '-') {
            return 1;
        } else if (c == '*' || c == '/') {
            return 2;
        } else if (c == '(' || c == ')') {
            //如果是（）
            return 3;
        }
        return 0;
    }
}

class SuffixExpression {
    private Deque<Character> postfixStack = new LinkedList<>();

    SuffixExpression(Deque<Character> postfixStack) {
        this.postfixStack = postfixStack;
    }

    int execute() {
        //创建计算用的栈
        Deque<Character> reverseStack = new LinkedList<>();
        Deque<Character> calculateStack = new LinkedList<>();
        //先处理反转栈
        while (!postfixStack.isEmpty()) {
            reverseStack.push(postfixStack.pop());
        }
        // TODO:
        //遍历后缀表达式
        while (!reverseStack.isEmpty()) {
            char ch = reverseStack.peek();
            if (ch >= '0' && ch <= '9') {
                //数字直接入栈
                calculateStack.push(ch);
                //反转栈同时出栈
                reverseStack.pop();
            }
            if (ch == '+' || ch == '-' || ch == '*' || ch == '/') {
                //反转栈同时出栈
                reverseStack.pop();
                //如果是运算符，弹出两个数字进行计算,并将计算结果压入栈
                int firstNum = calculateStack.pop() - '0';
                int secondNum = calculateStack.pop() - '0';
                switch (ch) {
                    case '+' -> {
                        calculateStack.push((char) ((firstNum + secondNum) + 48));
                    }
                    case '-' -> {
                        calculateStack.push((char) ((secondNum - firstNum) + 48));
                    }
                    case '*' -> {
                        calculateStack.push((char) ((firstNum * secondNum) + 48));
                    }
                    case '/' -> {
                        calculateStack.push((char) ((secondNum / firstNum) + 48));
                    }
                }
            }
        }
        //返回最终剩余的元素就是计算结果
        return calculateStack.pop() - 48;
    }
}

```

中间调试的时候一直出问题，最后发现是处理左括号的时候，直接就从operator的栈里面删除了，gpt修改之后确实解决了这个问题

---

GPT ：当我进行改进时，主要关注的是如何更简洁和准确地处理运算符的优先级以及左括号的处理。我会详细解释两个写法的差别：

原始代码块（我写的）：
```java
if (priorityNum(ch) <= priorityNum(operatorStack.peek())) {
    //如果ch的运算级别小于运算符栈顶的优先级
    while (!operatorStack.isEmpty()) {
        //推出栈顶运算符到后缀表达式栈,直到运算符栈为空,忽略括号
        if (operatorStack.peek() == '(' || operatorStack.peek() == ')') {
            operatorStack.pop();
            f = true;
        } else {
            postfixStack.push(operatorStack.pop());
        }
    }
    //将ch压入运算符栈
    if (f) {
        //补充左括号
        operatorStack.push('(');
    }
    operatorStack.push(ch);
} else {
    //运算级高，不用弹出,直接压入栈
    operatorStack.push(ch);
}
```

改进后的代码块：
```java
while (!operatorStack.isEmpty() && operatorStack.peek() != '(' &&
       priorityNum(ch) <= priorityNum(operatorStack.peek())) {
    // 弹出栈顶运算符到后缀表达式栈，直到运算符栈为空或栈顶为左括号，忽略括号
    postfixStack.push(operatorStack.pop());
}
// 将ch压入运算符栈
operatorStack.push(ch);
```

主要的改进在于以下几点：

1. 改进后的代码通过一个循环实现了连续地弹出运算符栈的栈顶元素，并将这些运算符压入后缀表达式栈，直到运算符栈为空或栈顶元素为左括号为止。这样，我们只需要一个循环就能处理所有优先级小于等于当前运算符的情况。而在原始代码中，为了处理这一情况，使用了一个 `while` 循环和一个额外的标志变量 `f`。

2. 改进后的代码更加简洁明了，没有涉及额外的条件判断和标志变量，使得逻辑更加清晰。

3. 改进后的代码在处理左括号时，直接忽略了左括号，而在原始代码中，将左括号也压入了后缀表达式栈。在处理后缀表达式时，我们不需要考虑左括号，因为它们只用于优先级的比较。

总的来说，改进后的代码更加简洁高效，并且更符合对后缀表达式计算的要求。在实现算法时，优先考虑使用简洁和清晰的逻辑，可以使代码更易于理解和维护。

---