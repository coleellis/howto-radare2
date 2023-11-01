# Searching: The / Module

The search module allows us to search for data in the binary. This is useful for finding strings, functions, regex, and other patterns.

There are several configuration options for searching. These are located in the `search` namespace of the `e` module. We can view these options by using `e??search`.

{% hint style="info" %}
The most common search command I change is `search.in`. It defaults to the current debug map, which is rather inconvenient. To search all maps (hence the entire file), change the configuration to:

```nasm
[0x00000000]> e search.in = dbg.maps
```

There are other good locations you could use for searching. For example, searching `dbg.maps.rw` searches writeable memory, meaning you could find a string to overwrite.
{% endhint %}

## Searching for Strings

`radare2` supports a variety of string types. The most common searches will be ASCII strings.

### ASCII Strings

To search for an ASCII string, use the `/` command. It will search all regions in `e search.in` and return the results.

```nasm
[0xf7fc88a0]> / flag.txt
Searching 8 bytes in [0x8048000-0x8049000]
hits: 0
Searching 8 bytes in [0x8049000-0x804a000]
hits: 0
Searching 8 bytes in [0x804a000-0x804b000]
hits: 1
Searching 8 bytes in [0x804b000-0x804d000]
hits: 1
Searching 8 bytes in [0xf7fa6000-0xf7faa000]
hits: 0
Searching 8 bytes in [0xf7faa000-0xf7fac000]
hits: 0
Searching 8 bytes in [0xf7fac000-0xf7fad000]
hits: 0
Searching 8 bytes in [0xf7fad000-0xf7fd2000]
hits: 0
Searching 8 bytes in [0xf7fd2000-0xf7fe1000]
hits: 0
Searching 8 bytes in [0xf7fe1000-0xf7fe4000]
hits: 0
Searching 8 bytes in [0xffc3d000-0xffc5e000]
hits: 0
0x0804a016 hit2_0 .You lose!cat flag.txtGood luck winni.
0x0804b016 hit2_1 .You lose!cat flag.txtGood luck winni.
```

{% hint style="success" %}
In the `radare2` output, the hits are color-coded, so they're easier to find.
{% endhint %}

Use the `/i` command to perform a case-insensitive search. This is a very useful tool to have.

```nasm
[0x0804923c]> /i YoU LoSe
0x0804a008 hit7_0 .You lose!cat flag.txtG.
0x0804b008 hit7_1 .You lose!cat flag.txtG.
```

### Hex Strings

Hex strings are useful for searching for specific bytes. This is useful for finding instructions, raw data, or other patterns. Use `/x` to search for hex strings.

The below command searches for a function call to `sym.read_in`.

```nasm
[0x0804923c]> /x e89effffff
0x0804924c hit5_0 e89effffff
```

You can use `..` to leave out data. For example, the below command searches for a function call to `sym.read_in` with any offset.

```nasm
[0x0804923c]> /x e8..ffff
0x080490b7 hit8_0 e884ffff
0x08049183 hit8_1 e868ffff
0x0804924c hit8_2 e89effff
0xf7fad1bc hit8_3 e8bfffff
0xf7fae31a hit8_4 e8a1ffff
0xf7fae4ae hit8_5 e8fbffff
0xf7fb1986 hit8_6 e8fdffff
0xf7fb1fb3 hit8_7 e8feffff
0xf7fb2ca4 hit8_8 e8faffff
...
```

### Wide Strings

A **wide string** is any string where each character is more than one byte. This is most common in Windows binaries. We can use `/w` to search for wide strings. In most Linux binaries, wide-string searches won't return any results.

```nasm
[0x0804923c]> /w You lose
...
hits: 0
```

The actual string that got searched is `Y\0o\0u\0 \0l\0o\0s\0e\0` assuming it was encoded in UTF-16. This is why the search didn't return any results.

## Searching for Regex

The `/e` command is used to match regular expressions (regex). This is useful for finding patterns that are too complex for the other search commands. An important note is that in `radare2`, the regex is surrounded by `/`. The wildcard character is `.` instead of `*`.

```nasm
[0x0804923c]> /e /You lose/
0x0804b008 hit1_0 .You lose!cat flag.txtG.
0x0804a008 hit1_1 .You lose!cat flag.txtG.

[0x0804923c]> /e /You.../
0xf7fe9a94 hit3_0 .FOR-PROGRAM...]You have invoked 'ld.s.
0xf7fe9b58 hit3_1 .le is started.You may invoke the pro.
0x0804b008 hit3_2 .You lose!cat flag.txt.
0x0804a008 hit3_3 .You lose!cat flag.txt.
```

## Searching for ROP Gadgets

The `/R` submodule is used for searching for ROP gadgets. There are a few available modifiers:

```nasm
[0x0804923c]> /R?
Usage: /R  search for ROP gadgets (see "? for escaping chars in the shell)
| /R [string]     show gadgets
| /R/ [regexp]    show gadgets [regular expression]
| /R/j [regexp]   json output [regular expression]
| /R/q [regexp]   show gadgets in a quiet manner [regular expression]
| /Rj [string]    json output
| /Rk [ropklass]  query stored ROP gadgets klass
| /Rq [string]    show gadgets in a quiet manner
```

The most common use case is the first. This command searches the file for an inputted string and returns all ROP gadgets that contain that string.

```nasm
[0x0804923c]> /R pop esi
...
  0xf7fd119f               2c5b  sub al, 0x5b
  0xf7fd11a1                 5e  pop esi
  0xf7fd11a2                 5f  pop edi
  0xf7fd11a3                 5d  pop ebp
  0xf7fd11a4                 c3  ret

  0xf7fd11a0                 5b  pop ebx
  0xf7fd11a1                 5e  pop esi
  0xf7fd11a2                 5f  pop edi
  0xf7fd11a3                 5d  pop ebp
  0xf7fd11a4                 c3  ret

  0xf7fe111c                 5e  pop esi
  0xf7fe111d                 c3  ret
```

Because of the nature of the command, this method often prints a lot of garbage. `/Rq` prints the same information but in a more compact format.

```nasm
[0x0804923c]> /Rq pop esi
...
0xf7fd11a0: pop ebx; pop esi; pop edi; pop ebp; ret;
0xf7fdc1d4: add eax, 0x2200e43; pop esi; or cl, byte [esi]; adc al, 0x41; ret;
0xf7fdc1d7: and byte [edx], al; pop esi; or cl, byte [esi]; adc al, 0x41; ret;
0xf7fe011c: pop esi; ret;
0xf7fe111c: pop esi; ret;
```

I find that other gadget-finders are more useful than this one. However, it is still a useful tool to have.

## Searching for Patterns

The `/p` command allows you to search for patterns. This is useful for finding ROP gadgets, instructions, or other patterns. The `/p` command takes a number as an argument, which is the pattern size to search for.

```nasm
[0x0804923c]> /p 50
```

{% hint style="warning" %}
This prints **a ton** of output. It is recommended to pipe the output to a file or use the `grep` module to filter it.
{% endhint %}

\## The Help Page \`\`\`nasm \[0x7fc5e38fd290]> /? Usage: /\[!bf] \[arg] Search stuff (see 'e??search' for options) Use io.va for searching in non virtual addressing spaces | / foo\x00 search for string 'foo\0' | /j foo\x00 search for string 'foo\0' (json output) | /! ff search for first occurrence not matching, command modifier | /!x 00 inverse hexa search (find first byte != 0x00) | /+ /bin/sh construct the string with chunks | // repeat last search | /a\[?]\[1aoditfmsltf] jmp eax find instructions by text or bytes (asm/disasm) | /b\[?]\[p] search backwards, command modifier, followed by other command | /c\[?]\[adr] search for crypto materials | /d 101112 search for a deltified sequence of bytes | /e /E.F/i match regular expression | /E esil-expr offset matching given esil expressions \$$ = here | /f search forwards, (command modifier) | /F file \[off] \[sz] search contents of file with offset and size | /g\[g] \[from] find all graph paths A to B (/gg follow jumps, see search.count and anal.depth) | /h\[t] \[hash] \[len] find block matching this hash. See ph | /i foo search for string 'foo' ignoring case | /k foo search for string 'foo' using Rabin Karp alg | /m\[?]\[ebm] magicfile search for magic, filesystems or binary headers | /o \[n] show offset of n instructions backward | /O \[n] same as /o, but with a different fallback if anal cannot be used | /p\[?]\[p] patternsize search for pattern of given size | /P patternsize search similar blocks | /s\[\*] \[threshold] find sections by grouping blocks with similar entropy | /r\[erwx]\[?] sym.printf analyze opcode reference an offset (/re for esil) | /R\[?] \[grepopcode] search for matching ROP gadgets, semicolon-separated | /v\[1248] value look for an \`cfg.bigendian\` 32bit value | /V\[1248] min max look for an \`cfg.bigendian\` 32bit value in range | /w foo search for wide string 'f\0o\0o\0' | /wi foo search for wide string ignoring case 'f\0o\0o\0' | /x ff..33 search for hex string ignoring some nibbles | /x ff0033 search for hex string | /x ff43:ffd0 search for hexpair with mask | /z min max search for strings of given size | /\* \[comment string] add multiline comment, end it with '\*/' \`\`\`
