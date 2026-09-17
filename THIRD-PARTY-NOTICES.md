# Third-Party Notices

The Nyet Windows bundle redistributes third-party software so that
`nyet` can produce native executables on a machine with nothing else
installed. Those components are **not** covered by Nyet's own
[LICENSE](LICENSE) — each is governed by its own license, reproduced
or linked below.

## The bundled toolchain (`clang\`)

The `clang\` folder is an unmodified [WinLibs][winlibs] distribution
of the MinGW-w64 toolchain with LLVM/Clang. The exact WinLibs release
used is recorded in the release notes of each Nyet release.

[winlibs]: https://winlibs.com/

It contains, among other components:

| Component | License |
|---|---|
| LLVM, Clang, LLD, compiler-rt | Apache License 2.0 with LLVM Exceptions |
| GCC | GNU GPL v3 with the GCC Runtime Library Exception |
| GNU Binutils | GNU GPL v3 |
| MinGW-w64 runtime and headers | Permissive (public-domain and BSD-style terms; see `clang\licenses\`) |
| GNU Make, GDB, and other GNU utilities | GNU GPL v3 |

The complete license texts ship inside the bundle, under
`clang\licenses\`.

### Source code for the GPL-licensed components

Some of the above are licensed under the GNU GPL, which entitles you to
the corresponding source code. Nyet does not modify any of these
components — the bundle contains upstream WinLibs binaries verbatim,
and the matching source is published by WinLibs alongside them:

- https://github.com/brechtsanders/winlibs_mingw/releases

Find the release named in the Nyet release notes and download its
source archive. If that link is ever unavailable, open an issue on
this repository and the source will be provided.

### A note on what the GPL covers here

The GPL-licensed components above are separate programs that `nyet`
invokes as external tools, in the same way any build system invokes a
compiler or linker. They are aggregated with Nyet on the same media,
not combined into it. This does not place the Nyet compiler under the
GPL, and does not give you rights to Nyet's source code. It does give
you full rights to the GPL-licensed components themselves, including
their source, as described above.

## Inside `nyet.exe`

`nyet.exe` is a frozen Python application built with
[PyInstaller][pyi]. It embeds:

[pyi]: https://pyinstaller.org/

| Component | License |
|---|---|
| CPython runtime and standard library | Python Software Foundation License 2.0 |
| PyInstaller bootloader | GNU GPL v2 or later, with the PyInstaller exception permitting its use in closed-source applications |

The Nyet compiler's own code, which runs on that embedded runtime, is
proprietary and is not published.

## Corrections

If you believe a component is redistributed here without proper
notice, or a license above is stated incorrectly, please
[open an issue](https://github.com/NeonSupernova/nyet-releases/issues)
— it will be fixed.
