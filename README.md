Unicorn Engine
==============

[![pypi downloads](https://pepy.tech/badge/unicorn)](https://pepy.tech/project/unicorn)
[![Fuzzing Status](https://oss-fuzz-build-logs.storage.googleapis.com/badges/unicorn.svg)](https://bugs.chromium.org/p/oss-fuzz/issues/list?sort=-opened&can=1&q=proj:unicorn)

**LOOKING FOR CONTRIBUTORS!** See [this](https://github.com/unicorn-engine/unicorn/issues/2237).
==============

<p align="center">
<img width="250" src="docs/unicorn-logo.png">
</p>

Unicorn is a lightweight, multi-platform, multi-architecture CPU emulator framework, based on [QEMU](http://qemu.org).

Unicorn offers some unparalleled features:

- Multi-architecture: ARM, ARM64 (ARMv8), M68K, MIPS, PowerPC, RISCV, SPARC, S390X, TriCore and X86 (16, 32, 64-bit)
- Clean/simple/lightweight/intuitive architecture-neutral API
- Implemented in pure C language, with bindings for Crystal, Clojure, Visual Basic, Perl, Rust, Ruby, Python, Java, .NET, Go, Delphi/Free Pascal, Haskell, Pharo, Lua and Zig.
- Native support for Windows & *nix (with Mac OSX, Linux, Android, *BSD & Solaris confirmed)
- High performance via Just-In-Time compilation
- Support for fine-grained instrumentation at various levels
- Thread-safety by design
- Distributed under free software license GPLv2

Further information is available at http://www.unicorn-engine.org

HarmonyOS Split-WX Notes
------------------------

This fork includes a HarmonyOS-oriented workaround for the platform's anonymous executable memory restriction.

On recent HarmonyOS builds, anonymous memory can no longer be mapped or upgraded to executable with the usual JIT-style flow such as `mmap(PROT_EXEC | MAP_ANONYMOUS)` or `mprotect(..., PROT_EXEC)`. That breaks Unicorn's normal TCG code generation path because translated code is traditionally written into writable memory and then executed from that same anonymous region.

To make Unicorn usable on HarmonyOS, this fork switches the TCG code buffer to a Split-WX model:

- create a file-backed shared buffer instead of relying on anonymous executable memory
- keep one writable mapping for code generation
- keep a second executable mapping for execution
- translate addresses between the RW and RX views where TCG and TB jump logic need it
- flush the instruction cache against the executable view

In short: write and execute are separated, so the runtime no longer depends on anonymous JIT pages.

This solution direction was suggested by teachers from the Huawei ecosystem team. Special thanks to the Huawei ecosystem teachers for pointing us to the Split-WX approach and helping clarify how to adapt Unicorn to HarmonyOS platform restrictions.


License
-------

This project is released under the [GPL license](COPYING).


Compilation & Docs
------------------

See [docs/COMPILE.md](docs/COMPILE.md) file for how to compile and install Unicorn.

More documentation is available in [docs/README.md](docs/README.md).

For common questions, read [docs/FAQ.md](docs/FAQ.md) before raising an issue.

Contact
-------

[Contact us](http://www.unicorn-engine.org/contact/) via mailing list, email or twitter for any questions.


Join [our group](https://t.me/+lnNl0fPpyCYzZmVh) for instant feedback.

Contribute
----------

If you want to contribute, please pick up something from our [Github issues](https://github.com/unicorn-engine/unicorn/issues).

We also maintain a list of more challenged problems in [milestones](https://github.com/unicorn-engine/unicorn/milestones) for our regular release.

Please send pull request to our [dev branch](https://github.com/unicorn-engine/unicorn/tree/dev).

[CREDITS.TXT](CREDITS.TXT) records important contributors of our project.
