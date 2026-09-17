# Nyet

Nyet is a small programming language: Lisp-like S-expression syntax, a
Rust-style ownership model that catches memory bugs at compile time,
and a real compiler that produces native executables.

```
(struct Point x:i32 y:i32)

(fn area (w:i32 h:i32) -> i32
  (* w h))

(fn main () -> unit
  (let p (Point x:3 y:4))
  (out! "area = " (area p.x p.y) "\n"))
```

This repository is where Nyet is **distributed**. The compiler itself
is closed source, so there is no compiler code here — what you will
find is the download, the documentation, browsable example programs,
and the issue tracker.

## Download

**[⬇ Download for Windows (x64)][latest]**

[latest]: https://github.com/NeonSupernova/nyet-releases/releases/latest

Everything needed to compile and run Nyet programs is in the one zip —
the compiler, a complete C/LLVM toolchain to link with, and a set of
example programs and games. Nothing needs to be installed, and no
admin rights are required: unzip it and go.

The download is large (around 400MB) because almost all of it is the
bundled toolchain that lets `nyet` produce `.exe` files entirely on its
own, with no other software on the machine.

Direct link, if you want to script it:

```
https://github.com/NeonSupernova/nyet-releases/releases/latest/download/nyet-windows-x64.zip
```

Every release also ships `SHA256SUMS.txt` so you can verify what you
downloaded:

```powershell
Get-FileHash nyet-windows-x64.zip -Algorithm SHA256
```

macOS and Linux builds are not published yet.

## Getting set up

1. Unzip it and copy the whole folder onto the machine. **Keep
   everything together in one folder** — don't pull `nyet.exe` out on
   its own, it needs the `clang\` and `_internal\` folders sitting
   right next to it.
2. Double-click `add-to-path.bat`. This lets you type `nyet` from any
   terminal window instead of having to `cd` into the folder every
   time. Close and reopen any terminal windows you already had open.
3. Open a **new** Command Prompt or PowerShell window and try:

   ```
   nyet run demo\01_hello.no
   ```

   You should see `hello from Nyet!`. If `nyet` isn't recognized,
   either open a fresh terminal window (it only picks up the PATH
   change on windows opened *after* step 2), or `cd` into the folder
   and run `.\nyet.exe` instead.

### A heads-up about Windows security warnings

The first time Windows notices `nyet.exe` — or any program `nyet`
compiles for you — it will likely show a blue "Windows protected your
PC" screen. This is expected: it means these are freshly built
programs that aren't digitally signed, not that anything is wrong.
Click **More info**, then **Run anyway**, and it won't ask again for
that file.

If a compiled program seems to vanish or refuses to run with no
warning at all, antivirus software may have quietly quarantined it —
worth checking, especially on managed or school computers.

If the computer resets everything on logoff, the PATH shortcut from
step 2 won't stick. That's fine: `cd` into the folder and run
`.\nyet.exe ...` instead of `nyet ...`; it does exactly the same thing.

## Using the compiler

```
nyet run file.no              # compile and immediately run
nyet check file.no            # check for errors, produce no executable
nyet -o myprogram.exe file.no # compile to a named executable
nyet --version                # which build you have
```

## The arcade — start here

The quickest way to see what Nyet can do is the arcade, a menu that
links five small games and tools into one program:

```
nyet run arcade\main.no
```

| # | Game | What it shows off |
|---|---|---|
| 1 | minibase | A tiny console database — add, rename, delete, save/load, search. |
| 2 | adventure | A pocket text adventure — explore a few connected rooms. |
| 3 | game of life | Conway's Game of Life, animated in the terminal, with an editor so you can draw your own starting board. |
| 4 | pipe dreams | A number puzzle built around Nyet's `\|>` pipe operator — chain simple steps to hit a target. |
| 5 | hangman | Classic word-guessing, complete with ASCII gallows art. |

Each also runs on its own, same code either way: `nyet run
minibase\main.no`, `nyet run adventure\main.no`, and so on.

## Browsing the language

You don't have to download anything to read Nyet. Everything in the
bundle is mirrored under [`examples/`](examples/) in this repo:

| Path | What's there |
|---|---|
| [`examples/demo/`](examples/demo/) | Short programs, one language feature each — the guided tour. |
| [`examples/lib/`](examples/lib/) | The arcade suite's real logic: structs, pattern matching, the borrow checker, `\|>` pipelines. |
| [`examples/programs/`](examples/programs/) | Each game's `main.no` entry point. |
| [`examples/language-reference.no`](examples/language-reference.no) | The whole language in one heavily commented file. |

A natural reading order in `examples/demo/`:

| File | What it shows off |
|---|---|
| `01_hello.no` | Hello world — the simplest possible Nyet program. |
| `02_structs_and_match.no` | Structs and pattern matching, via a shape-area calculator. |
| `03_ownership.no` | The ownership model in action — moves and borrows. |
| `03_ownership_bad.no` | The same idea, broken on purpose. The compiler catches a real bug before the program ever runs. Use `nyet check` here, not `run` — it's *supposed* to fail. |
| `04_generics_and_option.no` | Generics and an `Option[T]` that makes "forgot the empty case" bugs impossible. |
| `05_closures.no` | Closures and functions taking other functions as arguments. |

A note on `examples/language-reference.no`: it is the language
specification, and parts of it describe where Nyet is headed rather
than what's finished today. It is not guaranteed to compile start to
finish — treat it as a tour, not a test.

## Language at a glance

- **Primitives** — `i8`–`i64`, `u8`–`u64`, `usize`, `f32`, `f64`,
  `bool`, `char`, `string`, `unit`.
- **Bindings** are immutable by default (`let`); `var` makes one
  mutable.
- **Ownership** — move semantics with a compile-time borrow checker.
  Use-after-move and aliasing an exclusive borrow are hard errors.
- **Types** — structs, sum types, exhaustive pattern matching,
  generics via monomorphization, traits with operator overloading.
- **Also** — closures, hygienic macros, modules, file IO.

## Reporting a problem

[Open an issue](https://github.com/NeonSupernova/nyet-releases/issues/new/choose).
Bug reports about the compiler are welcome here even though its source
lives elsewhere — this is the right place for them. Please include the
release version, your OS, the `.no` program that triggers it, and the
exact output you got.

## Licensing

The Nyet compiler is proprietary. See [LICENSE](LICENSE) for the terms
the released binaries are distributed under, and
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md) for the third-party
toolchain the bundle redistributes.

The example programs under [`examples/`](examples/) are provided so you
can read, run, and learn from them — see
[`examples/LICENSE`](examples/LICENSE).
