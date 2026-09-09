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

<p align="center">
  <a href="https://github.com/thrustlang/thrustc">Compiler</a> |
  <a href="https://github.com/thrustlang/thrustc/releases">Downloads</a> |
  <a href="https://github.com/thrustlang/website">Website Source</a> |
  <a href="https://github.com/thrustlang/roadmap">Roadmap</a>
</p>

<img src="https://raw.githubusercontent.com/thrustlang/.github/main/assets/standard-text-separator.png" alt="separator" style="width: 100%;">

## Philosophy

Thrust gives low-level machine control like C, while still letting you reach for higher-level abstractions when needed.

New features are designed to keep the same mental model: if you can reason about C, you can reason about Thrust.

The language favors explicit code, predictable behavior, and readable systems programming over hidden runtime machinery.

## Language Features

- **C-style clarity**: explicit locals, direct control flow, predictable value and branch behavior.
- **Generics at compile time**: generic functions and structures with explicit instantiation and no runtime generic overhead.
- **C interoperability**: direct interop through `@extern`, `@convention("C")`, variadic declarations, and raw pointer boundaries.
- **Compile-time conditionals**: `@if`, `@elif`, and `@else` for platform-specific code selection.
- **Useful imports**: `std::` modules, aliases, selective imports, and file imports without hiding dependencies.
- **Built-in compile helpers**: `sizeOf`, `alignOf`, `typeWidth`, `pointerWidth`, `isSameType`, `staticAssert`, and compiler metadata helpers.
- **Cross-platform targets**: build for Linux, Windows, macOS, RISC-V, WebAssembly, and other backend-supported targets.

## Compiler Capabilities

- Standalone Ahead-Of-Time (AOT) compilation.
- Just-In-Time (JIT) compilation via `-jit`.
- LLVM backend with broad target support.
- System V ABI support and NVIDIA CUDA ABI work.
- Sanitizers, stack protection, and DWARF debug information.
- Diagnostics tooling and fuzzing coverage.
- Editor support for VS Code, Sublime Text, and Neovim.

## Experimental And Advanced Areas

- **CUDA / NVPTX**: GPU kernels compiled to PTX and launched through CUDA tooling.
- **Inline assembly**: `asmfn`, `asm`, and `global_asm` for architecture-specific code.
- **C header integration**: `importC` work for importing C declarations.

These areas are useful for low-level and platform-specific work, but they are expected to evolve as the language matures.

## Example

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
    io::print("fib(%d) = %d\n", 10, result);
    return 0;
}
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

Thrust is evolving in early `0.2.x` releases. The core pipeline is already in place: parsing, static type checking, LLVM code generation, JIT execution, cross-compilation, standard library snapshots, and release tooling.

The language and standard library are still young. Expect rough edges, missing pieces, and breaking changes while the project stabilizes.

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
