---
title: java silver bullet
date: 2019-12-24 03:35:21
categories:
- silver bullet
tags:
- java
---

Java is worse than c++ & python
Its STL is toooooooo bad!!!
You even cannot get the default of a non-existing key in a map!!!

<!--more-->

## HashMap & HashSet

HashMap & HashSet need wrapper types, so you cannot use int, float directly (because they are primitive types)
Instead, you have to use Character (char), Integer (int, long, short, byte), Double (double), etc

```java
HashMap<Integer, Integer> map = new HashMap<>();
HashSet<Integer> visited = new HashSet<>();
```

And in java, the sorted version of map & set are called **TreeMap** & **TreeSet** (It is weird)

Java does not have operator overloading mechanism for these STLs. Thus, you have to use awkward functions to have something which is easy in c++. For example

```java
HashMap<Integer, Integer> map = new HashMap<>();
map.put(2, 4);
System.out.println(map.getOrDefault(2, 0));

// Update a value
int[] arr = {1, 2, 3, 4, 5, 6};
for (int i : arr) {
    map.put(i, map.getOrDefault(i, 0) + 1);
}
```

Some methods of HashMap & HashSet

```java
// HashMap
HashMap<Integer, Integer> map = new HashMap<>();
map.put(1, 2);
map.get(1);
map.getOrDefault(1, 0);
map.containsKey(1);
map.containsValue(2);
map.keySet();
map.values();
map.remove(1);
map.clear();
map.size();
```

```java
HashSet<Integer> visited = new HashSet<>();
visited.add(1);
visited.clear();
visited.contains(1);
visited.size();
visited.remove(1);
```