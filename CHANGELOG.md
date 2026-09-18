# Changelog

Notable changes to released Nyet builds. Dates are the release date.

This file covers what users can observe: language features, compiler
behaviour, diagnostics, and the contents of the distributed bundle.

The format follows [Keep a Changelog](https://keepachangelog.com/).

## [0.4.1] — 2026-09-17

### Fixed

- A macro call the compiler couldn't expand produced a confusing error
  about generated code instead of naming the problem. Building a
  program whose macro isn't in scope — a typo, or a macro whose module
  was never loaded — reported something like
  `output.ll:5481:24: error: expected '(' in call`, pointing into
  compiler output nobody wrote. It now says which macro it is:
  `unknown macro 'out!': no macro named 'out' is in scope`.
- `(use some_module)` naming a module that doesn't exist compiled the
  program anyway, silently missing everything that module defined —
  the usual way to hit the error above. A module that can't be found
  is now a hard error naming it and the paths that were tried.

## [0.4.0] — 2026-09-17

### Changed — this one breaks existing programs

- **Calling a macro now needs a trailing `!`**: `(assert! cond)`,
  `(out! "hi")`, `(unless! done (retry))`. Declarations keep their bare
  name — the bang belongs to the call — so `(macro unless ...)` is
  unchanged. A macro called without the bang is no longer expanded and
  fails as an undefined name. This also lets a macro and a function
  share a name, since only the call site distinguishes them.

### Added

- **IO channels.** One `io` verb writes to and reads from any channel:
  `(io ch "text")` writes, `(io ch)` reads. `FileIO` opens a real file
  in `Read`, `Write` or `Append` mode, and `out`/`err`/`in` are the
  standard channels behind the `out!`/`err!` sugar. Your own type
  becomes a channel by implementing the `IOChannel` trait, and the raw
  `write`/`read`/`close` calls return a `Result` to `match` on instead
  of panicking.
- **Closures that capture their environment** — `fn` captures by
  reference, `move fn` by value. Function values are real closure
  records, so a closure can outlive the expression that built it.
- **`file_read_lines`** reads a file into an `Array[string]`, one
  element per line, following Python's `splitlines` rules.
- **`std/ansi.no`** — terminal styling as ordinary Nyet source: SGR
  styles, the 16 standard colors, 256-color and 24-bit truecolor,
  hex helpers, and nearest-256 approximation for terminals without
  truecolor. **`std/io.no`** — `prompt_line` and `busy_wait`.
- **Stricter borrow checking.** A value can have at most one live `&!`
  borrow, and never an `&!` alongside a `&`; writing through a shared
  `&` reference is rejected.

### Fixed

- `(len s)` on a string read the bytes of its text as a length and
  returned a garbage number, so every blank-string check built on it
  silently passed.
- A `char` printed as its numeric codepoint instead of the character:
  `(let ch:char 65) (out! ch)` printed `65`, not `A`.
- `busy_wait` and anything else built on `(now)` ran about 1000x longer
  than asked on Windows, which froze Game of Life on reseed and Pipe
  Dreams on a move. The arcade also no longer wipes a game's closing
  message when returning to the menu.
- Arithmetic over three or more operands (`(+ a b c)`) dropped
  everything past the second operand, and `|>` called the result of a
  partial application instead of threading the value through.
- A `&!T` parameter of a scalar type mutated a private copy, so the
  caller never saw the change.
- `fmt` and `out!` silently skipped an argument that produced no value
  — printing a literal `{}` — and a module-qualified call like
  `(std/math/sqrt 4.0)` produced nothing at all. Both are errors now.
- `Array[T]` creation and indexing: nested `Array[Array[T]]` grids,
  chained indexing, and a generic sum type used at more than one
  concrete type in the same program (which silently miscompiled).

### The bundle

- `main.no` now compiles, runs, and asserts its own documented results,
  so the language tour in the bundle is verified rather than
  aspirational.

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
