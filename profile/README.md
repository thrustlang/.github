<p align="center">
  <img src= "https://github.com/thrustlang/.github/blob/main/assets/logos/thrustlang-logo-name.png" alt= "logo" style= "width: 1hv; height: 1hv;"> </img>
</p>

<h1 align="center">Thrust Programming Language</h1>

<p align="center">The <b>Thrust Programming Language</b>. A general-purpose, statically typed systems programming language for writing verbose, accurate, and fast code.</p>

<img src= "https://github.com/thrustlang/.github/blob/main/assets/standard-text-separator.png" alt= "standard-separator" style= "width: 1hv;"> </img>

## Why Thrust?

Thrust is a very promising tool for bare-metal and embedded systems development thanks to its innovative low-level instruction concepts. These give developers more granular control over systems programming. Unlike traditional languages that hide the underlying abstractions of C, Thrust provides a syntax and set of features that map instruction-by-instruction. This enables total manual or automatic control over the compiler.  

**Thrust is essentially C, but at an even lower level.**

However, Thrust also allows high-level abstractions when needed, providing a perfect balance between low-level control and modern productivity.

For example, Thrust enables embedding a linear assembler within the compilation process, offering direct control over architecture-specific code generation.

```rust

asmfn invoke_x86_64_exit_syscall() void
@asmSyntax("AT&T") @convention("C")
{
    "mov $$60, %rax",
    "mov $$1, %rdi",
    "syscall"
} { 
    "~{rax}~{rdi}"
}

fn main() s32 @public {
    invoke_x86_64_exit_syscall();
    return 0;
}
```

**Exotic support**

Thrust Programming Language supports unique architectures, including those for GPUs, such as Nvidia CUDA or AMD GPUs.

```rust
intrinsic("llvm.nvvm.read.ptx.sreg.tid.x")   threadIdxX() u32 @public;
intrinsic("llvm.nvvm.read.ptx.sreg.ctaid.x") blockIdxX()  u32 @public;
intrinsic("llvm.nvvm.read.ptx.sreg.ntid.x")  blockDimX()  u32 @public;
intrinsic("llvm.nvvm.barrier0") syncthreads() void @public;

static mut sdata: array[f32; 256, 3] @linkage("internal") @public;

fn vecAdd(
    a: ptr[f32, 1],
    b: ptr[f32, 1],
    c: ptr[f32, 1],
    n: s32
) void @cuda @public {
    var tid: u32 = blockIdxX() * blockDimX() + threadIdxX();

    if (tid as s32 < n) {
        c[tid] = (deref a[tid]) + (deref b[tid]);
    }
}
```

If you would like to see more detailed real-world examples of how to use the programming language, you can check out the showcase folder on thrustc.

[Thrust Programming Language - Showcase folder](https://github.com/thrustlang/thrustc/blob/master/showcase)

<img src= "https://github.com/thrustlang/.github/blob/main/assets/standard-text-separator.png" alt= "standard-separator" style= "width: 1hv;"> </img>

## Current Features

- Full standalone Ahead Of Time (AOT) compilation.
- Full Just In Time (JIT) compilation via `-jit`.
- Control over deeper C compiler code optimizations.
- Deeper code generation control.
- Robust static type checking.
- Standalone x86_64 assembler interoperability.
- System-V ABI compliant

## Future Features

- Automatically generated types for C headers (**CBindgen**) through the Clang frontend C & C++ compiler.
- Quantum code generation, through QIR.
- Support for quantum behavior emulation with embedded QCOR, or a bytecode runner.
  
## Examples

### Compiler

#### Linux

```console
./thrustc -opt=O3 fibonacci.thrust -start -o fibonacci -end && ./fibonacci
```

#### Windows

```console
.\thrustc.exe -opt=O3 fibonacci.thrust -start -o fibonacci.exe -end && .\fibonacci.exe
```

### Package Manager

In the future, there will be a package manager that works exactly like Rust **Cargo**. Once it is installed in the system path at the root of the project, wherever the **Project.toml** file is located, it will automate the program's build process.

```console
torio run
```

### Code Example - Hello World

```rust
// ******************************************************************************************
//
//   Hello World!
//
// ******************************************************************************************

// Thrust Programming Language - File extensions
// 
// - '.🐦'
// - '.thrust'
//

// External declaration for the C printf function
fn print(fmt: const array[char]) s32 @public @arbitraryArgs @extern("printf") @convention("C");

fn main() s32 @public {

    print("Hello World!");         
    return 0;

}
```

### Code Example - Fibonacci

```rust
// ******************************************************************************************
//
//   Fibonacci - O(2^n)
//
// ******************************************************************************************

// Thrust Programming Language - File extensions
// 
// - '.🐦'
// - '.thrust'
//

// External declaration for the C printf function
fn print(fmt: const array[char]) s32 @public @arbitraryArgs @extern("printf") @convention("C");

// Computes the nth Fibonacci number recursively
//
// Parameters:
//   n: The index of the Fibonacci number to compute (unsigned 32-bit integer)
//
// Returns: The nth Fibonacci number (unsigned 32-bit integer)
//
// Attributes:
//   @hot: Marks the function as frequently executed, encouraging aggressive optimizations. 
//   @inline:
//         Maps to LLVM's 'inlinehint', suggesting the compiler inline this function
//         at call sites to reduce call overhead. May increase code size, and may be
//         limited for deep recursion.
//   @nounwind:
//         Maps to LLVM's 'nounwind', guaranteeing the function will not unwind the stack
//         (i.e., it will not throw an exception or cause an abnormal termination
//         that requires stack unwinding). This enables significant optimizations.
//
fn fibonacci(n: u32) u32
@hot
@inline
@nounwind
{
    if n <= 1 {
        return n;
    }

    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Prints the first n Fibonacci numbers
// Parameters:
//   n: The number of Fibonacci numbers to print (unsigned 32-bit integer)
fn printFibonacci(n: u32) void {
    for var i: u32 = 0; i < n; ++i; {
        print("%d\n", fibonacci(i));
    }
}

fn main(argc: u32, argv: ptr[array[char]]) s32 @public {

    print("Fibonacci sequence: ");         
    printFibonacci(25);            

    return 0;
}
```

### Code Example - 100 Millions Array Updates

```rust
// ******************************************************************************************
//
//   100 MILLIONS ARRAY UPDATES 
//
// ******************************************************************************************

fn atoi(str: const array[char]) s32 @public @vaArgs @extern("atoi") @convention("C");
fn srand(seed: u32) void @public @extern("srand") @convention("C");
fn time(timer: ptr) u32 @public @extern("time") @convention("C"); 
fn rand() s32 @public @extern("rand") @convention("C");
fn print(fmt: const array[char]) s32 @public @arbitraryArgs @extern("printf") @convention("C");

fn main(argc: s32, argv: ptr[array[char]]) s32 @public {

    var u: s32 = atoi(
        (deref (argv[1] as ptr[ptr[char]])) as const array[char]
    ); 

    srand(time(nullptr)); 
  
    var r: s32 = rand() % 10000; 
    var a: array[s32; 10000]; 

    for var i: s32 = 0; i < 10000; i++; {
        for var j: s32 = 0; j < 100000; j++; {
            a[i] = (deref a[i]) + ((j % u));             
        }

        a[i] = (deref a[i]) + r;
    }

    print("%ld\n", deref a[r]); 

    return 0;

}
```

```C
#include "stdio.h"
#include "stdlib.h"
#include "stdint.h"
#include "time.h"

int main(int argc, char** argv) {

    int u = atoi(argv[1]);             
 
    srand(time(NULL));                 
 
    int r = rand() % 10000;              
 
    int32_t a[10000] = {0};            
 
    for (int i = 0; i < 10000; i++) {    
        for (int j = 0; j < 100000; j++) { 
          a[i] = a[i] + j%u;              
        }
    
        a[i] += r;                        
    }
 
    printf("%d\n", a[r]);              

}
```

### Benchmark

- Thrust: AVG `1.76s`
- C: AVG `1.79s`

> [!NOTE]  
>  Actually, it can be a margin of error and is the same as C, although with `-jit` it even outperforms C, even though the Just-In-Time Compiler always has a heavy overhead at startup.

Commands:

#### Thrust

```console
thrustc -opt=O3 loop.thrust -start -o loop -end && ./loop 1
```

#### C

```console
clang -O3 loop.c -o loop && ./loop 1
```

> [!NOTE]  
>  Obviously, if you have a little knowledge of CS, you know that this isn't the ideal way to test which programming language is faster, but anyway, it's just to point out that Thrust is trying to be a C equivalent in the speed field.

## Case Of Study

The **Thrust Programming Language** is involved with a research project or is part of a census at the University of Porto (**U.Porto**) and the Institute for Systems and Computer Engineering, Technology and Science (**INESC TEC**), related to a PhD Research student.

<p align="left">
  <img src= "https://github.com/thrustlang/.github/blob/main/assets/research-universities.png" style= "width: 1hv; height: 1hv;"> </img>
</p>

DISCLAIMER: *THIS DOES NOT MEAN THAT Thrust PROGRAMMING LANGUAGE IS OFFICIALLY AFFILIATED WITH THESE INSTITUTIONS.*

## Background

In fact, the programming language was originally intended for learning purposes in the compiler fields, for the "team" behind the project. However, this doesn't mean it will be __taken as seriously as possible__.

The responsible team (practically a *solo developer*) considers it a side, not main, project. We focus on improving both ourselves and our side projects in parallel.

## Support

We're looking for contributors for our project! If you're a Spanish speaker and would like to contribute, contact us through our official social media channels.
Already know **[Rust](https://www.rust-lang.org/)** but not **[LLVM](https://llvm.org/)** or **[GCC](https://gcc.gnu.org/)**? Don't worry! We're happy to teach you.

<img src= "https://github.com/thrustlang/.github/blob/main/assets/standard-text-separator.png" alt= "standard-separator" style= "width: 1hv;"> </img>

## Social Networks

[![Thrust Programming Language](https://invite.casperiv.dev?inviteCode=MhVpCSxnhV)](https://discord.gg/MhVpCSxnhV)

# Always Remember

~ *"It takes a long time to make a tool that is simple and beautiful."* ~ Bjarne Stroustrup (C++ Programming Language creator)





































