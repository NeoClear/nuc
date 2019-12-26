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

## Switch

Switch statement in java is almost the same as the one in c++. However, c++ does not allow string to be switched, but java allow it
(java allow int, String and enumerate type to be switched)

```java
switch ("StarPort") {
case "INnoVation":
    System.out.println("INnoVation");
    break;
case "StarPort":
    System.out.println("StarPort");
    break;
default:
    System.out.println("Default");
}
```

## While & Do While

Just like c++

## For & For Each

Just like c++

```java
String[] players = {"INnoVation", "Maru", "ByuN", "Dark", "soO"};
for (String p : players)
    System.out.println(p);
```

## Array

Array in java is something similar to vector in c++ and list in python (not array in c)
In java you can use length property to get the length of an array (like size() method in c++ and len() function in python)

```java
int[] pl = {1, 2, 3, 4, 5};
for (int p : pl)
    System.out.println(p);

int[][] gr = {{1, 2, 3, 4, 5}, {2, 4, 6}, {4, 7}};
for (int[] line : gr) {
    for (int ele : line)
        System.out.print(ele);
    System.out.println(line.length);
}
```

## Method & Overloading

In java, you can call a method within the same class with only its name and parameter

And java supports overloading (distinguishing functions with the same name by their parameters)

```java
public class MyClass {
    static void run() {
        System.out.println("RUN");
    }

    static void run(String fname) {
        System.out.println(fname + " running");
    }

    static int add(int a, int b) {
        return a + b;
    }

    static double add(double a, double b) {
        return a + b;
    }
    public static void main(String[] args) {
        run();
        run("StarPort");

        System.out.println(add(2, 3));
        System.out.println(add(2.3d, 2.4d));
    }
}
```

## Class

In java, if a class is public, its name must match the file name (EX: public class MyClass must be contained by MyClass.java)

in java, it creates an object of a class by (which is the same as c++)

```java
MyClass myObj = new MyClass();
```

Then you can use methods and properties of this instance of MyClass (like get the value of its property)

### Compilation Process

You can actually compile java files into .class binaries. .class files are binary codes, but you need java virtual machine to interprete them

How it works?

There are two files that are in the same directory

```java
// MainClass.java
public class MainClass {
    public static void main(String[] args) {
        MyClass myObj = new MyClass();
        System.out.println(myObj.x);
    }
}
```

```java
// MyClass.java
public class MyClass {
    int x = 10;
}
```

you use the command **javac file.java** to compile each file. However, if you compile MainClass.java, it will generate two files: MainClass.class & MyClass.class. It is because MyClass is used in MainClass. In order to get MainClass work, MyClass must be compiled. But if you compile MyClass.java, it will only generate MyClass.class

After you have compiled .java files to get .class files, you can use **java MainClass** to run the program. This command looks for a file called MainClass.class then executes it

### Static & Non-Static methods

In java, function main is a public static function inside a class. Thus, you cannot call a non-static method in the same class. Just imaging, how can you modify something inside an instance if you are not in any instance?

However, a non-static function can call any static functon withing its class (NOT FAIR!!!)

## Constructor

Constructors in java is the same as c++

## Modifier

You can interpret modifiers as describers. They describes different aspects of a variable or function or even class

There are two types of modifiers:

+ Access Modifiers
+ Non-Access Modifers

### Access Modifiers

+ Classes
    - **public**: The class is accessible by any other class
    - **default**: The class is only accessible by classes in the same package
+ Attributes, methods & constructors
    - **public**: The code is accessible for all classes
    - **private**: The code is only accessible within the declared class
    - **default**: The code is only accessible in the same package
    - **protected**: The code is only accessible in the same package and subclasses

### Non-Access Modifiers

+ Classes
    - **final**: The class cannot be inherited by other classes
    - **abstract**: The class cannot be used to create objects

+ Attributes & Methods
    - **final**: Attributes & methods cannot be overridden/modified
    - **static**: Attributes & methods belongs to the class, rather than an object
    - **abstract**: Can only be used in an abstract class, and can only be used on methods. The method does not have a body
    - **transient**: Attributes & methods are skipped when serializing the object containing them
    - **synchronized**: Methods can only be accessed by one thread at a time
    - **volatile**: The value will not be cached. Always read from main memory

## Package

```java
// Import a class of a package
import java.util.HashMap;
// Import all classes of a package
import java.util.*;
```

For user defined packages, java uses a file system directory to store them. Just like folders on your computer. For example: root -> mypack -> MyPackageClass.java

**Hint: java packages are lower case (avoid conflict with classes)**

If you want to compile a package, you can follow these steps

```java
// MyPackageClass.java
package mypack;
public class MyPackageClass {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

```console
# -d specifies the location to put package
javac -d . MyPackageClass.java
```

Then you will see a folder with MyPackageClass.class in the same directory

Run it

```console
java mypack.MyPackageClass
```

## Inheritance

```java
public class MyClass {
    public static void main(String[] args) {
        Car myCar = new Car();
        myCar.honk();
        System.out.println(myCar.brand + " " + myCar.modelName);
    }
}

class Vehicle {
    protected String brand = "Ford";
    public void honk() {
        System.out.println("TuT");
    }
}

public class Car extends Vehicle {
    public String modelName = "Mustang";
}
```

It seems that my openjdk is not smart enough. It will throw an error if you place a class without main method at first

## Polymorphism

Polymorphism means use the same function of two inherited class to perform different tasks

```java
public class MyClass {
    public static void main(String[] args) {
        Animal myAnimal = new Animal();
        Animal myPig = new Pig();
        Animal myDog = new Dog();
        myAnimal.beep();
        myPig.beep();
        myDog.beep();
    }
}

public class Animal {
    public void beep() {
        System.out.println("Animal Makes Sounds");
    }
}

public class Pig extends Animal {
    public void beep() {
        System.out.println("Pig hoke");
    }
}

public class Dog extends Animal {
    public void beep() {
        System.out.println("Dog bark");
    }
}
```

## Inner Class

Very interesting topic (Although I didn't find it really useful)

Inner class is dependent on outer class. Thus, we need a instance or object to use inner class (this is weird)

```java
public class Main {
    public static void main(String[] args) {
        OuterClass myOuter = new OuterClass();
        // Remember this notation
        OuterClass.InnerClass myInner = myOuter.new InnerClass();
        System.out.println(myOuter.x);
        System.out.println(myInner.y);
    }
}

public class OuterClass {
    int x = 10;
    public class InnerClass {
        int y = 5;
    }
}
```

However, if you use static modifier to describe inner class, then you do not need an instance to use inner class

**Side Note**
We cannot declare static methods in inner class because inner class is implicitly associated with an outer class object

Inner class can actually access attributes & methods of outer classes

## Abstraction

Abstract keyword:

+ abstract class
    cannot be used to create objects
+ abstract method
    can only be used in an abstract class, does not have function body

Abstract classes can have non-abstract methods, abstract methods must inside an abstract class

```java
public class Main {
    public static void main(String[] args) {
        Pig myPig = new Pig();
        myPig.animalSound();
        myPig.sleep();
    }
}

abstract class Animal {
    public abstract void animalSound();
    public void sleep() {
        System.out.println("Zzz");
    }
}

class Pig extends Animal {
    public void animalSound() {
        System.out.println("Wee wee");
    }
}
```

## Interface

Unlike inheritance, interface use **implements** to implement. You can implement from several interfaces. You must overwrite all methods in interfaces, and you can add more methods to it

```java
public class Main {
    public static void main(String[] args) {
        Dog myDog = new Dog();
        myDog.voice();
        System.out.println(myDog.price());
    }
}

interface Animal {
    public void voice();
}

interface LiveStock {
    public int price();
}

class Dog implements Animal, LiveStock {
    public void voice() {
        System.out.println("Zzz");
    }
    void sleep() {
        System.out.println("Wee wee");
    }
    public int price() {
        return 255;
    }
}
```

## Enum

```java
public class Main {
    enum Level {
        LOW,
        MEDIUM,
        HIGH
    }
    public static void main(String[] args) {
        Level myVar = Level.HIGH;
        // Can print its string form directly
        System.out.println(myVar);
        // Loop through all values
        for (Level val : Level.values())
            System.out.println(val);
    }
}
```

## User Input

Use java.util.Scanner

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        System.out.println(input.nextInt());
        System.out.println(input.nextDouble());
        System.out.println(input.nextLine());
    }
}
```

## Date & Time

Date means year, month & day, time means hour, minute & second

```java
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;

public class Main {
    public static void main(String[] args) {
        LocalDate date = LocalDate.now();
        System.out.println(date);
        LocalTime time = LocalTime.now();
        System.out.println(time);
        LocalDateTime dateTime = LocalDateTime.now();
        System.out.println(dateTime);

        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MMM-dd HH:mm:ss");
        System.out.println(formatter.format(dateTime));
        // yyyy year
        // MM month in digit
        // MMM month in abbr
        // dd day
        // HH hour
        // mm minute
        // ss second
    }
}
```

