---
title: analysis of cil mono infrastructure 2
date: 2020-01-08 00:44:02
categories:
- common intermediate language
tags:
- common intermediate language
- mono
- dotnet core
- intermediate representation
---

There are various limitations of common intermediate language & its runtime. Since c# is designed to be a high level language, it does not support explicit memory allocation. Moreover, it does not have the concept of global variables. However, we can use some object oriented features to simulate some lower level c operations

<!--more-->

### Access Field Variables

We have a piece of c# code that has a static attribute inside the class

```cs
class Test {
    public static int value;
    public static void Main(string[] args) {
        value = 65535;
        System.Console.WriteLine(value);
    }
}
```

Then the conrresponding cil code would be (not exactly the same)

```cil
.assembly ConsoleApp{}

.class Test extends [mscorlib]System.Object {
    .field static int32 value
    .method static void Main(string[] args) {
		.entrypoint
		IL_0000:  ldc.i4.s 65535
		IL_0001:  stsfld int32 Test::value
		IL_0006:  ldsfld int32 Test::value
		IL_000b:  call void [mscorlib]System.Console::WriteLine(int32)
		IL_0010:  ret 
	}
}
```

As we can see here, we have new instructions. They are:

+ stsfld
Store static field, store the value to corresponding field

+ ldsfld
Load stataic field to stack top

### Get the address of a local variable

We get the address of a local variable & convert it to long & print it

```cs
class Test {
    public static unsafe void Main(string[] args) {
        int val = 0;
        System.Console.WriteLine((long)&val);
    }
}
```

```cil
.method public static void Main (string[] args)  cil managed 
    {
	.entrypoint
	.maxstack 1
	.locals init (
		int32	V_0)
	IL_0000:  ldc.i4.0 
	IL_0001:  stloc.0 
	IL_0002:  ldloca.s 0
	IL_0004:  conv.u8 
	IL_0005:  call void class [mscorlib]System.Console::WriteLine(int64)
	IL_000a:  ret 
    } // end of method Test::Main

  } // end of class Test
```

#### New instructions

+ ldloca.s
load the address of local variable to stack top

### Get the address of attributes

```cs
class Test {
    public static int val = 0;
    public static unsafe void Main(string[] args) {
        fixed (int* addr = &val) {
            System.Console.WriteLine((long)addr);
        }
    }
}
```

```cil
.class Test extends [mscorlib]System.Object {
    .field  public static  int32 val
    .method public static hidebysig 
           default void Main (string[] args)  cil managed 
    {
        // Method begins at RVA 0x2058
    .entrypoint
    // Code size 17 (0x11)
    .maxstack 1
    .locals init (
      int32& pinned	V_0)
    IL_0000:  ldsflda int32 Test::val
    IL_0005:  stloc.0 
    IL_0006:  ldloc.0 
    IL_0007:  conv.u8 
    IL_0008:  call void class [mscorlib]System.Console::WriteLine(int64)
    IL_000d:  ldc.i4.0 
    IL_000e:  conv.u 
    IL_000f:  stloc.0 
    IL_0010:  ret 
      } // end of method Test::Main
  } // end of class Test
```

As we can see here, the local variable that represents the address of static attribute has special typing
It is **int32& pinned V_0**. For pinned pointers, GC of CLR will not move the address of variables that pinned pointers point to

### Initializing global arrays

In c#, every array is actually a reference. Thus, programers must initialize it before storing values into them

This is the cil form of initializer. You just put it there and everything down automatically

```cil
.method private static void '.cctor' () {
    .maxstack 8
    IL_0000:  ldc.i4.0 
    IL_0001:  stsfld int32 Test::val
    IL_0006:  ldc.i4.s 0x20
    IL_0008:  newarr [mscorlib]System.Int32
    IL_000d:  stsfld int32[] Test::arr
    IL_0012:  ret 
}
```

### Defining structure

Structure is a class inherited from System.ValueType
When we define a structure and use it in main function

```cs
class Test {
    public struct Book {
        public int x;
        public int y;
    }
    public static unsafe void Main(string[] args) {
        Book s;
        s.x = 5;
        s.y = 8;
        System.Console.WriteLine(s.x);
    }
}
```

```cil
.class Test extends [mscorlib]System.Object {
    .method static void Main (string[] args) {
        .entrypoint
        .locals init (
            valuetype Test/Book	V_0
        )
        IL_0000:  ldloca.s 0
        IL_0002:  ldc.i4.5 
        IL_0003:  stfld int32 Test/Book::x
        IL_0008:  ldloca.s 0
        IL_000a:  ldc.i4.8 
        IL_000b:  stfld int32 Test/Book::y
        IL_0010:  ldloca.s 0
        IL_0012:  ldfld int32 Test/Book::x
        IL_0017:  call void class [mscorlib]System.Console::WriteLine(int32)
        IL_001c:  ret 
    }
    .class Book extends [mscorlib]System.ValueType {
        .field  public  int32 x
        .field  public  int32 y
    }
}
```

As you can see, the struct actually extends from System.ValueType

#### New instructions

+ stfld
need the reference of the variable & the actual value to store the value to certain place
must give the type of that field and the class name & field name

### Global Struct Variables

```cs
class Test {
    public struct Book {
        public int x;
        public int y;
    }
    public static Book book;
    public static unsafe void Main(string[] args) {
        book.x = 5;
        book.y = 8;
        System.Console.WriteLine(book.x);
    }
}
```

```cil
.class Test extends [mscorlib]System.Object {
    .field public static valuetype Test/Book book
    .method static void main (string[] args) {
        .entrypoint
        IL_0000:  ldsflda valuetype Test/Book Test::book
        IL_0005:  ldc.i4.5 
        IL_0006:  stfld int32 Test/Book::x
        IL_000b:  ldsflda valuetype Test/Book Test::book
        IL_0010:  ldc.i4.8 
        IL_0011:  stfld int32 Test/Book::y
        IL_0016:  ldsflda valuetype Test/Book Test::book
        IL_001b:  ldfld int32 Test/Book::x
        IL_0020:  call void class [mscorlib]System.Console::WriteLine(int32)
        IL_0025:  ret 
    }
    .class nested Book extends [mscorlib]System.ValueType {
        .field  public  int32 x
        .field  public  int32 y
    }
}
```

The difference is just change ldloca.s to ldsflda
