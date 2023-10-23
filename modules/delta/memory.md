# Memory Analysis

## Viewing Registers

Use the `dr` submodule to get more information about the registers.

{% hint style="info" %}
Use `dr?` to view the help pages for this submodule.
{% endhint %}

```nasm
[0x0804923c]> dr
eax = 0x0804923c
ebx = 0xf7e2a000
ecx = 0x8e819449
edx = 0xffe4a930
esi = 0xffe4a9c4
edi = 0xf7f47b80
esp = 0xffe4a90c
ebp = 0xf7f48020
eip = 0x0804923c
eflags = 0x00000246
oeax = 0xffffffff
```

## Viewing Memory Segments

Use the `dm` submodule to get more information about the memory segments.

```nasm
[0x0804923c]> dm
0x08048000 - 0x08049000 - usr     4K s r-- /home/joybuzzer/args /home/joybuzzer/args ; segment.ehdr
0x08049000 - 0x0804a000 * usr     4K s r-x /home/joybuzzer/args /home/joybuzzer/args ; map._home_joybuzzer_args.r_x
0x0804a000 - 0x0804b000 - usr     4K s r-- /home/joybuzzer/args /home/joybuzzer/args ; map._home_joybuzzer_args.r__
0x0804b000 - 0x0804c000 - usr     4K s r-- /home/joybuzzer/args /home/joybuzzer/args ; map._home_joybuzzer_args.rw_
0x0804c000 - 0x0804d000 - usr     4K s rw- /home/joybuzzer/args /home/joybuzzer/args ; obj._GLOBAL_OFFSET_TABLE_
0xf7c00000 - 0xf7c20000 - usr   128K s r-- /usr/lib/i386-linux-gnu/libc.so.6 /usr/lib/i386-linux-gnu/libc.so.6
0xf7c20000 - 0xf7da2000 - usr   1.5M s r-x /usr/lib/i386-linux-gnu/libc.so.6 /usr/lib/i386-linux-gnu/libc.so.6
0xf7da2000 - 0xf7e27000 - usr   532K s r-- /usr/lib/i386-linux-gnu/libc.so.6 /usr/lib/i386-linux-gnu/libc.so.6
0xf7e27000 - 0xf7e28000 - usr     4K s --- /usr/lib/i386-linux-gnu/libc.so.6 /usr/lib/i386-linux-gnu/libc.so.6
0xf7e28000 - 0xf7e2a000 - usr     8K s r-- /usr/lib/i386-linux-gnu/libc.so.6 /usr/lib/i386-linux-gnu/libc.so.6
0xf7e2a000 - 0xf7e2b000 - usr     4K s rw- /usr/lib/i386-linux-gnu/libc.so.6 /usr/lib/i386-linux-gnu/libc.so.6 ; ebx
0xf7e2b000 - 0xf7e35000 - usr    40K s rw- unk0 unk0
0xf7f09000 - 0xf7f0b000 - usr     8K s rw- unk1 unk1
0xf7f0b000 - 0xf7f0f000 - usr    16K s r-- [vvar] [vvar] ; map._vvar_.r__
0xf7f0f000 - 0xf7f11000 - usr     8K s r-x [vdso] [vdso] ; map._vdso_.r_x
0xf7f11000 - 0xf7f12000 - usr     4K s r-- /usr/lib/i386-linux-gnu/ld-linux.so.2 /usr/lib/i386-linux-gnu/ld-linux.so.2
0xf7f12000 - 0xf7f37000 - usr   148K s r-x /usr/lib/i386-linux-gnu/ld-linux.so.2 /usr/lib/i386-linux-gnu/ld-linux.so.2 ; map._usr_lib_i386_linux_gnu_ld_linux.so.2.r_x
0xf7f37000 - 0xf7f46000 - usr    60K s r-- /usr/lib/i386-linux-gnu/ld-linux.so.2 /usr/lib/i386-linux-gnu/ld-linux.so.2 ; map._usr_lib_i386_linux_gnu_ld_linux.so.2.r__
0xf7f46000 - 0xf7f48000 - usr     8K s r-- /usr/lib/i386-linux-gnu/ld-linux.so.2 /usr/lib/i386-linux-gnu/ld-linux.so.2 ; map._usr_lib_i386_linux_gnu_ld_linux.so.2.rw_
0xf7f48000 - 0xf7f49000 - usr     4K s rw- /usr/lib/i386-linux-gnu/ld-linux.so.2 /usr/lib/i386-linux-gnu/ld-linux.so.2
0xffe2c000 - 0xffe4d000 - usr   132K s rwx [stack] [stack] ; map._stack_.rwx
```

We can use the `dm.` command to find the memory segment of a specific address. It defaults to the seek address if no address is provided.

```nasm
[0x0804923c]> dm.
0x08049000 - 0x0804a000 * usr     4K s r-x /home/joybuzzer/args /home/joybuzzer/args ; map._home_joybuzzer_args.r_x

[0x0804923c]> dm. @ 0xffe4a90c
0xffe2c000 - 0xffe4d000 * usr   132K s rwx [stack] [stack] ; map._stack_.rwx
```

This submodule provides several commands to allocate, deallocate, and map virtual memory. I don't have any writeups using the write flag, but in the future, I might make this addition.

## Heap Analysis