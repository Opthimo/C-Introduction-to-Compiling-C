<!---
{
  "depends_on": ["https://github.com/STEMgraph/718193ef-11a1-408d-af23-4b10c24d490d", "https://github.com/STEMgraph/99787eda-617a-4a68-b9a4-d60ec5c5c303"],
  "author": "Stephan Bökelmann",
  "first_used": "2025-04-02",
  "keywords": ["C Compiler", "Assembler", "Linking", "Object Files"]
}
--->

# C Compiler Basics

## 1) Introduction
> ⚠️ We do not write the code ourselves – the compiler does that for us. We just have to make sure the compiler understands us.

When we were writing assembly, a lot of thinking about data structures and the correct order of executing `system calls` was required. High-level languages such as C mitigate a subset of these problems for us.  
Let’s be clear here: when we are talking about a programming language, we are actually talking about a program that knows how to transform a certain piece of written text into executable code. This program, a part of the workflow that produces our executable code (alongside the linker), is called the **compiler**.

Historically, the compiler was a *job*. A person would read a physicist’s or mathematician’s instructions, consult the machine’s manual, and then write down the correct machine instructions so operators could carry out the calculations.

Today, the compiler is just a piece of software to which we pass the path of a text file we want translated into machine code. Over the last 80 years of computer development, a sort of canonical structure has emerged that resembles how compilers actually work.

The compiler starts by opening the file and copying the code into a data structure in RAM. The process of reading serialized text and transforming it into a program-specific data structure is called **parsing**. While doing that, the code goes through the first stage of compilation, called **tokenization**. Every word in the source code gets tagged with an attribute, such as `identifier`, `operator`, or `literal`.

In the next step, the **syntax** is checked: do the statements, which are typically separated by a delimiter (`;` in C), form valid constructs in terms of syntactical rules? For example, without further reading, the syntax analyzer can check whether two identifiers are written next to each other without an operator. This can't produce a well-formed program, so the compilation process stops with an error.

If there is no syntactical issue, the compiler proceeds to the **semantic analysis**: does this code make sense?  
Are all operators used correctly in terms of the data types they operate on? Are all variables declared before they are used?

During this process, the compiler also transforms the former linear code into a network of nodes – a graph-like structure called the **annotated syntax tree** (or AST).

This graph now contains **relationships** ([mappings](https://github.com/STEMgraph/da751de6-10d3-4770-a4d3-359f08bc6631)) between different entities. A formatter then writes this tree into a new form of program: a very specific language called **intermediate representation** (IR).  
This IR contains a lot of metadata about the code and its structure and can be interpreted by other programs similarly to how a compiler interprets C.

Based on this, mathematical transformations (function mappings) can be applied to the IR.  
Remember: the function we originally defined in our code is still the entity we’re working with.

You see, not only values can be mapped onto other values — the **structure and logic of functions** can also be transformed.  
A simple example: the function `f(x) = 3x + 5x - 10x` can be reduced to `f(x) = -2x`. Although the expression looks different, it's still **functionally equivalent**.  
Such transformations aim to reduce the number of steps needed to calculate outputs. There are many transformations that can modify the syntax tree to generate **smaller, faster, or more efficient** programs that behave identically — as evaluated by their **observable behavior**.

> 🔍 **The _as-if rule_**: The C standard explicitly states that the compiler may perform any transformations it wants, as long as the observable behavior of the program remains the same.

Applying these transformations is known as **optimization**, and it’s a field of study in its own right.

After several **optimization passes** over the IR, the compiler ends up with an optimized version of the syntax tree.

From here, **assembly** is generated, and an assembler (like GAS in the GCC toolchain) is invoked.  
The assembler now needs a **target triplet** to know which machine it’s generating code for (e.g., `x86_64-pc-linux-gnu`).

### 1.2) Historical Context
The first compilers were written in assembly and designed to translate languages like Fortran into machine code. John Backus and his team at IBM created one of the earliest optimizing compilers in the 1950s. Brian Kernighan and Dennis Ritchie’s work on Unix and C helped establish the modern compile/link model we still use today. 

> ⚠️ Fun fact: The original C compiler was itself written in C, using a technique called *bootstrapping*. This allowed for portability and platform independence at a time when everything was deeply hardware-specific.

### 1.1) Further Readings and Other Sources
- Brian W. Kernighan & Dennis M. Ritchie: *The C Programming Language*
- [Compiler Explorer (godbolt.org)](https://godbolt.org/)
- [What Does a Compiler Do? (YouTube)](https://www.youtube.com/watch?v=FnGCDLhaxKU)

## 2) Tasks
1. **Hello Compiler**: Write a simple C program that prints "Hello, World!" and compile it with `gcc -Wall -o hello hello.c`. Then, use `file`, `ls -l`, and `strings` to inspect the binary.
```C
#include<stdio.h>

int main(int argc, char** argv)
{
  printf("Hello World\n");
  
  return 0;
}
```

2. **Compiler Stages**: Use `gcc -E`, `gcc -S`, `gcc -c` and `gcc -o` to observe preprocessing, compilation, assembling, and linking.
3. **Explore Output**: Compare the output of `gcc -S` with your own hand-written assembly.
4. **Inline Assembly**: Insert a short inline assembly instruction (like `asm("nop");`) in your C file and recompile.
```C
#include <stdio.h>

int main(int argc, char** argv) {
    asm("nop");  // Inline assembler instruction

    printf("Hello World\n");

    return 0;
}
```

## 3) Questions
1. What are the four stages of compilation?
2. How is the `.s` file generated by GCC similar or different from your own `.s` file written in GAS?
3. Why is the `-Wall` flag recommended?
4. What information can be extracted using the `file` command on the executable?
5. How does the C compiler help manage hardware-specific details?

## 4) Advice
Use `gcc -v` to trace what the compiler is doing. Don’t hesitate to use `man gcc` to explore flags and options. Experimenting with `-O1`, `-O2`, `-Og`, and `-g` helps deepen your understanding of optimization and debugging symbols.

