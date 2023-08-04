---
title: Java集合2
tags: [Java，Set,Map]
categories: [学习]
cover: https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202308021631325.png
swiper_index: 2 #置顶轮播图顺序，非负整数，数字越大越靠前
description: Java学习记录



---

Map基本操作,Map的遍历操作,Map的equals和hashCode重写,Key的重要性（需要被重写equals和hashCode），TreeMap
以及compare的复习，Set

<!-- more -->

# Map

## Map基本操作

**Stu类**

```java
package main.java.learn.date.August2023.rd3.demo1;

public class Stu {
    private String name;
    private int grade;

    public Stu(String name, int age) {
        this.name = name;
        this.grade = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getGrade() {
        return grade;
    }

    public void setGrade(int grade) {
        this.grade = grade;
    }
}

```

**Test**

```java
package main.java.learn.date.August2023.rd3.demo1;

import java.util.HashMap;
import java.util.Map;

public class Test {
    public static void main(String[] args) {
        //根据学生姓名查询学生并获得成绩
        Map<String,Stu> map = new HashMap<>();
        map.put("Alan",new Stu("Alan",99));
        Stu stu1 = map.get("Alan");
        System.out.println(stu1.getGrade());
    }
}
```

---

## Map的遍历操作

```java
package main.java.learn.date.August2023.rd3.demo2;

import java.util.HashMap;
import java.util.Map;

public class Test01 {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 123);
        map.put("pear", 456);
        map.put("banana", 789);
        //method1(map);
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey());
            System.out.println(entry.getValue());
        }
    }

    private static void method1(Map<String, Integer> map) {
        //先获取key的集合（set），再去遍历
        for (String key : map.keySet()) {
            Integer num = map.get(key);
            System.out.println(num);
        }
    }
}

```

### 测试题（廖雪峰老师的教程中）

```java
package main.java.learn.date.August2023.rd3.demo2;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public static void main(String[] args) {
        List<Student> list = List.of(
                new Student("Bob", 78),
                new Student("Alice", 85),
                new Student("Brush", 66),
                new Student("Newton", 99));
        var holder = new Students(list);
        System.out.println(holder.getScore("Bob") == 78 ? "测试成功!" : "测试失败!");
        System.out.println(holder.getScore("Alice") == 85 ? "测试成功!" : "测试失败!");
        System.out.println(holder.getScore("Tom") == -1 ? "测试成功!" : "测试失败!");
    }
}


class Students {
    List<Student> list;
    Map<String, Integer> cache;

    Students(List<Student> list) {
        this.list = list;
        cache = new HashMap<>();
    }

    /**
     * 根据name查找score，找到返回score，未找到返回-1
     */
    int getScore(String name) {
        // 先在Map中查找:
        Integer score = this.cache.get(name);
        if (score == null) {
            // TODO:
            //Find in the list
            score = findInList(name);
            if(score != null) {
                //在list中查找到但是map中无缓存
                cache.put(name,score);
            }
        }
        return score == null ? -1 : score.intValue();
    }

    Integer findInList(String name) {
        for (var ss : this.list) {
            if (ss.name.equals(name)) {
                return ss.score;
            }
        }
        return null;
    }
}

class Student {
    String name;
    int score;

    Student(String name, int score) {
        this.name = name;
        this.score = score;
    }
}
```

---

## Map的equals和hashCode重写

```java
package main.java.learn.date.August2023.rd3.demo3;

import java.util.Objects;

public class Light {
    private String id;
    private int price;
    private String color;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public Light(String id, int price, String color) {
        this.id = id;
        this.price = price;
        this.color = color;
    }

    //重写equals方法
    @Override
    public boolean equals(Object o) {
        if (o instanceof Light light) {
            return Objects.equals(light.color, this.color) && Objects.equals(light.id, this.id) && light.id == this.id;
        } else {
            throw new ClassCastException("类型不匹配");
        }
    }
    //重写hashcode
    @Override
    public int hashCode() {
        return Objects.hash(this.color,this.id,this.price);
    }
}

```

**Test**

```java
package main.java.learn.date.August2023.rd3.demo3;

import java.util.HashMap;
import java.util.Map;

public class Test {
    public static void main(String[] args) {
       Map<Light,Integer> map = new HashMap<>();
       map.put(new Light("1",23,"blue"),1);
        System.out.println(map.get(new Light("1", 23, "blue")));

    }
}

```

hashmap中依据key的hash值来确定value存储位置，所以一定要重写hashCode方法，而重写equals方法，是为了解决hash冲突，如果两个key的hash值相同，就会调用equals方法，比较key值是否相同，在存储时：如果equals结果相同就覆盖更新value值，如果不同就用List他们都存储起来。在取出来是：如果equals结果相同就返回当前value值，如果不同就遍历List中下一个元素。即要key与hash同时匹配才会认为是同一个key。

JDK中源码:if(e.hash == hash && ((k = e.key) == key || (key != null && key.equals(k)))){ops;}

---

上面的解释是来自评论区的一个同志的总结，我认为很不错，为什么要重写equals和hashCode，这是因为哈希冲突的存在，当我们的key值不同时，比如a，b，但是在计算哈希值的时候产生了重复，就导致了两个key都映射到了同一个value，但是实际上我们访问value的时候发现并没有冲突，这是因为这个value存储了一个集合，这个集合存储类型是一个键值对，这样子的话，哪怕冲突了，我也可以先遍历这个value存储的集合，然后查找到你本次访问我用的key，然后返回对应的value中的value

这个同志还解释了存储的时候的问题，比如我用相同的key值存储到同一个value的地方，然后检查后发现你的key是一样的，那我就在这里直接把你的新value覆盖掉旧的value，如果你传入的key不一样，那我就为你添加到集合里面

---

### EnumMap

这里还学习到了以枚举类型为key的map，暂时先放着以后用到再去体会

```java
package main.java.learn.date.August2023.rd3.demo3;

import java.time.DayOfWeek;
import java.util.EnumMap;
import java.util.Map;

public class TestEnumMap {

    public static void main(String[] args) {
        Map<DayOfWeek, String> map = new EnumMap<>(DayOfWeek.class);
        map.put(DayOfWeek.MONDAY, "星期一");
        map.put(DayOfWeek.TUESDAY, "星期二");
        map.put(DayOfWeek.WEDNESDAY, "星期三");
        map.put(DayOfWeek.THURSDAY, "星期四");
        map.put(DayOfWeek.FRIDAY, "星期五");
        map.put(DayOfWeek.SATURDAY, "星期六");
        map.put(DayOfWeek.SUNDAY, "星期日");
        System.out.println(map);
        System.out.println(map.get(DayOfWeek.MONDAY));
    }
}
```



## TreeMap

以及compare的复习  

假装已经有一个javabean类（Person）

```java
package main.java.learn.date.August2023.rd3.demo4;

import java.util.Comparator;
import java.util.Map;
import java.util.TreeMap;

public class Test {
    public static void main(String[] args) {
        Map<Person,Integer> map = new TreeMap<>(new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                if(o1.getAge() == o2.getAge()) {
                    return 0;
                }else {
                    return o1.getAge() > o2.getAge() ? -1 : 1;
                }
            }
        });
        map.put(new Person(87,"Mike"),1);
        map.put(new Person(45,"Bob"),2);
        map.put(new Person(99,"AWL"),3);

        for(Person p : map.keySet()) {
            System.out.println(map.get(p));// 3,1,2
        }
    }
}

```

这里我想复习一下compare的用法，这里用了匿名内部类，里面重写compare方法，一般来说应该是前者大于后者返回1，这样子实现一个默认的升序，这里想实现降序的话就把-1放到前面，实现一个降序，也就是高的在前

---

# Set

## HashSet&TreeSet

```java
package main.java.learn.date.August2023.rd3.demo5;

import java.util.HashSet;
import java.util.Set;
import java.util.TreeSet;

public class Test {
    public static void main(String[] args) {
        Set<String> stringSet = new TreeSet<>();
        stringSet.add("apple");
        stringSet.add("banana");
        stringSet.add("grape");
        for(String str : stringSet) {
            System.out.println(str);
        }
    }
}

```

HashSet和TreeSet其实就可以类比HashMap和TreeMap，底层就是用这两个map类实现的

---

### Solution(用set去重)

```java
package main.java.learn.date.August2023.rd3.demo5;

import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<Message> received = List.of(
                new Message(1, "Hello!"),
                new Message(2, "发工资了吗？"),
                new Message(2, "发工资了吗？"),
                new Message(3, "去哪吃饭？"),
                new Message(3, "去哪吃饭？"),
                new Message(4, "Bye")
        );
        List<Message> displayMessages = process(received);
        for (Message message : displayMessages) {
            System.out.println(message.text);
        }
    }

    static List<Message> process(List<Message> received) {
        // TODO: 按sequence去除重复消息
        Set<Message> set = new TreeSet<>(new Comparator<Message>() {
            @Override
            public int compare(Message m1, Message m2) {
                if (m1.sequence == m2.sequence) {
                    return 0;
                } else {
                    return m1.sequence > m2.sequence ? 1 : -1;
                }
            }
        });
        set.addAll(received);
        return new ArrayList<Message>(set);
    }
}

class Message {
    public final int sequence;
    public final String text;

    public Message(int sequence, String text) {
        this.sequence = sequence;
        this.text = text;
    }

}

```

---

