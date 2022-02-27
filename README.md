# GBA Minimal Runtime

`gba-minrt` provides all the low-level components necessary to build ELF
executables for the Game Boy Advance. It is meant to be used with an unmodified
`arm-none-eabi` GCC toolchain, and forms a replacement for devkitARM's tooling.

It includes appropriate startup code (rt/crt0.s), a linker script (rt/rom.ld),
and a sample project with a Makefile suitable for C and Assembly (template/).

Consider this project experimental. It is not feature-complete, and has not yet
been thoroughly tested.

## License

All source code in this project is licensed under the
Mozilla Public License Version 2.0.\
See [LICENSE.txt](./LICENSE.txt) for more information.
