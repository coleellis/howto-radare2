# Checking Security

You can use `iI` to get the summary information about the binary, which includes the security features.

{% tabs %}
{% tab title="iI" %}
```nasm
[0xf7f3c8a0]> iI
arch     x86
baddr    0x8048000
binsz    13864
bintype  elf
bits     32
canary   false
injprot  false
class    ELF32
compiler GCC: (Ubuntu 11.3.0-1ubuntu1~22.04.1) 11.3.0
crypto   false
endian   little
havecode true
intrp    /lib/ld-linux.so.2
laddr    0x0
lang     c
linenum  true
lsyms    true
machine  Intel 80386
nx       false
os       linux
pic      false
relocs   true
relro    partial
rpath    NONE
sanitize false
static   false
stripped false
subsys   linux
va       true
```
{% endtab %}

{% tab title="rabin2" %}
```nasm
$ rabin2 -I args
arch     x86
baddr    0x8048000
binsz    13864
bintype  elf
bits     32
canary   false
injprot  false
class    ELF32
compiler GCC: (Ubuntu 11.3.0-1ubuntu1~22.04.1) 11.3.0
crypto   false
endian   little
havecode true
intrp    /lib/ld-linux.so.2
laddr    0x0
lang     c
linenum  true
lsyms    true
machine  Intel 80386
nx       false
os       linux
pic      false
relocs   true
relro    partial
rpath    NONE
sanitize false
static   false
stripped false
subsys   linux
va       true
```
{% endtab %}
{% endtabs %}

The most important security features listed here are:

* `canary`
* `pic` (and `baddr` if PIE is enabled)
* `nx`
* `relro`
* `lang`, `endian`, `arch`, `bits`, and `os`

Radare2 supports using pipes. Therefore, you can pipe the output to `grep` to summarize the output.

```nasm
$ rabin2 -I args | grep -E "canary|pic|nx|relro"
canary   false
nx       false
pic      false
relro    partial
```
