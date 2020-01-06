---
title: analysis of cil & mono infrastructure
date: 2020-01-05 21:54:42
categories:
- common intermediate language
tags:
- common intermediate language
- mono
- dotnet core
- intermediate representation
---

let's start our microsoft common intermediate language journey

## Why common intermediate language on dotnet virtual machine
+ compared to llvm, better tool chain (driven by microsoft)
+ compared to jvm, better documentation (driven by microsoft)
+ easy to understand
+ cross platform
+ microsoft means easy to use & good quality (yes I'm ms dog)

<!--more-->

## why mono
+ mono is an implementation of dotnet core & it is sponsored by microsoft. It is a better choice if you want to deploy your project to other platforms
+ implement all tools in dotnet core
+ install through choco in windows, and package manager in other platforms

## commands
+ ilasm: compile cil source code into executable program
+ mono: run executable code under all platforms
+ monodis: disassemble executable program into corresponding cil, alternative of **ildasm** (use --output=file to specify output file)
+ mcs: compile csharp code into executable code

## basic structure of cil & its runtime

like jvm bytecode assembly, microsoft dotnet cil is based on stack. All operations are carried out through stack operations

## translations

### Hello World

```cs
class Test {
    public static void Main(string[] args) {
        System.Console.WriteLine("Hello World!");
    }
}
```

```cil
.assembly ConsoleApp{}

.method public static void Main()  
{  
    .entrypoint
    ldstr "hello world!"
    call void [mscorlib]System.Console::WriteLine(string)
    ret
}
```

.entrypoint denotes the entry of the whole program. In order to get the program work, .assembly ConsoleApp{} must be included at the front of the cil source file

ldstr pushes the reference of static string to stack top. Then a function in standard lib is called given stack top as its argument. Finally, ret returns the function

If the parameter of System.Console.WriteLine is changed to int32, then the code would be

```cil
ldc.i4 2333
call void [mscorlib]System.Console::WriteLine(int32)
```

### Local Variables

```cs
class Test {
    public static void Main(string[] args) {
        int a = 2;
        int b = 3;
        int c = a + b;
        System.Console.WriteLine(c);
    }
}
```

```cil
.assembly ConsoleApp{}

.method public static void Main()  
{  
    .entrypoint
    .locals init (
        int32   V_0,
        int32   V_1,
        int32   V_2
    )
    ldc.i4.2
    stloc.0
    ldc.i4.3
    stloc.1
    ldloc.0
    ldloc.1
    add
    stloc.2
    ldloc.2
    call void [mscorlib]System.Console::WriteLine(int32)
    ret
}  
```

The new thing here is local variable. If you have read csapp, you will know that x86 assembly language actually stores local variables in registers or at the bottom of current stack frame. In cil, local variables are stored in a table. It is simpler, but who knows how microsoft staffs implement this functionality

.locals init creates local variable table, and stloc.i stores stack top to local variable indexed i, ldloc.i load ith local variable to stack top

### Primitive Types & Their Representation in local variable

When specifying type in cil, different notation is used

+ int -> int32
+ string -> string
+ float -> float32
+ char -> char
+ bool -> bool

#### load a const int32 to stack top

if the load value is within [0, 8] or is -1, a simpler & faster instruction can be used

```cil
ldc.i4.0
ldc.i4.8
ldc.i4.M1
```

If the value exceedes it, you have to use short format
```cil
ldc.i4.s 532235
```

#### Load local variable

If you want to load local variable to stack top and within [0, 3], then you can just use

```cil
ldloc.0
ldloc.3
```

If the index exceedes 3, use short format instead

```cil
ldloc.s 42
```
