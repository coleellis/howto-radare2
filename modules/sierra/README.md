# Seeking: The s Module

**Seeking** is the process of moving around the file. `radare2` maintains a **seek address** to determine where we are in the binary. This is shown on the command line:

```nasm
[0x0804923c]> 
```

In this case, `0x0804923c` is our seek address.

## Seeking to an Address

We can use the `s` command to change the seek address:

```nasm
[0x0804923c]> s 0x0
[0x00000000]> 
```

You can seek to an expression such as an address, offset, or register.

```nasm
[0x080491f0]> s esp
[0xffbd7d40]> 
```

```nasm
[0x080491f3]> s main
[0x0804923c]> 
```

```nasm
[0xffbd7d40]> s sym.read_in + 4
[0x080491f3]> 
```

## Seek History

We use `s` to print the seeking history. This takes several secondary parameters of how the data should be printed. Scroll through the following to see the various types:

{% tabs %}
{% tab title="Verbose" %}
```nasm
[0x08049240]> s*
f undo_2 @ 0x80491ef
f undo_1 @ 0x8048000
f undo_0 @ 0x804923c
```
{% endtab %}

{% tab title="Linked List" %}
```nasm
[0x08049240]> s=
0x80491ef > 0x8048000 > 0x804923c
```
{% endtab %}

{% tab title="Names" %}
```nasm
[0x08049240]> s!
0x80491ef sym.read_in
0x8048000 segment.LOAD0
0x804923c main
0x8049240 main + 4
```
{% endtab %}

{% tab title="JSON" %}
```nasm
[0x08049240]> sj
[{"offset":134517231,"name":"sym.read_in"},{"offset":134512640,"name":"segment.LOAD0"},{"offset":134517308,"name":"main"},{"offset":134517312,"name":"main+4","current":true}]
```
{% endtab %}
{% endtabs %}

If we want to undo the seek, we can use `s-` to go back to the previous seek address:

```nasm
[0x00000000]> s-
[0x0804923c]> 
```

We can then redo this seek using `s+`:

```nasm
[0x0804923c]> s+
[0x00000000]> 
```

We can clear the seek history using `s-*`:

```nasm
[0x00000000]> s-*
```

## Relative Seeking

We can use `s+ <bytes>` to move `<bytes>` forward.

```nasm
[0x00000000]> s+ 4
[0x00000004]> 
```

We can use `s- <bytes>` to move `<bytes>` backward.

```nasm
[0x00000004]> s- 4
[0x00000000]> 
```

We can use `s++ <blocks>` to move `<blocks>` forward.

```nasm
[0x00000000]> s++ 1
[0x00000100]> 
```

We can use `s-- <blocks>` to move `<blocks>` backward.

```nasm
[0x00000100]> s-- 1
[0x00000000]> 
```

{% hint style="info" %}
#### How Many Bytes in a Block?

This depends on your configuration. Use `b` to get the current block size. `b <num>` is used to set the current block size.
{% endhint %}

## Seeking to Special Locations

Radare2 allows us to seek to various special locations in the binary. This is a good way to find the start of functions, strings, and comments without bloating the output.

### Seeking to Functions

The `sf` command lets us seek to the start of the _next_ function.

```nasm
[0x08049240]> s sym.read_in
[0x080491ef]> sf
[0x0804923c]> pd 1
            ; DATA XREFS from entry0 @ 0x80490b0(r), 0x80490b6(w)
┌ 24: int main (int argc, char **argv, char **envp);
│ rg: 0 (vars 0, args 0)
│ bp: 0 (vars 0, args 0)
│ sp: 0 (vars 0, args 0)
│           0x0804923c      55             push ebp
```

We can use `sf.` to seek to the _beginning of the current_ function.

```nasm
[0x08049246]> sf.
[0x0804923c]> pd 1
            ; DATA XREFS from entry0 @ 0x80490b0(r), 0x80490b6(w)
┌ 24: int main (int argc, char **argv, char **envp);
│ rg: 0 (vars 0, args 0)
│ bp: 0 (vars 0, args 0)
│ sp: 0 (vars 0, args 0)
│           0x0804923c      55             push ebp
```

{% hint style="info" %}
`pd 1` is used to print one line of disassembly at the current seek address. We use this to show that we're at the top of the `main` function.
{% endhint %}

You can use `sf <function>` to seek to a specific function. However, since `s <function>` also takes us to a function, this function is not very useful.

```nasm
[0x0804923c]> sf sym.read_in
[0x080491ef]>
```

### Seeking to Strings

We use `s/ <string>` to seek to the next occurrence of `<string>`.

```nasm
[0x0804923c]> s/ flag
Searching 4 bytes in [0x804923d-0x804a000]
hits: 0
Searching 4 bytes in [0x804a000-0x804b000]
0x0804a016 hit1_0 .You lose!cat flag.txtGood luck w.
[0x0804a016]> 
```

{% hint style="danger" %}
#### Search Bounds

Radare2 defaults to searching the current memory block for the string. We can change this setting by modifying the `e search.in` setting.

```nasm
[0x0804923c]> e search.in = dbg.maps
```

Changing this will cause `s/` to search the entire binary for the string.
{% endhint %}

We can also search for bytes by using `s/x` and specifying the hex value.

```nasm
[0x080491ef]> s/x 89e5
Searching 2 bytes in [0x80491f0-0x804a000]
0x080491f0 hit2_0 89e5
[0x080491f0]> pd 1
│           0x080491f0      89e5           mov ebp, esp
```

## Using Functions at Other Addresses

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
