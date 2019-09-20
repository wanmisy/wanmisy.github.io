---
title: java8实战之Lambda表达式
date: 2019-04-20 19:54:00
tags: [java8实战]
categories: java8实战
---
# Lambda表达式

## Ladmaba简介
> Lambda表达式可以理解成可传递的匿名函数的一种方式，由参数、箭头和主体组成
Lambada表达式只能配合函数式接口使用，那么什么是函数式接口呢？
* 函数式接口：只有一个抽象方法的接口，可以用@FunctionalInterface标记

  

  

  <!-- more -->
## java8设计的几个常见的函数式接口
1. Predicate
```
@FunctionalInterface
public interface Predicate<T> {

    /**
     * Evaluates this predicate on the given argument.
     *
     * @param t the input argument
     * @return {@code true} if the input argument matches the predicate,
     * otherwise {@code false}
     */
    boolean test(T t);
    ...
}
```
2. Consumer 
```
@FunctionalInterface
public interface Consumer<T> {

    /**
     * Performs this operation on the given argument.
     *
     * @param t the input argument
     */
    void accept(T t);
    ...
}
```
3. Function  
```
@FunctionalInterface
public interface Function<T, R> {

    /**
     * Applies this function to the given argument.
     *
     * @param t the function argument
     * @return the function result
     */
    R apply(T t);
    ...
}
```

#### 注意
任何函数式接口都不能抛出异常  
那么如果我们需要Lambda表达式抛出异常，怎么办呢？这里提供两种解决方案  
1. 定义及自己的函数接口
```
package io.github.jiangdequan;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.InputStreamReader;

public class ReadProcessor {
    public static void main(String[] args) throws Exception {
        File file = new File("D:\\missj\\java8_actual_combat\\Lambda表达式\\README.md");
        
        BufferedReader bufferedReader = new BufferedReader(new FileReader(file));
        FileProcessor processor = (arg) -> arg.readLine();
        String content = processor.process(bufferedReader);
        System.out.println(content);
    }
    
}

@FunctionalInterface
public interface FileProcessor{
    String process(BufferedReader bufferedReader) throws Exception;
}

```
2. 使用java8的函数式接口显示的捕捉受检异常
```
package io.github.jiangdequan;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.InputStreamReader;
import java.util.function.Function;

public class ReadProcessor {
    public static void main(String[] args) throws Exception {
        File file = new File("D:\\missj\\java8_actual_combat\\Lambda表达式\\README.md");
        
        BufferedReader bufferedReader = new BufferedReader(new FileReader(file));
        Function<BufferedReader, String> processor = (BufferedReader b) -> {
            b.readLine();
        };

        String content = processor.apply(bufferedReader);
        System.out.println(content);
    }
    
}

```
#### 实战
我们将苹果按照重量排序
```
import java.util.ArrayList;
import java.util.List;

import static java.util.Comparator.comparing;

public class Filter {

    public static void main(String[] args) {
        Apple apple = new Apple(2.3, 5.0, "green");
        Apple apple1 = new Apple(1.0, 15.0, "red");
        Apple apple2 = new Apple(2.0, 10.0, "red");
        List<Apple> list = new ArrayList<>();
        list.add(apple);
        list.add(apple1);
        list.add(apple2);

        list.sort(comparing(Apple::getColor));
        list.forEach(arg -> System.out.println(arg.toString()));
    }
    
}

```
