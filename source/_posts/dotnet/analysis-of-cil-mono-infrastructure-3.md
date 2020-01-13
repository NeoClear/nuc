---
title: analysis of cil mono infrastructure 3
date: 2020-01-10 18:03:27
categories:
- common intermediate language
tags:
- common intermediate language
- mono
- dotnet core
- intermediate representation
---

c# is a c-like high level programing language designed for object oriented programing. Thus, some low level features of c is omitted
Garbage collector inside its runtime automatically moved objects for better memory organization. Thus, the address of its objects might change. However, c# has a special library that can allocate unmanaged memories. These memories will not be moved by GC

<!--more-->

### Malloc & Free

There is a builtin function that alloc the size

```cs
static void* malloc(int size) {
    return (void*)System.Runtime.InteropServices.Marshal.AllocHGlobal(size);
}
```

The corresponding cil code is

```cil
.method private hidebysig static void* malloc(
    int32 size
) cil managed {
    IL_0000: ldarg.0      // size
    IL_0001: call         native int [mscorlib]System.Runtime.InteropServices.Marshal::AllocHGlobal(int32)
    IL_0006: call         void* [mscorlib]System.IntPtr::op_Explicit(native int)
    IL_000b: ret
}
```

This is why c# is not a pure language. Its target is to have dynamic memories managed by garbage collector, but it still have user managed memories. But it is a good news for c compiler designers (This is what java lacks)

### Array Initialization

Array initialization uses special syntaxes in cil
In c#, it is

```cs
int[] lst = {1, 2, 3, 4, 5};
```

```cil
.class private auto ansi beforefieldinit Test extends [mscorlib]System.Object {
    .method public static hidebysig default void Main (string[] args)  cil managed  {
        IL_0000:  ldc.i4.5 
        IL_0001:  newarr [mscorlib]System.Int32
        IL_0006:  dup 
        IL_0007:  ldtoken field valuetype '<data>'/'$val_1' '<data>'::'$field_1'
        IL_000c:  call void class [mscorlib]System.Runtime.CompilerServices.RuntimeHelpers::InitializeArray(class [mscorlib]System.Array, valuetype [mscorlib]System.RuntimeFieldHandle)
        IL_0012:  ret 
    }
}

.class private auto ansi abstract sealed beforefieldinit '<data>' extends [mscorlib]System.Object {
    .field  assembly static initonly  valuetype '<data>'/'$val_1' '$field_1' at D_00004000
    .class nested private sequential ansi sealed beforefieldinit '$val_1' extends [mscorlib]System.ValueType {
        .pack 1
        .size 20
    }
}
.data D_00004000 = bytearray (
    01 00 00 00 02 00 00 00 03 00 00 00 04 00 00 00
    05 00 00 00
)
```

Very wired, but mechanic. You can see a very clear pattern here

What if the initialized values are float or string?
It still use the binary form of float to be initialized. There is no difference

But when it is the initialization of the array is about string, compiler will use a piece of static value to initialize it. It just use the stupidest way

### Multi dementional array

The default multi dementional array of c# is a nested reference of objects. This is not what we want
Two types of bultin arrays do not satisfy our requirement. Thus, we need to do it on our own. We can simulate multidimentional 
arrays into juse one dimensional array. Just through simple calculation

### Get the address of multidimensional array
Since in builtin array, address of different arrays are different. Thus, it is suitable for the simulation

```cil
IL_001a: ldloc.0      // numArray
IL_001b: ldc.i4.1
IL_001c: ldelema      [mscorlib]System.Int32
IL_0021: stloc.2      // V_2
```

### String
String in c# is read only as in python
Thus, we have to use char arrays here (which is what c does)

```cil
ldc.i4.0
IL_0029: ldelema      [mscorlib]System.Char
IL_002e: stloc.1      // V_1
```

