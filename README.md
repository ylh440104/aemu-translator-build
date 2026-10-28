# aemu-translator-build

Builds the ARM32 translator that AEmulator CE uses for Android 1.x–2.2.

The translator is QEMU 8.2.4 (linux-user, arm target) built from the SENSES source
distribution — https://github.com/Istratii-tech/SENSES — with all of its patches applied,
plus a change of our own: `target_set_read_implies_exec()` in `linux-user/mmap.c` adds
`PROT_EXEC` to every readable mapping when the firmware's main program has no
`PT_GNU_STACK`. Android 1.x binaries are built that way, and their PixelFlinger/Glide
assembler writes generated scanline routines into the heap and jumps there; without that
rule the guest takes SIGSEGV right after `ARMAssembler: generated scanline`.

Output: a statically linked aarch64 `qemu-arm`, musl-based, as a workflow artifact.

QEMU and the SENSES changes are GPL-2.0-or-later. See `patches/` and `src/`.