---
title: kotlin notes
date: 2020-01-18 00:21:23
categories:
- programing languages
tags:
- kotlin
---

Try some kotlin syntaxes. (Maybe more about jetbrain compile tool chain & gradle)

<!--more-->

## Functions

Main function

```kotlin
fun main() {
    println("Hello World!")
}
```

Defining a function

```kotlin
fun func(a: Int, b: Int) {
    return a + b
}
```

Return type can be specified

```kotlin
fun func(a: Int, b: Int): Int {
    return a + b
}
```

Or could be written in this way

```kotlin
fun func(a: Int, b: Int) = a + b
```

In kotlin, you can place expression directly in statements

```kotlin
fun maximum(a: Int, b: Int) = if (a > b) a else b
```

Function return void

```kotlin
fun func(): Unit {

}
```

## String Templates

```kotlin
fun main() {
    var a = 2333
    var b = 6666
    var s: String = "$a plus $b is ${a + b}"
}
```

## Nullable Values

```kotlin
fun parseInt(s: Int): Int? {
    if (s != 0)
        return null
    return s
}

fun main() {
    var t = parseInt(1)
    if (t == null) {
        println("gg")
        return
    }
    println(t)
}
```

A variable can be assigned as nullable

## Type Casting

Please refer to official doc to get more detailed explanation

```kotlin
fun getStringLength(obj: Any): Int? {
    if (obj is String)
        return obj.length
    // obj is automatically casted to String in this branch
    return null
}
```

## For & While Loop

```kotlin
for (ele in lst)
    println(ele)

for (index in lst.indices)
    println("Element at $index is ${lst[index]}")
var i = 0
while (i < lst.size) {
    println("Element at $i is ${lst[i]}")
    i++
}
```

## When expression

```kotlin
when (obj) {
    1           -> "One"
    "Hello"     -> "Greeting"
    is Long     -> "Long"
    !is String  -> "Not String"
    else        -> "Unknown"
}
```

## Range

```kotlin
for (i in 1..10)
    println(i)
for (i in 10 downTo 1 step 2)
    println(i)
```

Does not include the last one

```kotlin
fun main() {
    for (i in 1 until 10)
        println(i)
}
```

## Apply lambda to collection class

```kotlin
fun main() {
    val fruits = listOf("banana", "apple", "kiwi", "avocado")
    fruits
        .filter { it.startsWith("a") }
        .sortedBy { it }
        .map { it.toUpperCase() }
        .forEach { println(it) }
}
```

## Boxing

Primitive types are stored as primitive types in java
Nullable values however, are boxed in its bytecode

```kotlin
fun main() {
    val a = 1000
    println(a === a)
    println(a == a)
    val b: Int? = a
    val c: Int? = a
    println(b === c)
    println(b == c)
}
```

## Array Generation

```kotlin
fun main() {
    val asc = Array(5) { i -> (i * i).toString() }
    for (ele in asc)
        println(ele)
}
```