---
title: java8之行为参数化传递
date: 2019-04-20 17:44:37
tags: [java8实战]
categories: java8实战
---

## 行为参数化传递代码
行为参数化是一种处理频繁变更需求的软件开发模式

例如现在有一个需求，我们需要筛选出所有的绿色的苹果，我们可以这么去操作
[源码](https://gitee.com/missj/java8_actual_combat/tree/master/通过行为参数化传递代码/chapter2_1)
```
    public static List<Apple> filterGreenApple(List<Apple> inventory){
        List<Apple> result = new ArrayList<>();
        for (Apple var : inventory) {
            if("green".equals(var.getColor())){
                result.add(var);
            }
        }
        return result;
    }
```
<!-- more -->
可是需求突然变更了，我们需要筛选出所有红色的苹果，当然，我们依然可以这么操作，将上述代码中的green改成red，这样也可以实现，但我们会发现存在大量重复代码，这时候我们发现先可以将颜色抽离出来作为一个参数
[源码](https://gitee.com/missj/java8_actual_combat/tree/master/通过行为参数化传递代码/chapter2_2)
```
public static List<Apple> filterAppleByColor(List<Apple> inventory, String color){
    List<Apple> result = new ArrayList<>();
    for (Apple var : inventory) {
        if(color.equals(var.getColor())){
            result.add(var);
        }
    }
    return result;
}
```
需求接着变更了，需要筛选重量，或者筛选颜色的时候需要筛选重量，你可能会想我们可以加一个判断变量
[源码](https://gitee.com/missj/java8_actual_combat/tree/master/通过行为参数化传递代码/chapter2_3)
```
public static List<Apple> filter(List<Apple> invetory, double weight, String color, boolean flag){
    List<Apple> result = new ArrayList<>();
    if(flag){
        for (Apple var : invetory) {
            if(var.getWeight() > weight){
                result.add(var);
            }
        }
    } else {
        for (Apple var : invetory) {
            if(color.equals(var.getColor())){
                result.add(var);
            }
        }
    }
    return result;
}
```
上面的三种情况，我们都是通过添加更多参数来满足不断变化的需求，虽然也实现了功能，但是对于一个优秀的程序员来说，这种做法是非常差的，原因在于：1、代码冗余 2、扩展性差，不易维护 3、可读性差。
接下来我们来使用更高层次的抽象，行为参数化，我我们可以仿照策略设计模式编写一套标准，对不同的筛选情况定义算法
[源码](https://gitee.com/missj/java8_actual_combat/tree/master/通过行为参数化传递代码/chapter2_4)
```
public interface ApplePredicate{
    public boolean test(Apple apple);
}

public class AppleGreenColorPredicate implements ApplePredicate{
    public boolean test(Apple apple){
        if("green".equals(apple)){
            return true;
        }
        return false;
    }
}

public class AppleHeavyPredicate implements ApplePredicate{
    public boolean test(Apple apple){
        if(apple.getWeight() > 2.0){
            return true;
        }
        return false;
    }
}
```
使用时调用对应的实现
```
public static List<Apple> filter(List<Apple> invetory, ApplePredicate applePredicate){
    List<Apple> result = new ArrayList<>();
    ApplePredicate greenApplePre = new AppleGreenColorPredicate();
    for (Apple var : invetory) {
        if(greenApplePre.test(apple)){
            result.add(var);
        }
    }
}
```
这个时候我们已将将行为参数化了，代码的可读性和可扩展性都得到了很大的提升，但是依然不够，当我们的需求不断变更的时候，就需要去写大量的实现类去扩展我们的算法，而这些算法很可能只使用一次，就不再使用了，接下来，我们考虑一下优化，使用匿名类
[源码](https://gitee.com/missj/java8_actual_combat/tree/master/通过行为参数化传递代码/chapter2_5)
```
package io.github.jiangdequan;

import java.util.ArrayList;
import java.util.List;

public class Filter  {
    public static List<Apple> filter(List<Apple> invetory, ApplePredicate applePredicate){
        List<Apple> result = new ArrayList<>();
        for (Apple var : invetory) {
            if(applePredicate.test(apple)){
                result.add(var);
            }
        }
    }


    public static void main(String[] args) {
        Apple apple = new Apple(2.3, 5.0, "green");
        Apple apple1 = new Apple(1.0, 15.0, "red");
        Apple apple2 = new Apple(2.0, 10.0, "red");
        List<Apple> list = new ArrayList<>();
        list.add(apple);
        list.add(apple1);
        list.add(apple2);

        // 筛选出所有绿色的苹果
        List<apple> result = Filter.filter(list, new ApplePredicate(){
            public boolean test(Apple apple){
                if("red".equals(apple.getColor())){
                    return true;
                }
                return false;
            }
        });

        result.forEach(arg -> System.out.println(apple.toString()));
    }
}

public interface ApplePredicate{
    public boolean test(Apple apple);
}
```
使用匿名类精简了很多，但依然显得笨重，接下来我们尝试使用Lambda表达式
[源码](https://gitee.com/missj/java8_actual_combat/tree/master/通过行为参数化传递代码/chapter2_6)
```
package io.github.jiangdequan;

import java.util.ArrayList;
import java.util.List;

public class Filter  {
    public static List<Apple> filter(List<Apple> invetory, ApplePredicate applePredicate){
        List<Apple> result = new ArrayList<>();
        for (Apple var : invetory) {
            if(applePredicate.test(apple)){
                result.add(var);
            }
        }
    }


    public static void main(String[] args) {
        Apple apple = new Apple(2.3, 5.0, "green");
        Apple apple1 = new Apple(1.0, 15.0, "red");
        Apple apple2 = new Apple(2.0, 10.0, "red");
        List<Apple> list = new ArrayList<>();
        list.add(apple);
        list.add(apple1);
        list.add(apple2);

        // 筛选出所有绿色的苹果
        List<apple> result = Filter.filter(list, (Apple apple) -> {
            if("green".equals(apple.getColor())){
                return true;
            }
            return false;
        }
        );

        result.forEach(arg -> System.out.println(apple.toString()));
    }
}

public interface ApplePredicate{
    public boolean test(Apple apple);
}
```
现在我们已经很好的完成了需求，可是突然要求变了，我们的产品不再是Apple，而是其他怎么办，我们可以用泛型对他再进行优化
[源码](https://gitee.com/missj/java8_actual_combat/tree/master/通过行为参数化传递代码/chapter2_7)
```
package io.github.jiangdequan;

import java.util.ArrayList;
import java.util.List;

public class Filter  {
    public static T List<T> filter(List<T> invetory, Predicate<T> predicate){
        List<T> result = new ArrayList<>();
        for (T var : invetory) {
            if(predicate.test(apple)){
                result.add(var);
            }
        }
    }


    public static void main(String[] args) {
        Apple apple = new Apple(2.3, 5.0, "green");
        Apple apple1 = new Apple(1.0, 15.0, "red");
        Apple apple2 = new Apple(2.0, 10.0, "red");
        List<Apple> list = new ArrayList<>();
        list.add(apple);
        list.add(apple1);
        list.add(apple2);

        // 筛选出所有绿色的苹果
        List<apple> result = Filter.filter(list, (Apple apple) -> {
            if("green".equals(apple.getColor())){
                return true;
            }
            return false;
        }
        );

        result.forEach(arg -> System.out.println(apple.toString()));
    }
}

public interface Predicate<T>{
    public boolean test(T t);
}
```
