<p align="center">
  <img src="https://raw.githubusercontent.com/thrustlang/.github/main/assets/logos/new%20logo/thrustlang-logo-banner-text-italic.png" alt="Thrust Programming Language logo" style="width: 100%; max-width: 900px;">
</p>

<h1 align="center">Thrust Programming Language</h1>

<p align="center">
  A general-purpose, statically typed systems programming language for writing verbose, accurate, and fast code.
</p>

<p align="center">
  <em>Source files use the <code>.thrust</code> extension.</em>
</p>

<img src="https://raw.githubusercontent.com/thrustlang/.github/main/assets/standard-text-separator.png" alt="separator" style="width: 100%;">

## Philosophy

Thrust gives you low-level control over the machine, the same kind of control C gives you, while letting you reach for higher-level abstractions when you need them.

As the language grows, its complexity stays anchored to C. New features never ask you to carry more mental overhead than C would: if you can reason about C, you can reason about Thrust.

That balance is the whole point: you can go as deep as you need to, and you are not paying for it the rest of the time.

## Highlights

- **GPU programming**: CUDA support, with kernels compiled to PTX and launched through the CUDA Driver API.
- **C-level interoperability**: FFI declarations, System V and NVIDIA CUDA ABIs, and `importC` for C header integration.
- **Cross-compilation**: build for RISC-V, WebAssembly, macOS, Windows, Linux and more from a single machine.
- **Inline assembly**: available as an unstable feature for architecture-specific code.

## Features

### Current

- Full standalone Ahead-Of-Time (AOT) compilation.
- Full Just-In-Time (JIT) compilation via `-jit`.
- LLVM backend targeting 18 architectures.
- System V ABI and NVIDIA CUDA ABI.
- CUDA support through the NVPTX backend.
- Cross-compilation to targets such as RISC-V and WebAssembly.
- Inline assembly (`asmfn`, `asm`, `global_asm`), unstable.
- `importC` C header integration, in progress and unstable.
- Sanitizers, stack protection, and DWARF debug information.
- Editor support: VS Code, Sublime Text, and Neovim.
- Diagnostics tooling and an active fuzzing suite.

### Future

- A package manager (**torio**), working like Rust's Cargo.
- Compile-time code execution via `@compiletime`.
- Direct C code execution from the compiler.

## Examples

### Fibonacci

```rust
import std::io;

fn atoi(str: array[char]) s32 @public @arbitraryArgs @extern("atoi") @convention("C");

fn fibonacci(n: s32) s32 @public {
    if n <= 0 {
        return 0;
    }

    if n == 1 {
        return 1;
    }

    var a: s32 = 0;
    var b: s32 = 1;
    var i: s32 = 2;
    var result: s32 = 0;

    while i <= n {
        result = a + b;
        a = b;
        b = result;
        i++;
    }

    return result;
}

fn main(argc: s32, argv: ptr[array[char]]) s32 @public {
    if argc < 2 {
        io::print("Usage: ./fibonacci <n>\n");
        return 1;
    }

    var n: s32 = atoi(argv[1]);

    if n < 0 {
        io::print("Please enter a non-negative number\n");
        return 1;
    }

    var result: s32 = fibonacci(n);

    io::print("fib(%d) = %d\n", n, result);

    return 0;
}
```

### CUDA

```rust
intrinsic("llvm.nvvm.read.ptx.sreg.tid.x")   threadIdxX() u32 @public;
intrinsic("llvm.nvvm.read.ptx.sreg.ctaid.x") blockIdxX()  u32 @public;
intrinsic("llvm.nvvm.read.ptx.sreg.ntid.x")  blockDimX()  u32 @public;
intrinsic("llvm.nvvm.barrier0") syncthreads() void @public;

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

The full set of working examples lives in the [showcase folder](https://github.com/thrustlang/thrustc/blob/master/showcase): an HTTP server, an OpenGL renderer, and complete GPU kernels.

## Getting Started

The compiler is distributed as prebuilt binaries in [GitHub releases](https://github.com/thrustlang/thrustc/releases) for Linux x64, Windows x64, and macOS (x64/ARM). For a full command reference, see the [CLI documentation](https://github.com/thrustlang/thrustc/blob/master/CLI.md).

#### Linux

```console
./thrustc -opt=O3 fibonacci.thrust -cc-args="-o fibonacci" && ./fibonacci
```

#### Windows

```console
.\thrustc.exe -opt=O3 fibonacci.thrust -cc-args="-o fibonacci.exe" && .\fibonacci.exe
```

## Status

Thrust is an active side project by a small team (practically a solo developer), evolving in early **0.1.x** releases. The core pipeline (parsing, static type checking, LLVM codegen, JIT, CUDA, cross-compilation, and the release tooling) is already in place and improving. There are still edge cases to handle, so expect a few rough edges.

## Useful Repositories

- [**Main Compiler** (`thrustc`)](https://github.com/thrustlang/thrustc)
- [**Syntax**](https://github.com/thrustlang/syntax)
- [**Roadmap**](https://github.com/thrustlang/roadmap)

## Support

We're looking for contributors! Whether you're new to systems programming or an old hand, we'd love to hear from you. Already know **[Rust](https://www.rust-lang.org/)** but not **[LLVM](https://llvm.org/)** or **[GCC](https://gcc.gnu.org/)**? Don't worry, we're happy to teach you. Spanish speakers are especially welcome.

## Social Networks

[![Discord](https://invite.casperiv.dev?inviteCode=MhVpCSxnhV)](https://discord.gg/MhVpCSxnhV)

<img src="https://raw.githubusercontent.com/thrustlang/.github/main/assets/standard-text-separator.png" alt="separator" style="width: 100%;">

# Always Remember

~ *"It takes a long time to make a tool that is simple and beautiful."* ~ Bjarne Stroustrup (C++ Programming Language creator)
