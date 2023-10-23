# Introduction

## Seeking

We use **seeking** to handle where we are looking in the binary at some time. `radare2` maintains a **seek address** to determine where we are in the binary. This is shown on the command line:

```nasm
[0x0804923c]> 
```

In this case, `0x0804923c` is our seek address. We can use the `s` command to change the seek address:

```nasm
[0x0804923c]> s 0x0
[0x00000000]> 
```

When using various modules within `radare2`, the program defaults to our seek address when an address is requested. For example, if we use the `pdf` command, it will print the disassembly of the function at the seek address:

```nasm
[0x00000000]> s main
[0x0804923c]> pdf
            ;-- eax:
            ;-- eip:
            ; DATA XREFS from entry0 @ 0x80490b0(r), 0x80490b6(w)
┌ 24: int main (int argc, char **argv, char **envp);
│           0x0804923c b    55             push ebp
│           0x0804923d      89e5           mov ebp, esp
│           0x0804923f      83e4f0         and esp, 0xfffffff0
│           0x08049242      e80d000000     call sym.__x86.get_pc_thunk.ax
│           0x08049247      05b92d0000     add eax, 0x2db9
│           0x0804924c      e89effffff     call sym.read_in
│           0x08049251      90             nop
│           0x08049252      c9             leave
└           0x08049253      c3             ret
```

However, we can use the `@` symbol to specify a different address:

```nasm
[0x0804923c]> s 0
[0x00000000]> pdf @ main
            ;-- eax:
            ;-- eip:
            ; DATA XREFS from entry0 @ 0x80490b0(r), 0x80490b6(w)
┌ 24: int main (int argc, char **argv, char **envp);
│           0x0804923c b    55             push ebp
│           0x0804923d      89e5           mov ebp, esp
│           0x0804923f      83e4f0         and esp, 0xfffffff0
│           0x08049242      e80d000000     call sym.__x86.get_pc_thunk.ax
│           0x08049247      05b92d0000     add eax, 0x2db9
│           0x0804924c      e89effffff     call sym.read_in
│           0x08049251      90             nop
│           0x08049252      c9             leave
└           0x08049253      c3             ret
```