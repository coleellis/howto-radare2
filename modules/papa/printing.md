---
description: Formatting Data written in Memory.
---

# Printing Data

The printing module is responsible for printing data in various ways. We can print raw data (in decimal, hex, or other basic types) or disassembly.

In Von Neumann architecture (the architecture used in most modern computing), the CPU cannot distinguish between data and instructions. This means we can print any data as raw data or as an instruction.

## Printing Raw Data

This section explains how to print raw data from a binary, including hexadecimal, decimal, strings, and other basic data types.

### Basic Data Types

There is a long list of basic data types. You can use `pf??` for the format characters and `pf???` for examples of these types. The most important types available are:

```nasm
Format:
|  c       char (signed byte)
|  f       float value (4 bytes)
|  F       double value (8 bytes)
|  G       long double value (16 bytes (10 with padding))
|  i       signed integer value (4 bytes) (see 'd' and 'x')
|  p       pointer reference (2, 4 or 8 bytes)
|  s       32bit pointer to string (4 bytes)
|  S       64bit pointer to string (8 bytes)
|  T       show Ten first bytes of buffer
|  x       0xHEX value and flag (fd @ addr) (see 'd' and 'i')
|  X       show formatted hexpairs
|  ?       data structure `pf ? (struct_name)example_name`
```

Accompanying each type is a corresponding size. You can choose the size of the output. The available sizes are:

```nasm
Sizes:
|  b       byte (unsigned)
|  w       word (2 bytes unsigned short in hex)
|  d       dword (4 bytes in hex) (see 'i' and 'x')
|  q       quadword (8 bytes)
|  Q       uint128_t (16 bytes)
```

Let's look at some examples of this in action. We most commonly use hexadecimal output and then the size of an address in the binary. We can also specify the number of bytes to output.

{% hint style="info" %}
The default number of bytes is rather large. It's recommended to choose an output size. The default is configured so it's easier to examine the stack.
{% endhint %}

```nasm
[0x0804923c]> pxw 4
0x0804923c  0x83e58955                                   U...
[0x0804923c]> pi 1
push ebp
[0x0804923c]> pd 1 @sym.win+60
│  sym.win + 60             0x080491e2      e899feffff     call sym.imp.system ; int system(const char *string)
```

We can use `pf` to print format: this prints an address based on the data type. Here are some examples.

```nasm
[0x0804923c]> pf i
0x0804923c = -2082109099
[0x0804923c]> pf D
push ebp
[0x0804923c]> pf x
0x0804923c = 0x83e58955
[0x0804923c]> pf D @ sym.win + 60
call sym.imp.system
```

### Printing Strings

The `ps` submodule handles the printing of strings. There are a few formats for printing strings. The most common is to print null-terminated strings. The `ps` or `psz` command handles this:

```nasm
[0x0804923c]> ps @ 0x0804a008
You lose!
[0x0804923c]> psz @ 0x0804a012
cat flag.txt
```

### High-Level Language Views

`radare2` supports a variety of high-level languages. These languages are used to print data in a more human-readable format. The most common languages are:

```nasm
| pc   C
| pcd  C dwords (8 byte)
| pch  C half-words (2 byte)
| pcw  C words (4 byte)
| pcj  json
| pcs  string
| pcp  python
```

Here are some examples of this in action:

```nasm
[0x0804923c]> pc 4
#define _BUFFER_SIZE 4
const uint8_t buffer[_BUFFER_SIZE] = {
  0x55, 0x89, 0xe5, 0x83
};
[0x0804923c]> pcs 4
"\x55\x89\xe5\x83"
[0x0804923c]> pcp 4
import struct
buf = struct.pack ("4B", *[
0x55,0x89,0xe5,0x83])
```

## Printing Disassembly

This section involves the printing of assembly code in the binary. The `pd` submodule is responsible for printing disassembly.

There are two pieces to the `pd` submodule:

* `pd`: Disassemble `N` instructions
* `pD`: Disassemble `N` bytes

We will primarily use `pd` since we are more often interested in disassembling entire instructions rather than bytes.

### Disassembling Instructions

The `pd` function is used to print disassembly. It takes a number as an argument: the number of instructions to disassemble.

```nasm
[0x0804923d]> pd 1
│  main + 1              0x0804923d      89e5           mov ebp, esp
```

If you're at the start of a function, it will also print the header:

```nasm
[0x0804923c]> pd 1
   main + 0              ; DATA XREFS from entry0 @ 0x80490b0(r), 0x80490b6(w)
┌ 24: int main (int argc, char **argv, char **envp);
│ rg: 0 (vars 0, args 0)
│ bp: 0 (vars 0, args 0)
│ sp: 0 (vars 0, args 0)
│  main + 0              0x0804923c      55             push ebp
```

By default, `radare2` chooses to dissect `64` instructions. This bloats all output, so it is recommended to use a more meaningful number.

To disassemble a number of bytes, use `pD`. By default, this will continue disassembling until an invalid instruction is hit; therefore, you should always input a number of bytes.

```pd
[0x080491f0]> pD 1
│  sym.read_in + 1              0x080491f0      89             invalid
[0x080491f0]> pD 2
│  sym.read_in + 1              0x080491f0      89e5           mov ebp, esp
```

The first output isn't helpful to us. This is why we use `pd` and not `pD` for most disassembling purposes.

### Disassembling Functions

To disassemble an entire function, use `pdf`. This function finds the current function and disassembles the whole function, _no matter where you are inside the function_.

```nasm
[0x080491f0]> pdf
   sym.read_in + 0              ; CALL XREF from main @ 0x804924c(x)
┌ 77: sym.read_in ();
│  sym.read_in + 0              ; var int32_t var_4h @ ebp-0x4
│  sym.read_in + 0              ; var int32_t var_30h @ ebp-0x30
│  sym.read_in + 0              0x080491ef      55             push ebp
│  sym.read_in + 1              0x080491f0      89e5           mov ebp, esp
│  sym.read_in + 3              0x080491f2      53             push ebx
│  sym.read_in + 4              0x080491f3      83ec34         sub esp, 0x34
│  sym.read_in + 7              0x080491f6      e8e5feffff     call sym.__x86.get_pc_thunk.bx
│  sym.read_in + 12             0x080491fb      81c3052e0000   add ebx, 0x2e05
│  sym.read_in + 18             0x08049201      83ec0c         sub esp, 0xc
│  sym.read_in + 21             0x08049204      8d831fe0ffff   lea eax, [ebx - 0x1fe1]
│  sym.read_in + 27             0x0804920a      50             push eax
│  sym.read_in + 28             0x0804920b      e860feffff     call sym.imp.puts ; int puts(const char *s)
│  sym.read_in + 33             0x08049210      83c410         add esp, 0x10
│  sym.read_in + 36             0x08049213      8b83fcffffff   mov eax, dword [ebx - 4]
│  sym.read_in + 42             0x08049219      8b00           mov eax, dword [eax]
│  sym.read_in + 44             0x0804921b      83ec0c         sub esp, 0xc
│  sym.read_in + 47             0x0804921e      50             push eax
│  sym.read_in + 48             0x0804921f      e82cfeffff     call sym.imp.fflush ; int fflush(FILE *stream)
│  sym.read_in + 53             0x08049224      83c410         add esp, 0x10
│  sym.read_in + 56             0x08049227      83ec0c         sub esp, 0xc
│  sym.read_in + 59             0x0804922a      8d45d0         lea eax, [var_30h]
│  sym.read_in + 62             0x0804922d      50             push eax
│  sym.read_in + 63             0x0804922e      e82dfeffff     call sym.imp.gets ; char *gets(char *s)
│  sym.read_in + 68             0x08049233      83c410         add esp, 0x10
│  sym.read_in + 71             0x08049236      90             nop
│  sym.read_in + 72             0x08049237      8b5dfc         mov ebx, dword [var_4h]
│  sym.read_in + 75             0x0804923a      c9             leave
└  sym.read_in + 76             0x0804923b      c3             ret
```

You can use the `@` operator to choose a function to disassemble.

```nasm
[0x080491f0]> pdf@main
   main + 0              ; DATA XREFS from entry0 @ 0x80490b0(r), 0x80490b6(w)
┌ 24: int main (int argc, char **argv, char **envp);
│  main + 0              0x0804923c      55             push ebp
│  main + 1              0x0804923d      89e5           mov ebp, esp
│  main + 3              0x0804923f      83e4f0         and esp, 0xfffffff0
│  main + 6              0x08049242      e80d000000     call sym.__x86.get_pc_thunk.ax
│  main + 11             0x08049247      05b92d0000     add eax, 0x2db9
│  main + 16             0x0804924c      e89effffff     call sym.read_in
│  main + 21             0x08049251      90             nop
│  main + 22             0x08049252      c9             leave
└  main + 23             0x08049253      c3             ret
```

### Disassembly Syntax

`radare2` supports a variety of assembly syntax. It defaults to `intel` syntax, but you can change this based on your preference. Use `e asm.syntax=?` to see the list of options:

```nasm
[0x00000000]> e asm.syntax=?
att
intel
masm
jz
regnum
```

To switch between AT\&T and Intel syntax:

```nasm
[0x00000000]> e asm.syntax = intel
[0x00000000]> e asm.syntax = att
```
