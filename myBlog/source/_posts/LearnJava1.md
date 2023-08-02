---
title: Java泛型
tags: [Java，泛型]
categories: [学习]
cover: https://c11njxxsblog.oss-cn-hangzhou.aliyuncs.com/202308021344646.png
swiper_index: 2 #置顶轮播图顺序，非负整数，数字越大越靠前
description: Java学习记录


---

泛型，擦拭法，extends和super通配符

<!-- more -->

#泛型

## 上界通配符（ <? extends Number> )

```java
public class PairHelper {
    public static int add(Pair<? extends Number> p) {
        Number first = p.getFirst();
        Number end = p.getEnd();
        return first.intValue() + end.intValue();
    }

    public static int addAfterSet(Pair<? extends Number> p) {
        //由于通配符的存在，p可以指定为任意的Number的子类，但我们的p只能是一个子类T比如Integer，如果说的T是Double或其他子类
        //我在处理T的set的时候，就会把Double或者其他子类set给我的Integer，这样明显是不符合规矩的

        //因为我这里可以接受Number本身及其所有子类，当我去设置的时候，我无法保证我设置的是Integer还是Double
        //因为这两个子类相当于是同级的，他们的父类都是Number，虽然都能接受并且获取（因为使用父类引用所以都能获取）
        //p.setFirst(new Integer(p.getFirst().intValue() + 100));
        //p.setEnd(new Integer(p.getEnd().intValue() + 100));
        Number first = p.getFirst();
        Number end = p.getEnd();
        return first.intValue() + end.intValue();
    }
}
```

## super通配符 (< ? super Integer>) 

```java
public class SuperPair {
    public static void set(Pair<? super Integer> p, Integer first, Integer end) {
        p.setFirst(first);
        p.setEnd(end);
    }
    public static int add(Pair<? super Integer> pair) {
        //由于我的add方法接受了Integer本身及其父类，如果说我传入了Number类，然后在下面获取了first或者end
        //那么就相当于把Number向下转成Integer，这是不合理的
        //Integer first = pair.getFirst();
        //Integer end = pair.getEnd();
        //return first + end;
        throw new ClassCastException();
    }
}
```

## 总结

extends 和 super的区分方法就是 前者他可以获取当前类（就是extends 后面的类）的本类型及其子类，这样子的话，我在方法里都用父类引用去接收，可以实现泛型的效果，也不会出现类型无法转换，但是这样就带来了无法修改的问题，由于本类和子类的类型都可以被父类引用去接收，所以导致了后续可能会出现不同类进行相互操作，这样是不合理的

再看super通配符，相当于是设定了一个下界，传入的类都有一个子类是Integer（比如），这样子的话，用可以对可传入的类型（比如Object，Number）进行修改set，即传入Integer（然后就是父类引用接收子类类型），但是却不能get，也就是只能写不能读，因为如果我要读取，然后用子类型去接收，相当于是用子类引用接收父类类型，这是不允许的。

整体上就是为了实现父类引用接收子类类型

extends可以get，因为他用父类去接收本类及子类，符合原则，但是他不能set，因为可以接收子类，但是子类之间是平级的，要避免这样的问题

super可以set，因为我接收子类本类及其父类，然后把子类的值赋给他们，符合原则，但是不能get，因为get的话，相当于用子类引用去get可能出现的父类型。