# Introduction

This section covers the basics of using `radare2`: finding the help pages for each module, the basic command format for each command, and using expressions.

## Help Commands

I don't think there exists anyone that knows every `radare2` command by heart. Thankfully, `radare2` has a well-built help system.

The question mark command (`?`) is used for accessing the help pages.

This can be combined with the modules to provide information on the available submodules and the commands inside each module. For example, if we want information on the debugger module (`d`), we can use `d?` to access the help page for the debugger module:

```nasm
[0xf7f518a0]> d?
Usage: d   # Debug commands
| d:[?] [cmd]              run custom debug plugin command
| db[?]                    breakpoints commands
| dbt[?]                   display backtrace based on dbg.btdepth and dbg.btalgo
| dc[?]                    continue execution
| dd[?][*+-tsdfrw]         manage file descriptors for child process
| de[-sc] [perm] [rm] [e]  debug with ESIL (see de?)
| dg <file>                generate a core-file (WIP)
| dh [plugin-name]         select a new debug handler plugin (see dbh)
| dH [handler]             transplant process to a new handler
| di[?]                    show debugger backend information (See dh)
| dk[?]                    list, send, get, set, signal handlers of child
| dL[?]                    list or set debugger handler
| dm[?]                    show memory maps
| do[?]                    open process (reload, alias for 'oo')
| doo[args]                reopen in debug mode with args (alias for 'ood')
| doof[file]               reopen in debug mode from file (alias for 'oodf')
| doc                      close debug session
| dp[?]                    list, attach to process or thread id
| dr[?]                    cpu registers
| ds[?]                    step, over, source line
| dt[?]                    display instruction traces
| dw <pid>                 block prompt until pid dies
| dx[?][aers]              execute code in the child process
| date [-b]                use -b for beat time
```

## Basic Command Format

Commands in `radare2` are organized into **modules**. The official documentation marks the format of a `radare2` command as:

```
Usage: [.][times][cmd][~grep][@[@iter]addr!size][|>pipe] ; ...

Append '?' to any char command to get detailed help
Prefix with number to repeat command N times (f.ex: 3x)
```

Here are some examples of some commands that are commonly used:

```nasm
ds                      ; debugger module -- step in
pd 1                    ; print 1 line of disassembly
pxw 4 @ esp             ; print 4 bytes of hex at esp
is~OBJ                  ; equiv. is | grep OBJ -- find variables
px 20 ; pd 3 ; px 40    ; multiple commands in one line
```

Most commands offer autocompletion using the `Tab` key. To extend autocompletion, use the `!!!` submodule command.

When using the `grep` command, you can `grep` for either rows or columns. Use `:` for rows and `[]` for columns.

```nasm
[0xf7f148a0]> pd 1          ; output for pd 1 (for reference)
            ;-- eip:
            0xf7f148a0      89e0           mov eax, esp
[0xf7f148a0]> pd 1~mov[0]   ; get first column
0xf7f148a0
[0xf7f148a0]> pd 1~mov:0    ; get first row
            0xf7f148a0      89e0           mov eax, esp
```

## Expressions: The ? Module

Expressions are prepended with the `?` command. This module handles the evaluation of expressions and other miscellaneous features. We will cover the most common uses of the `?` module.

{% hint style="info" %}
To access the help pages for the `?` module, use `???`.
{% endhint %}

### Evaluating Expressions

The `?v` submodule is responsible for viewing the output of expressions. There are three formats for viewing the output of expressions:

* `?v` - View the output of the expression in hexadecimal.
* `?vi` - View the output of the expression in decimal.
* `?vx` - View the output of the expression in 8-byte padded hexadecimal.

To understand the outputs, let's look at the same expression for each output:

```nasm
[0xf7f148a0]> ?v 1+2
0x3
[0xf7f148a0]> ?vi 1+2
3
[0xf7f148a0]> ?vx 1+2
0x00000003
```

The supported arithmetic operations are addition (`+`), subtraction (`-`), multiplication (`*`), division (`/`), modulus (`%`), shifting (`<<` and `>>`), and bitwise operations (`&`, `|`, and `^`).

{% hint style="warning" %}
#### Binary OR

To use binary OR, quote the entire expression to avoid executing the `|` pipe operator.

```nasm
[0xf7f148a0]> "?vi 1 | 2"
3
```
{% endhint %}

You can use the raw `?` operator to get the output in several forms.

```nasm
[0xf7f148a0]> ? 1+2
int32   3
uint32  3
hex     0x3
octal   03
unit    3
segment 0000:0003
string  "\x03"
fvalue  3.0
float   0.000000f
double  0.000000
binary  0b00000011
base36  0_3
ternary 0t10
```

### What-Is

`?w` is used for determining what is at an address. We can use this to determine if the address is an instruction or data and the permissions of that address.

```nasm
[0xf7f148a0]> ?w main
/home/joybuzzer/args .text main,main main program R X 'push ebp' 'args'
[0xf7f148a0]> ?w esp
[stack] esp stack R W X 'add dword [eax], eax' '[stack]'
```

## Shell Commands: The ! Module

You can use shell commands inside `radare2` using the `!` module. The `!` command is used to execute commands.

```nasm
[0xf7f148a0]> !cat flag.txt
cat: flag.txt: No such file or directory
[0xf7f148a0]> !date
Mon Oct 23 06:28:20 PM EDT 2023
```

`grep` can be used in combination with shell commands. However, two exclamations must be used.

```nasm
[0xf7f148a0]> !!ls -l~README
-rw-rw-r-- 1 joybuzzer joybuzzer 1890 Oct 23 12:57 README.md
```
