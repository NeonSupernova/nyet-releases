# Changelog

Notable changes to released Nyet builds. Dates are the release date.

This file covers what users can observe: language features, compiler
behaviour, diagnostics, and the contents of the distributed bundle.

The format follows [Keep a Changelog](https://keepachangelog.com/).

## [0.3.0] — 2026-09-17

The first public build. Everything below is what the language does
today rather than a list of changes, since there is no earlier release
to compare against.

### The language

- **Primitives** — `i8`–`i64`, `u8`–`u64`, `usize`, `f32`, `f64`,
  `bool`, `char` (a Unicode scalar value), `string`, `unit`.
- **Ownership** — move semantics with a compile-time borrow checker.
  Use-after-move and aliasing an exclusive borrow are hard errors that
  block a build, not warnings.
- **Types** — structs, sum types, exhaustive pattern matching,
  generics via monomorphization, traits with operator overloading.
- **Also** — closures, hygienic macros, modules, file IO, explicit
  primitive casts with `(as expr type)`.

### The compiler

- Compiles to native executables through LLVM. No interpreter and no
  runtime dependency in the programs it produces.
- `nyet run`, `nyet check`, `nyet -o <exe>`, and `nyet --version`.
- A non-exhaustive `match` warns; a borrow error fails the build.

### The bundle

- Ships a complete MinGW-w64 + LLVM toolchain, so a Windows machine
  needs nothing else installed and no admin rights.
- Includes the arcade (five games behind one menu), the shorter
  single-feature demos, the arcade's own readable source under `lib\`,
  and `main.no` as a language tour.

### Known limitations

- Windows x64 only. No macOS or Linux build yet.
- The download is around 400MB, almost entirely the bundled toolchain.
- Nothing is code-signed, so Windows SmartScreen will warn the first
  time it sees `nyet.exe` or a program it compiled. See the README.
- `main.no` documents features beyond what is implemented and is not
  guaranteed to compile start to finish.
- Pre-1.0: the language may change incompatibly between releases.
