---
title: java notes
date: 2019-12-22 16:50:58
categories:
- programing languages
tags:
- java
---

start learning java now!!!

<!--more-->

## Skeleton

Basic Skeleton

```java
public class MyClass {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

There should be a public class with a static public function called main which receives String[] as its arguments.

## Comments

The way of commeting in java is the same as commenting in c/c++.

```java
// This is a single line of comment

/*
 * This is a multiple line of comment
 */
```

## Variables & Data Types

There are 8 primitive data types in java, along with 1 non-primitive builtin data type Stirng.

+ byte: 1 byte
+ short: 2 bytes
+ int: 4 bytes
+ long: 8 bytes
+ boolean: 1 bit
+ char: 2 bytes
+ float: 4 bytes
+ double: 8 bytes

These 8 data types are called primitive data types

+ primitive data types are defined in java, while non-primitive data types are not (except String)
+ primitive data types do not have method calls, while non-primitive data types have
+ primitive data types always have values, while non-primitive data types might be null
+ primitive data types start with lowercase letter, while non-primitive data types start with uppercase letter
+ size of primitive data types depend on its type, while all non-primitive data types have the same size

## Type Casting

+ Widening Casting (Automatically)
byte -> short -> int -> long -> float -> double

+ Narrowing Casting (Manually)
double -> float -> long -> int -> short -> byte

```java
public class MyClass {
    public static void main(String[] args) {
        // Widening Casting
        System.out.println("Widening");
        int myInt = 6;
        double myDouble = myInt;
        System.out.println(myInt);
        System.out.println(myDouble);

        // Narrowing Casting
        System.out.println("Narrowing Casting");
        double mDouble = 6.0d;
        int mInt = (int)mDouble;
        System.out.println(mDouble);
        System.out.println(mInt);
    }
}
```

## Operators

The operators of java is almost the same as c/c++ (I didn't find any difference)

Some examples (although I don't think it is necessary)

```java
public class MyClass {
    public static void main(String[] args) {
        // plus
        System.out.println(3 + 5);
        // minus
        System.out.println(3 - 5);
        // times
        System.out.println(3 * 5);
        // division
        System.out.println(3 / 5);
        System.out.println(3 / 5d);
    }
}
```

## String

String of java is very similar to string in c++ and str in python. Just basic string operations like concatination and bunch of method calls.

However, String of java is more similar to python than c++, because it supports automatic creating (Yes I created this terminology).

In java you can do this

```java
System.out.println("innovation".toUpperCase());
```

It is the same as python

```python
"innovation".strip()
```

However, in c++, you cannot do that

```c++
"innovation".substr(0, 4); // It is wrong!
```

Some example methods of String in java

```java
public class MyClass {
    public static void main(String[] args) {
        String s = "innovation";
        // method calls
        System.out.println(s.length());
        System.out.println(s.toUpperCase());
        System.out.println(s.toLowerCase());
        System.out.println(s.indexOf("innovation"));
        System.out.println("innovation".toUpperCase());
    }
}
```

## Math

There is a module in java called Math (first letter is capitalized). It has math functions.
All methods inside this module is static (you can call them without an instance)

Moreover, java uses polymorphism to handle functions with same names but different arguments. In python, an input of number whether it is a float or int, it would be treated as number. So, it would be a single function. In java however, input of double, float or other number types are different. So, although a function called Math.abs handles numbers, it has several functions dealing with different number types. This is awkward if you have used python before.

```java
// Math.sqrt returns float or double?
System.out.println(Math.sqrt(64));
System.out.println(Math.sqrt(0));
// Returns NaN
System.out.println(Math.sqrt(-1));
System.out.println(Math.abs(-43));
// 0 <= Math.random() < 1
System.out.println(Math.random());
```

## Boolean & IF Statement

In java, true & false values can be printed directly (just like python), which is why java is smarter than c++ (still not as smart as python)

```java
System.out.println(true);
System.out.println(false);
// Print true
System.out.println(10 > 9);
```

As for if statement, java is different from both c++ and python. In java, the expression in if statement must be boolean. (It is awesome, although I prefer c/c++ style)

```java
if (true) {
    System.out.println(true);
} else {
    System.out.println(false);
}
```

Moreover, if the body of the statement contains only one statement, then the curly brackets can be missed (just like c/c++)

```java
if (10 > 9)
    System.out.println(true);
else
    System.out.println(false);
```