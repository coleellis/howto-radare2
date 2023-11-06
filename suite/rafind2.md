# Rafind2: Pattern Matching

`rafind2` is the command line front-end of the searching library in `radare2`. These commands are equivalent to the `/` (searching) module.  For more information on searching, see the [Searching](../modules/papa/searching.md) page.

## String Searching

We can search for strings using the `-s` flag.  This will search for a specific string and return *the offset from the base address*.
```nasm
$ rafind2 -s "flag.txt" ~/args
0x2016
```

Use the `-z` flag to search for **all** zero-terminated strings. This will return the address and the string.
```nasm
$ rafind2 -z ~/args
0x00000028 4 \v(
0x00000136 td8 
0x00000176 td\b/
0x08048194 /lib/ld-linux.so.2
0x0804829d _IO_stdin_used
0x080482ac gets
0x080482b1 stdout
0x080482b8 __libc_start_main
0x080482ca puts
0x080482cf system
0x080482d6 fflush
0x080482dd libc.so.6
0x080482e7 GLIBC_2.0
0x080482f1 GLIBC_2.34
0x080482fc __gmon_start__
0x0804a008 You lose!
0x0804a012 cat flag.txt
0x0804a01f Good luck winning here!
0x0804a0db ;*2$" 
0x00000000 GCC: (Ubuntu 11.3.0-1ubuntu1~22.04.1) 11.3.0
0x00000001 crt1.o
0x00000008 __abi_tag
0x00000012 crtstuff.c
0x0000001d deregister_tm_clones
0x00000032 __do_global_dtors_aux
0x00000048 completed.0
0x00000054 __do_global_dtors_aux_fini_array_entry
0x0000007b frame_dummy
0x00000087 __frame_dummy_init_array_entry
0x000000a6 args.c
0x000000ad __FRAME_END__
0x000000bb _DYNAMIC
0x000000c4 __GNU_EH_FRAME_HDR
0x000000d7 _GLOBAL_OFFSET_TABLE_
0x000000ed __libc_start_main@GLIBC_2.34
0x0000010a __x86.get_pc_thunk.bx
0x00000120 fflush@GLIBC_2.0
0x00000131 gets@GLIBC_2.0
0x00000140 _edata
0x00000147 _fini
0x0000014d __data_start
0x0000015a puts@GLIBC_2.0
0x00000169 system@GLIBC_2.0
0x0000017a __gmon_start__
0x00000189 __dso_handle
0x00000196 _IO_stdin_used
0x000001a9 _end
0x000001ae _dl_relocate_static_pie
0x000001c6 _fp_hw
0x000001cd stdout@GLIBC_2.0
0x000001de __bss_start
0x000001ea main
0x000001ef __x86.get_pc_thunk.ax
0x00000205 read_in
0x0000020d __TMC_END__
0x00000219 _init
0x00000001 .symtab
0x00000009 .strtab
0x00000011 .shstrtab
0x0000001b .interp
0x00000023 .note.gnu.build-id
0x00000036 .note.ABI-tag
0x00000044 .gnu.hash
0x0000004e .dynsym
0x00000056 .dynstr
0x0000005e .gnu.version
0x0000006b .gnu.version_r
0x0000007a .rel.dyn
0x00000083 .rel.plt
0x0000008c .init
0x00000092 .text
0x00000098 .fini
0x0000009e .rodata
0x000000a6 .eh_frame_hdr
0x000000b4 .eh_frame
0x000000be .init_array
0x000000ca .fini_array
0x000000d6 .dynamic
0x000000df .got
0x000000e4 .got.plt
0x000000ed .data
0x000000f3 .bss
0x000000f8 .comment
```

## Regex Searching

Use the `-e` flag to search for a regular expression. This will return the address and the string.  Remember that `radare2` uses `/` for edge wildcards and `..` for centered wildcards. 
```nasm
$ rafind2 -e "/flag/" ~/args
0x2016
```