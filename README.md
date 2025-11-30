# Kiv Compiler (kivc)

A modular, high-performance compiler for the Kiv programming language.

## Overview

This is the compiler sub-repository for **Kiv**, implementing a modern compilation pipeline with multiple intermediate representations (IRs) and LLVM code generation. The compiler is designed as a workspace of focused crates, each handling a specific phase of compilation.

## Architecture

The compiler follows a traditional multi-phase architecture:

```
Source Code → Lexer → Parser → AST → Resolver → Type Checker → HIR → MIR → LIR → LLVM → Native Code
```

### Core Crates

#### Frontend
- **`kivc_span`** - Source location tracking and file management
- **`kivc_diagnostics`** - Error reporting and diagnostic messages
- **`kivc_token`** - Token definitions
- **`kivc_lexer`** - Lexical analysis and token generation
- **`kivc_ast`** - Abstract Syntax Tree definitions
- **`kivc_parser`** - Syntax parsing and AST construction
- **`kivc_resolver`** - Name resolution and symbol table management
- **`kivc_typeck`** - Type checking and inference

#### Intermediate Representations
- **`kivc_hir`** - High-level Intermediate Representation definitions
- **`kivc_hir_lowering`** - AST to HIR conversion
- **`kivc_mir`** - Mid-level Intermediate Representation definitions
- **`kivc_mir_build`** - HIR to MIR conversion
- **`kivc_mir_opt`** - MIR optimizations (FVS, DVR, RC elimination)
- **`kivc_lir`** - Low-level Intermediate Representation definitions
- **`kivc_lir_gen`** - MIR to LIR conversion

#### Backend
- **`kivc_codegen`** - LLVM IR generation via inkwell
- **`kivc_runtime`** - Runtime library for memory management and built-in functions

#### Infrastructure
- **`kivc_driver`** - Compilation pipeline orchestration
- **`kivc_session`** - Compilation session management
- **`kivc_utils`** - Utility functions and data structures
- **`kivc_test_utils`** - Testing utilities and framework
- **`kivc`** - Command-line interface (main binary)

## Development

### Prerequisites

- Rust 1.70 or later (2024 edition)
- LLVM 21.1.6 (for code generation)
- Git

### Installing LLVM 21.1.6

#### Ubuntu/Debian
```bash
wget https://apt.llvm.org/llvm.sh
chmod +x llvm.sh
sudo ./llvm.sh 21
sudo apt-get install -y libpolly-21-dev
```

#### macOS
```bash
brew install llvm@21
export LLVM_SYS_211_PREFIX=$(brew --prefix llvm@21)
```

#### Windows
Download and extract the prebuilt LLVM:
```powershell
# Download
Invoke-WebRequest -Uri "https://github.com/llvm/llvm-project/releases/download/llvmorg-21.1.6/clang+llvm-21.1.6-x86_64-pc-windows-msvc.tar.xz" -OutFile "llvm.tar.xz"

# Extract (requires 7-Zip)
7z x llvm.tar.xz
7z x llvm.tar

# Set environment variable
$env:LLVM_SYS_211_PREFIX = "C:\path\to\clang+llvm-21.1.6-x86_64-pc-windows-msvc"
```

### Building

```bash
git clone https://github.com/kiv-lang/kivc.git
cd kivc
cargo build --release
```

### Pre-built Binaries

Pre-built binaries are automatically generated for each commit and available as GitHub Actions artifacts:

#### Stable Builds (main branch)
- **Linux (x86_64)**: `kivc-stable-linux-x86_64`
- **macOS (x86_64)**: `kivc-stable-macos-x86_64`
- **macOS (ARM64)**: `kivc-stable-macos-aarch64`
- **Windows (x86_64)**: `kivc-stable-windows-x86_64`

Artifacts are retained for 90 days.

#### Nightly Builds (develop branch)
- **Linux (x86_64)**: `kivc-nightly-linux-x86_64`
- **macOS (x86_64)**: `kivc-nightly-macos-x86_64`
- **macOS (ARM64)**: `kivc-nightly-macos-aarch64`
- **Windows (x86_64)**: `kivc-nightly-windows-x86_64`

Artifacts are retained for 30 days.

To download artifacts, go to the [Actions tab](https://github.com/kiv-lang/kivc/actions), select a successful workflow run, and download the artifacts at the bottom of the page.

### Testing

```bash
# Run all tests
cargo test

# Test specific crate
cargo test -p kivc-span
```

### Development Workflow

1. **Make changes** to the relevant crate(s)
2. **Run tests** to ensure functionality
3. **Check formatting**: `cargo fmt`
4. **Run clippy**: `cargo clippy -- -D warnings`
5. **Commit** with proper DCO signing: `git commit -s`

## Contributing

We welcome contributions! Please see the main [Kiv repository](https://github.com/kiv-lang/kiv) for contribution guidelines.

All contributions require signing off with the Developer Certificate of Origin (DCO):

```bash
git commit -s
```

## License

Licensed under either of:

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE))
- MIT License ([LICENSE-MIT](LICENSE-MIT))

at your option.

## Relation to Main Repository

This is the **compiler implementation** sub-repository. The main [Kiv repository](https://github.com/kiv-lang/kiv) contains:

- Language specification
- Standard library
- Documentation and website
- Community resources

For language questions, standard library contributions, or general discussions, please use the main repository. For compiler-specific issues and contributions, use this repository.
