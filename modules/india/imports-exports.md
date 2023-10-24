# Imports and Exports

Imports and exports are useful for understanding the function calls, external libraries imported, and the overall functionality of the binary.

## Imports

Imports and exports are important for understanding the libraries imported into the binary and the externally visible and usable functions.

Function imports are essential for understanding the library functions used in the binary. This provides a massive hint into the functionality of the binary. Use \`i\`\` to list the imports.

```nasm
[0x00000000]> ii
[Imports]
nth vaddr      bind   type   lib name
―――――――――――――――――――――――――――――――――――――
1   0x08049040 GLOBAL FUNC       __libc_start_main
2   0x08049050 GLOBAL FUNC       fflush
3   0x08049060 GLOBAL FUNC       gets
4   0x08049070 GLOBAL FUNC       puts
5   0x08049080 GLOBAL FUNC       system
6   ---------- WEAK   NOTYPE     __gmon_start__
7   ---------- GLOBAL OBJ        stdout
```

## Exports

Use `iE` to list the exports. This is useful for understanding the functions that are externally visible and usable. This is especially useful for understanding the functions that are called from other binaries.

```nasm
[0x00000000]> iE
[Exports]
nth paddr      vaddr      bind   type   size lib name                    demangled
――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――
8   0x00002004 0x0804a004 GLOBAL OBJ    4        _IO_stdin_used
19  0x000010e0 0x080490e0 GLOBAL FUNC   4        __x86.get_pc_thunk.bx
23  ---------- 0x0804c028 GLOBAL NOTYPE 0        _edata
24  0x00001258 0x08049258 GLOBAL FUNC   0        _fini
25  0x00003020 0x0804c020 GLOBAL NOTYPE 0        __data_start
29  0x00003024 0x0804c024 GLOBAL OBJ    0        __dso_handle
30  0x00002004 0x0804a004 GLOBAL OBJ    4        _IO_stdin_used
31  0x000011a6 0x080491a6 GLOBAL FUNC   73       win
32  ---------- 0x0804c02c GLOBAL NOTYPE 0        _end
33  0x000010d0 0x080490d0 GLOBAL FUNC   5        _dl_relocate_static_pie
34  0x00001090 0x08049090 GLOBAL FUNC   49       _start
35  0x00002000 0x0804a000 GLOBAL OBJ    4        _fp_hw
37  ---------- 0x0804c028 GLOBAL NOTYPE 0        __bss_start
38  0x0000123c 0x0804923c GLOBAL FUNC   24       main
39  0x00001254 0x08049254 GLOBAL FUNC   0        __x86.get_pc_thunk.ax
40  0x000011ef 0x080491ef GLOBAL FUNC   77       read_in
41  ---------- 0x0804c028 GLOBAL OBJ    0        __TMC_END__
42  0x00001000 0x08049000 GLOBAL FUNC   0        _init
```