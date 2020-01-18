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