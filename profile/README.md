<p align="center">
  <img src="https://raw.githubusercontent.com/thrustlang/.github/main/assets/logos/new%20logo/thrustlang-logo-banner-text-italic.png" alt="Thrust Programming Language logo" style="width: 100%; max-width: 900px;">
</p>

<h1 align="center">Thrust Programming Language</h1>

<p align="center">
  A general-purpose, statically typed systems programming language for writing verbose, accurate, and fast code.
</p>

<p align="center">
  <a href="https://github.com/thrustlang/thrustc">Compiler</a> |
  <a href="https://github.com/thrustlang/thrustc/releases">Downloads</a> |
  <a href="https://github.com/thrustlang/website">Website Source</a> |
  <a href="https://github.com/thrustlang/roadmap">Roadmap</a>
</p>

<img src="https://raw.githubusercontent.com/thrustlang/.github/main/assets/standard-text-separator.png" alt="separator" style="width: 100%;">

## Philosophy

Thrust exposes memory operations, data layout, explicit memory alignment, calling conventions, and target information in the source code while providing high-level zero cost abstractions with simplicity.

Zero cost abstractions are resolved during compilation. Generics are specialized, modules declare imported files and symbols, and compile time conditionals remove inactive code before type checking and code generation. A scope based ownership mechanism can automatically deallocate memory without runtime ownership tracking.

Thrust keeps memory management direct and simple. The programmer controls allocation, pointer validity, resource lifetime, and the exact point at which memory is released, as in C.

## Experimental And Advanced Areas

- **CUDA / NVPTX**: GPU kernels compiled to PTX and launched through CUDA tooling.
- **Inline assembly**: `asmfn`, `asm`, and `global_asm` for architecture-specific code.
- **WebAssembly**: `wasm32` code generation and ABI support without an integrated WASI or browser runtime.
- **C header integration**: `importC` is reserved for work on importing C declarations and is not yet a complete integration.
- **Language server**: completion and limited source analysis are available, while diagnostics, definitions, and document symbols are still incomplete.

These areas are incomplete or depend on external platform tooling. Their interfaces and supported workflows may change as the compiler matures.

## Examples

### Standard Library And Generics

```thrust
import std::io;

fn fibonacci[T](n: T) T @public {
    if n <= 0 { return 0; }
    if n == 1 { return 1; }

    var a: T = 0;
    var b: T = 1;
    var i: T = 2;
    var result: T = 0;

    while i <= n {
        result = a + b;
        a = b;
        b = result;
        i++;
    }

    return result;
}

fn main() s32 @public {
    var result: s32 = fibonacci[s32](10);
    io::print(fmt= "fib(%d) = %d\n", 10, result);
    
    return 0;
}
```

### Generic Data Structures

```thrust
struct Buffer[T] @public {
    data: ptr[T],
    length: usize,
    capacity: usize
}

var buffer := new Buffer[u8] {
    data: nullptr,
    length: 0,
    capacity: 64
};
```

### C Interoperability

```thrust
fn puts(text: const array[char]) s32
    @public
    @extern("puts")
    @convention("C");
```

### Compile-Time Selection

```thrust
@if(isLinux()) const PLATFORM: u32 = 2;
@elif(isWindows()) const PLATFORM: u32 = 1;
@else const PLATFORM: u32 = 3;
```

## Getting Started

Prebuilt compiler binaries are available in the [`thrustc` releases](https://github.com/thrustlang/thrustc/releases) for Linux x64, Windows x64, and macOS x64/ARM64.

### Linux

```console
./thrustc fibonacci.thrust -cc-args="-o fibonacci" && ./fibonacci
```

### Windows

```console
.\thrustc.exe fibonacci.thrust -cc-args="-o fibonacci.exe" && .\fibonacci.exe
```

For compiler flags and examples, see the [`thrustc` repository](https://github.com/thrustlang/thrustc).

## Status

Thrust is evolving in early `0.2.x` releases. Its compiler has a complete frontend to LLVM, static type checking, AOT compilation, JIT execution, a versioned standard library, and target configurable code generation.

The language and standard library are still young. Target support varies by ABI, linker, system libraries.

## Useful Repositories

- [**Main Compiler** (`thrustc`)](https://github.com/thrustlang/thrustc)
- [**Syntax**](https://github.com/thrustlang/syntax)
- [**Website Source**](https://github.com/thrustlang/website)
- [**Roadmap**](https://github.com/thrustlang/roadmap)

## Support

Contributors are welcome. Whether you are new to systems programming or experienced with low-level tooling, there is room to help with language design, compiler work, documentation, examples, tests, and editor support.

Spanish speakers are especially welcome.

## Social Networks

[![Discord](https://invite.casperiv.dev?inviteCode=MhVpCSxnhV)](https://discord.gg/MhVpCSxnhV)

<img src="https://raw.githubusercontent.com/thrustlang/.github/main/assets/standard-text-separator.png" alt="separator" style="width: 100%;">

# Always Remember

~ *"It takes a long time to make a tool that is simple and beautiful."* ~ Bjarne Stroustrup (C++ Programming Language creator)
