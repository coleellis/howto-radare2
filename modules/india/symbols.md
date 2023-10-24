# Symbols and Strings

Symbols and strings are vital info.  Strings are super telling about the functionality of the binary, and symbols are useful for understanding the variables and functions.

## Strings

The `iz` submodule is responsible for finding strings. Based on the help pages for this submodule, we have the following options:

```nasm
[0x0804923c]> iz?
| iz[?][j]           strings in data sections (in JSON/Base64)
| iz*                print flags and comments r2 commands for all the strings
| izz                search for Strings in the whole binary
| izz*               same as iz* but exposing the strings of the whole binary
| izzz               dump Strings from whole binary to r2 shell (for huge files)
| iz- [addr]         purge string via bin.str.purge
```

The most important two commands here are `iz` and `izz`. The other commands perform similar functions, but these two format strings in the most readable way.

{% tabs %}
{% tab title="iz" %}
```nasm
[0x08049050]> iz
[Strings]
nth paddr      vaddr      len size section type  string
―――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x00002008 0x0804a008 9   10   .rodata ascii You lose!
1   0x00002012 0x0804a012 12  13   .rodata ascii cat flag.txt
2   0x0000201f 0x0804a01f 23  24   .rodata ascii Good luck winning here!
```
{% endtab %}

{% tab title="izz" %}
```nasm
[0x08049050]> izz
[Strings]
nth paddr      vaddr      len size section   type    string
―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x00000028 0x00000028 4   10             utf16le 4 \v(
1   0x00000136 0x00000136 4   5              ascii   td8 
2   0x00000176 0x00000176 4   5              ascii   td\b/
3   0x00000194 0x08048194 18  19   .interp   ascii   /lib/ld-linux.so.2
4   0x0000029d 0x0804829d 14  15   .dynstr   ascii   _IO_stdin_used
5   0x000002ac 0x080482ac 4   5    .dynstr   ascii   gets
6   0x000002b1 0x080482b1 6   7    .dynstr   ascii   stdout
7   0x000002b8 0x080482b8 17  18   .dynstr   ascii   __libc_start_main
8   0x000002ca 0x080482ca 4   5    .dynstr   ascii   puts
9   0x000002cf 0x080482cf 6   7    .dynstr   ascii   system
10  0x000002d6 0x080482d6 6   7    .dynstr   ascii   fflush
11  0x000002dd 0x080482dd 9   10   .dynstr   ascii   libc.so.6
12  0x000002e7 0x080482e7 9   10   .dynstr   ascii   GLIBC_2.0
13  0x000002f1 0x080482f1 10  11   .dynstr   ascii   GLIBC_2.34
14  0x000002fc 0x080482fc 14  15   .dynstr   ascii   __gmon_start__
15  0x00002008 0x0804a008 9   10   .rodata   ascii   You lose!
16  0x00002012 0x0804a012 12  13   .rodata   ascii   cat flag.txt
17  0x0000201f 0x0804a01f 23  24   .rodata   ascii   Good luck winning here!
18  0x000020db 0x0804a0db 6   7    .eh_frame ascii   ;*2$" 
19  0x00003028 0x00000000 44  45   .comment  ascii   GCC: (Ubuntu 11.3.0-1ubuntu1~22.04.1) 11.3.0
20  0x00003309 0x00000001 6   7    .strtab   ascii   crt1.o
21  0x00003310 0x00000008 9   10   .strtab   ascii   __abi_tag
22  0x0000331a 0x00000012 10  11   .strtab   ascii   crtstuff.c
23  0x00003325 0x0000001d 20  21   .strtab   ascii   deregister_tm_clones
24  0x0000333a 0x00000032 21  22   .strtab   ascii   __do_global_dtors_aux
25  0x00003350 0x00000048 11  12   .strtab   ascii   completed.0
26  0x0000335c 0x00000054 38  39   .strtab   ascii   __do_global_dtors_aux_fini_array_entry
27  0x00003383 0x0000007b 11  12   .strtab   ascii   frame_dummy
28  0x0000338f 0x00000087 30  31   .strtab   ascii   __frame_dummy_init_array_entry
29  0x000033ae 0x000000a6 6   7    .strtab   ascii   args.c
30  0x000033b5 0x000000ad 13  14   .strtab   ascii   __FRAME_END__
31  0x000033c3 0x000000bb 8   9    .strtab   ascii   _DYNAMIC
32  0x000033cc 0x000000c4 18  19   .strtab   ascii   __GNU_EH_FRAME_HDR
33  0x000033df 0x000000d7 21  22   .strtab   ascii   _GLOBAL_OFFSET_TABLE_
34  0x000033f5 0x000000ed 28  29   .strtab   ascii   __libc_start_main@GLIBC_2.34
35  0x00003412 0x0000010a 21  22   .strtab   ascii   __x86.get_pc_thunk.bx
36  0x00003428 0x00000120 16  17   .strtab   ascii   fflush@GLIBC_2.0
37  0x00003439 0x00000131 14  15   .strtab   ascii   gets@GLIBC_2.0
38  0x00003448 0x00000140 6   7    .strtab   ascii   _edata
39  0x0000344f 0x00000147 5   6    .strtab   ascii   _fini
40  0x00003455 0x0000014d 12  13   .strtab   ascii   __data_start
41  0x00003462 0x0000015a 14  15   .strtab   ascii   puts@GLIBC_2.0
42  0x00003471 0x00000169 16  17   .strtab   ascii   system@GLIBC_2.0
43  0x00003482 0x0000017a 14  15   .strtab   ascii   __gmon_start__
44  0x00003491 0x00000189 12  13   .strtab   ascii   __dso_handle
45  0x0000349e 0x00000196 14  15   .strtab   ascii   _IO_stdin_used
46  0x000034b1 0x000001a9 4   5    .strtab   ascii   _end
47  0x000034b6 0x000001ae 23  24   .strtab   ascii   _dl_relocate_static_pie
48  0x000034ce 0x000001c6 6   7    .strtab   ascii   _fp_hw
49  0x000034d5 0x000001cd 16  17   .strtab   ascii   stdout@GLIBC_2.0
50  0x000034e6 0x000001de 11  12   .strtab   ascii   __bss_start
51  0x000034f2 0x000001ea 4   5    .strtab   ascii   main
52  0x000034f7 0x000001ef 21  22   .strtab   ascii   __x86.get_pc_thunk.ax
53  0x0000350d 0x00000205 7   8    .strtab   ascii   read_in
54  0x00003515 0x0000020d 11  12   .strtab   ascii   __TMC_END__
55  0x00003521 0x00000219 5   6    .strtab   ascii   _init
56  0x00003528 0x00000001 7   8    .shstrtab ascii   .symtab
57  0x00003530 0x00000009 7   8    .shstrtab ascii   .strtab
58  0x00003538 0x00000011 9   10   .shstrtab ascii   .shstrtab
59  0x00003542 0x0000001b 7   8    .shstrtab ascii   .interp
60  0x0000354a 0x00000023 18  19   .shstrtab ascii   .note.gnu.build-id
61  0x0000355d 0x00000036 13  14   .shstrtab ascii   .note.ABI-tag
62  0x0000356b 0x00000044 9   10   .shstrtab ascii   .gnu.hash
63  0x00003575 0x0000004e 7   8    .shstrtab ascii   .dynsym
64  0x0000357d 0x00000056 7   8    .shstrtab ascii   .dynstr
65  0x00003585 0x0000005e 12  13   .shstrtab ascii   .gnu.version
66  0x00003592 0x0000006b 14  15   .shstrtab ascii   .gnu.version_r
67  0x000035a1 0x0000007a 8   9    .shstrtab ascii   .rel.dyn
68  0x000035aa 0x00000083 8   9    .shstrtab ascii   .rel.plt
69  0x000035b3 0x0000008c 5   6    .shstrtab ascii   .init
70  0x000035b9 0x00000092 5   6    .shstrtab ascii   .text
71  0x000035bf 0x00000098 5   6    .shstrtab ascii   .fini
72  0x000035c5 0x0000009e 7   8    .shstrtab ascii   .rodata
73  0x000035cd 0x000000a6 13  14   .shstrtab ascii   .eh_frame_hdr
74  0x000035db 0x000000b4 9   10   .shstrtab ascii   .eh_frame
75  0x000035e5 0x000000be 11  12   .shstrtab ascii   .init_array
76  0x000035f1 0x000000ca 11  12   .shstrtab ascii   .fini_array
77  0x000035fd 0x000000d6 8   9    .shstrtab ascii   .dynamic
78  0x00003606 0x000000df 4   5    .shstrtab ascii   .got
79  0x0000360b 0x000000e4 8   9    .shstrtab ascii   .got.plt
80  0x00003614 0x000000ed 5   6    .shstrtab ascii   .data
81  0x0000361a 0x000000f3 4   5    .shstrtab ascii   .bss
82  0x0000361f 0x000000f8 8   9    .shstrtab ascii   .comment
```
{% endtab %}
{% endtabs %}

## Symbols and Variables

Use the `is` command to list the available symbols.

```nasm
[0x0804923c]> is
[Symbols]
nth paddr      vaddr      bind   type   size lib name                                   demangled
―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――
8   0x00002004 0x0804a004 GLOBAL OBJ    4        _IO_stdin_used
1   ---------- 0x00000000 LOCAL  FILE   0        crt1.o
2   0x000001cc 0x080481cc LOCAL  OBJ    32       __abi_tag
3   ---------- 0x00000000 LOCAL  FILE   0        crtstuff.c
4   0x000010f0 0x080490f0 LOCAL  FUNC   0        deregister_tm_clones
5   0x00001130 0x08049130 LOCAL  FUNC   0        register_tm_clones
6   0x00001170 0x08049170 LOCAL  FUNC   0        __do_global_dtors_aux
7   ---------- 0x0804c028 LOCAL  OBJ    1        completed.0
8   0x00002f0c 0x0804bf0c LOCAL  OBJ    0        __do_global_dtors_aux_fini_array_entry
9   0x000011a0 0x080491a0 LOCAL  FUNC   0        frame_dummy
10  0x00002f08 0x0804bf08 LOCAL  OBJ    0        __frame_dummy_init_array_entry
11  ---------- 0x00000000 LOCAL  FILE   0        args.c
12  ---------- 0x00000000 LOCAL  FILE   0        crtstuff.c
13  0x0000215c 0x0804a15c LOCAL  OBJ    0        __FRAME_END__
14  ---------- 0x00000000 LOCAL  FILE   0
15  0x00002f10 0x0804bf10 LOCAL  OBJ    0        _DYNAMIC
16  0x00002038 0x0804a038 LOCAL  NOTYPE 0        __GNU_EH_FRAME_HDR
17  0x00003000 0x0804c000 LOCAL  OBJ    0        _GLOBAL_OFFSET_TABLE_
19  0x000010e0 0x080490e0 GLOBAL FUNC   4        __x86.get_pc_thunk.bx
20  0x00003020 0x0804c020 WEAK   NOTYPE 0        data_start
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
1   0x00001040 0x08049040 GLOBAL FUNC   16       imp.__libc_start_main
2   0x00001050 0x08049050 GLOBAL FUNC   16       imp.fflush
3   0x00001060 0x08049060 GLOBAL FUNC   16       imp.gets
4   0x00001070 0x08049070 GLOBAL FUNC   16       imp.puts
5   0x00001080 0x08049080 GLOBAL FUNC   16       imp.system
6   ---------- ---------- WEAK   NOTYPE 16       imp.__gmon_start__
7   ---------- ---------- GLOBAL OBJ    16       imp.stdout
```

To get the list of variables, we need to filter for the **objects** in this list. We can do this using the `~` operator (the `grep` operator).

```nasm
[0x0804923c]> is~OBJ
8   0x00002004 0x0804a004 GLOBAL OBJ    4        _IO_stdin_used
2   0x000001cc 0x080481cc LOCAL  OBJ    32       __abi_tag
7   ---------- 0x0804c028 LOCAL  OBJ    1        completed.0
8   0x00002f0c 0x0804bf0c LOCAL  OBJ    0        __do_global_dtors_aux_fini_array_entry
10  0x00002f08 0x0804bf08 LOCAL  OBJ    0        __frame_dummy_init_array_entry
13  0x0000215c 0x0804a15c LOCAL  OBJ    0        __FRAME_END__
15  0x00002f10 0x0804bf10 LOCAL  OBJ    0        _DYNAMIC
17  0x00003000 0x0804c000 LOCAL  OBJ    0        _GLOBAL_OFFSET_TABLE_
29  0x00003024 0x0804c024 GLOBAL OBJ    0        __dso_handle
30  0x00002004 0x0804a004 GLOBAL OBJ    4        _IO_stdin_used
35  0x00002000 0x0804a000 GLOBAL OBJ    4        _fp_hw
41  ---------- 0x0804c028 GLOBAL OBJ    0        __TMC_END__
7   ---------- ---------- GLOBAL OBJ    16       imp.stdout
```