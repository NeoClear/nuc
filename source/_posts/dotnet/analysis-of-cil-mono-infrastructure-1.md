---
title: analysis of cil & mono infrastructure 1
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

### Access method arguments & control flow

We have a piece of code that accesses its arguments

```cs
class Test {
    public static void Main(string[] args) {
        foreach (string it in args)
            System.Console.WriteLine(it);
    }
}
```

```cil
.assembly ConsoleApp{}

.method public static void Main (string[] args)
{
	.entrypoint
	.locals init (
		string	V_99,
		string[]	V_1,
		int32	V_2
	)
    // load the first argument to stack top
	IL_0000:  ldarg.0 
	IL_0001:  stloc.1 
	IL_0002:  ldc.i4.0 
	IL_0003:  stloc.2

    // jump to the label
	IL_0004:  br IL_0017

	IL_0009:  ldloc.1
	IL_000a:  ldloc.2
    // load the reference of given array at given index to stack top
	IL_000b:  ldelem.ref 
	IL_000c:  stloc.0
	IL_000d:  ldloc.0
	IL_000e:  call void class [mscorlib]System.Console::WriteLine(string)
	IL_0013:  ldloc.2
	IL_0014:  ldc.i4.1
	IL_0015:  add
	IL_0016:  stloc.2
	IL_0017:  ldloc.2
	IL_0018:  ldloc.1
    // load the length of stack top array to stack top
	IL_0019:  ldlen
    // change the stack top to int32
	IL_001a:  conv.i4
    // if the first is smaller than the second, jump to target
	blt IL_0009

	ret 
} // end of method Test::Main
```

#### new instructions in this part

+ ldarg
if index is within [0, 3], just use **ldarg.0** to load arguments
if exceeds, use **ldarg.S 432** instead

+ br
jump to label

+ ldelem.ref
load the element of array given index to stack top as a reference

+ ldlen
load the length of given array to stack top

+ conv.i4
convert to int32

+ blt
jump to label if the first is less than the second

### Generate new array & control flow

I have the code:

```cs
class Test {
    public static void Main(string[] args) {
        int[] lst = {0, 1};
        for (int i = 0; i < 2; i++)
            System.Console.WriteLine(lst[i]);
    }
}
```

The corresponding cil code is

```cil
.assembly	ConsoleApp{}

.method public static void Main (string[] args) {
	.entrypoint
	.locals init (
		int32[]	V_0,
		int32	V_1
	)
	IL_0000:  ldc.i4.2
	IL_0001:  newarr [mscorlib]System.Int32
	IL_0006:  dup
	IL_0007:  ldc.i4.1
	IL_0008:  ldc.i4.1
	IL_0009:  stelem.i4
	IL_000a:  stloc.0
	IL_000b:  ldc.i4.0
	IL_000c:  stloc.1 
	IL_000d:  br IL_001e

	IL_0012:  ldloc.0 
	IL_0013:  ldloc.1 
	IL_0014:  ldelem.i4 
	IL_0015:  call void class [mscorlib]System.Console::WriteLine(int32)
	IL_001a:  ldloc.1 
	IL_001b:  ldc.i4.1 
	IL_001c:  add 
	IL_001d:  stloc.1 
	IL_001e:  ldloc.1 
	IL_001f:  ldc.i4.2 
	IL_0020:  blt IL_0012

	IL_0025:  ret 
} // end of method Test::Main
```

#### new instruction of this example

+ newarr
create a new array and pushes its addr to stack top given array size

+ dup
copy the stack top

### customized cil codes

This is a code that generates a int[] = {0, 3, 1} and print arr.getIndex(i) times "Hello World"

```cil
.method public static void main(string[] args) {
    .entrypoint
    .locals init (
        string  V_0,
        int32[] V_1,
        int32   V_2
    )
    ldstr   "hello world!"
    stloc.0
    ldc.i4.0
    stloc.2

    ldc.i4.3
    newarr [mscorlib]System.Int32
    stloc.1
    ldloc.1
    ldc.i4.1
    ldc.i4.3
    stelem.i4
    ldloc.1
    ldc.i4.2
    ldc.i4.1
    stelem.i4
    IL_0000:    br IL_0002

    IL_0001:    ldloc.0
    ldloc.1
    ldloc.2
    ldelem.i4
    call void print(string, int32)
    ldc.i4.0
    call void [mscorlib]System.Console::WriteLine(int32)

    ldloc.2
    ldc.i4.1
    add
    stloc.2

    IL_0002:    ldloc.2
    ldc.i4.3
    blt IL_0001

    ret
}
```

This piece of code shows how arrays are stored in the actual code. Array is just a reference, and after modifying its elements, you do not need to store its value back to local variable table
