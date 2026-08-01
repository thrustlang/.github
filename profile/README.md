<p align="center">
  <img src= "https://github.com/thrustlang/.github/blob/main/assets/logos/new%20logo/thrustlang-logo-banner-text-italic.png" alt= "logo" style= "width: 1hv; height: 1hv;"> </img>
</p>

<h1 align="center">Thrust Programming Language</h1>

<p align="center">The <b>Thrust Programming Language</b>. A general-purpose, statically typed systems programming language for writing verbose, accurate, and fast code.</p>

<img src= "https://github.com/thrustlang/.github/blob/main/assets/standard-text-separator.png" alt= "standard-separator" style= "width: 1hv;"> </img>

## Thrust Programming Language

Thrust is a system programming language. It offers developers more precise control over system programming, similar to native C and C-like languages.

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
- System-V ABI compliant.

## Future Features

- Automatically generated types for C headers (**CBindgen**) through the Clang frontend C & C++ compiler.
  
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

### Hello World

```rust
// ******************************************************************************************
//
//                                      Hello World!
//
// ******************************************************************************************

// Thrust Programming Language - File extensions
// 
// - '.🐦'
// - '.thrust'
//

// External declaration for the C printf function.
fn print(fmt: const array[char]) s32 @public @arbitraryArgs @extern("printf") @convention("C");

fn main() s32 @public {

    print("Hello World!");         
    return 0;

}
```

## Case Of Study

The **Thrust Programming Language** is involved with a research project or is part of a census at the University of Porto (**U.Porto**) and the Institute for Systems and Computer Engineering, Technology and Science (**INESC TEC**), related to a PhD Research student.

<p align="left">
  <img src= "https://github.com/thrustlang/.github/blob/main/assets/research-universities.png" style= "width: 1hv; height: 1hv;"> </img>
</p>

DISCLAIMER: *THIS DOES NOT MEAN THAT Thrust PROGRAMMING LANGUAGE IS OFFICIALLY AFFILIATED WITH THESE INSTITUTIONS.*

## Background

The responsible team (practically a *solo developer*) considers it a side, not main, project. We focus on improving both ourselves and our side projects in parallel.

## Support

We're looking for contributors for our project! If you're a Spanish speaker and would like to contribute, contact us through our official social media channels.
Already know **[Rust](https://www.rust-lang.org/)** but not **[LLVM](https://llvm.org/)** or **[GCC](https://gcc.gnu.org/)**? Don't worry! We're happy to teach you.

<img src= "https://github.com/thrustlang/.github/blob/main/assets/standard-text-separator.png" alt= "standard-separator" style= "width: 1hv;"> </img>

## Social Networks

[![Thrust Programming Language](https://invite.casperiv.dev?inviteCode=MhVpCSxnhV)](https://discord.gg/MhVpCSxnhV)

# Always Remember

~ *"It takes a long time to make a tool that is simple and beautiful."* ~ Bjarne Stroustrup (C++ Programming Language creator)





































