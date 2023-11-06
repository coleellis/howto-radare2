# Rabin2: File Analysis

`rabin2` is the command line front-end of the file analysis library in `radare2`. These commands are equivalent to the `i` (info) module.  For more information on file analysis, see the [File Analysis](../modules/india/README.md) page.

## Imports and Exports
We will use the `-i` and `-E` flags for imports and exports. These intentionally match the `ii` and `iE` commands inside `radare2`.

{% tabs %}
{% tab title="Imports" %}
```nasm
$ rabin2 -i ~/args
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
{% endtab %}
{% tab title="Exports" %}
```nasm
$ rabin2 -E ~/args
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
{% endtab %}
{% endtabs %}

## Symbols and Strings

Symbols and strings will be found using the `-s` and `-z` flag.  This mimics doing `is` and `iz` inside `radare2`.

{% tabs %}
{% tab title="Symbols" %}
```nasm
$ rabin2 -s ~/args
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
{% endtab %}
{% tab title="Strings" %}
```nasm
$ rabin2 -z ~/args
[Strings]
nth paddr      vaddr      len size section type  string
―――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x00002008 0x0804a008 9   10   .rodata ascii You lose!
1   0x00002012 0x0804a012 12  13   .rodata ascii cat flag.txt
2   0x0000201f 0x0804a01f 23  24   .rodata ascii Good luck winning here!
``` 
{% endtab %}
{% endtabs %}

## Program Sections

Use the `-S` flag for program sections. This mimics the `iS` command inside `radare2`.

```nasm
$ rabin2 -S ~/args
[Sections]

nth paddr        size vaddr       vsize perm type        name
―――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――
0   0x00000000    0x0 0x00000000    0x0 ---- NULL
1   0x00000194   0x13 0x08048194   0x13 -r-- PROGBITS    .interp
2   0x000001a8   0x24 0x080481a8   0x24 -r-- NOTE        .note.gnu.build-id
3   0x000001cc   0x20 0x080481cc   0x20 -r-- NOTE        .note.ABI-tag
4   0x000001ec   0x20 0x080481ec   0x20 -r-- GNU_HASH    .gnu.hash
5   0x0000020c   0x90 0x0804820c   0x90 -r-- DYNSYM      .dynsym
6   0x0000029c   0x6f 0x0804829c   0x6f -r-- STRTAB      .dynstr
7   0x0000030c   0x12 0x0804830c   0x12 -r-- GNU_VERSYM  .gnu.version
8   0x00000320   0x30 0x08048320   0x30 -r-- GNU_VERNEED .gnu.version_r
9   0x00000350   0x10 0x08048350   0x10 -r-- REL         .rel.dyn
10  0x00000360   0x28 0x08048360   0x28 -r-- REL         .rel.plt
11  0x00001000   0x24 0x08049000   0x24 -r-x PROGBITS    .init
12  0x00001030   0x60 0x08049030   0x60 -r-x PROGBITS    .plt
13  0x00001090  0x1c8 0x08049090  0x1c8 -r-x PROGBITS    .text
14  0x00001258   0x18 0x08049258   0x18 -r-x PROGBITS    .fini
15  0x00002000   0x37 0x0804a000   0x37 -r-- PROGBITS    .rodata
16  0x00002038   0x44 0x0804a038   0x44 -r-- PROGBITS    .eh_frame_hdr
17  0x0000207c   0xe4 0x0804a07c   0xe4 -r-- PROGBITS    .eh_frame
18  0x00002f08    0x4 0x0804bf08    0x4 -rw- INIT_ARRAY  .init_array
19  0x00002f0c    0x4 0x0804bf0c    0x4 -rw- FINI_ARRAY  .fini_array
20  0x00002f10   0xe8 0x0804bf10   0xe8 -rw- DYNAMIC     .dynamic
21  0x00002ff8    0x8 0x0804bff8    0x8 -rw- PROGBITS    .got
22  0x00003000   0x20 0x0804c000   0x20 -rw- PROGBITS    .got.plt
23  0x00003020    0x8 0x0804c020    0x8 -rw- PROGBITS    .data
24  0x00003028    0x0 0x0804c028    0x4 -rw- NOBITS      .bss
25  0x00003028   0x2d 0x00000000   0x2d ---- PROGBITS    .comment
26  0x00003058  0x2b0 0x00000000  0x2b0 ---- SYMTAB      .symtab
27  0x00003308  0x21f 0x00000000  0x21f ---- STRTAB      .strtab
28  0x00003527  0x101 0x00000000  0x101 ---- STRTAB      .shstrtab
```

## Help Page
```nasm
$ rabin2 -h
Usage: rabin2 [-AcdeEghHiIjlLMqrRsSUvVxzZ] [-@ at] [-a arch] [-b bits] [-B addr]
              [-C F:C:D] [-f str] [-m addr] [-n str] [-N m:M] [-P[-P] pdb]
              [-o str] [-O str] [-k query] [-D lang mangledsymbol] file
 -@ [addr]       show section, symbol or import at addr
 -A              list sub-binaries and their arch-bits pairs
 -a [arch]       set arch (x86, arm, .. or <arch>_<bits>)
 -b [bits]       set bits (32, 64 ...)
 -B [addr]       override base address (pie bins)
 -c              list classes
 -cc             list classes in header format
 -C [fmt:C:D]    create [elf,mach0,pe] with Code and Data hexpairs (see -a)
 -d              show debug/dwarf information
 -D lang name    demangle symbol name (-D all for bin.demangle=true)
 -e              program entrypoint
 -ee             constructor/destructor entrypoints
 -E              globally exportable symbols
 -f [str]        select sub-bin named str
 -F [binfmt]     force to use that bin plugin (ignore header check)
 -g              same as -SMZIHVResizcld -SS -SSS -ee (show all info)
 -G [addr]       load address . offset to header
 -h              this help message
 -H              header fields
 -i              imports (symbols imported from libraries)
 -I              binary info
 -j              output in json
 -k [sdb-query]  run sdb query. for example: '*'
 -K [algo]       calculate checksums (md5, sha1, ..)
 -l              linked libraries
 -L [plugin]     list supported bin plugins or plugin details
 -m [addr]       show source line at addr
 -M              main (show address of main symbol)
 -n [str]        show section, symbol or import named str
 -N [min:max]    force min:max number of chars per string (see -z and -zz)
 -o [str]        output file/folder for write operations (out by default)
 -O [str]        write/extract operations (-O help)
 -p              show always physical addresses
 -P              show debug/pdb information
 -PP             download pdb file for binary
 -q              be quiet, just show fewer data
 -qq             show less info (no offset/size for -z for ex.)
 -Q              show load address used by dlopen (non-aslr libs)
 -r              radare output
 -R              relocations
 -s              symbols
 -S              sections
 -SS             segments
 -SSS            sections mapping to segments
 -t              display file hashes
 -T              display file signature
 -u              unfiltered (no rename duplicated symbols/sections)
 -U              resoUrces
 -v              display version and quit
 -V              show binary version information
 -w              display try/catch blocks
 -x              extract bins contained in file
 -X [fmt] [f] .. package in fat or zip the given files and bins contained in file
 -z              strings (from data section)
 -zz             strings (from raw bins [e bin.str.raw=1])
 -zzz            dump raw strings to stdout (for huge files)
 -Z              guess size of binary program
Environment:
 R2_NOPLUGINS:     1|0|               # do not load shared plugins (speedup loading)
 RABIN2_ARGS:                         # ignore cli and use these program arguments
 RABIN2_CHARSET:   e cfg.charset      # set default value charset for -z strings
 RABIN2_DEBASE64:  e bin.str.debase64 # try to debase64 all strings
 RABIN2_DEMANGLE=0:e bin.demangle     # do not demangle symbols
 RABIN2_DMNGLRCMD: e bin.demanglercmd # try to purge false positives
 RABIN2_LANG:      e bin.lang         # assume lang for demangling
 RABIN2_MAXSTRBUF: e bin.str.maxbuf   # specify maximum buffer size
 RABIN2_PDBSERVER: e pdb.server       # use alternative PDB server
 RABIN2_PREFIX:    e bin.prefix       # prefix symbols/sections/relocs with a specific string
 RABIN2_STRFILTER: e bin.str.filter   # r2 -qc 'e bin.str.filter=??' -
 RABIN2_MACHO_NOFUNCSTART:           # if set it will ignore the FUNCSTART information
 RABIN2_MACHO_NOSWIFT
 RABIN2_MACHO_SKIPFIXUPS
 RABIN2_CODESIGN_VERBOSE
 RABIN2_STRPURGE:  e bin.str.purge    # try to purge false positives
 RABIN2_SYMSTORE:  e pdb.symstore     # path to downstream symbol store
 RABIN2_SWIFTLIB:  1|0|               # load Swift libsto demangle (default: true)
 RABIN2_VERBOSE:   e bin.verbose      # show debugging messages from the parser
```