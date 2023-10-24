# Static Analysis: The `a` Module

The Analysis (`a`) Module is used to explore the contents of the binary. We use this to understand the contents of functions, function calls, cross-references, and more. This module drives our **static analysis**. Static analysis is analyzing the binary without running it. This is done by looking at the assembly instructions, the strings, and the symbols.

We will collect the following information using this module:
* Cross-References
* Functions and Calling Conventions
* Variables (Local and Global)

This will be the heart of our disassembly process. We will use this information to understand the binary and the underlying source code.

## Pre-Analysis
If you don't use flags to open the file, you will need to tell `radare2` to analyze the binary. There are multiple levels of analysis listed under the `aa` submodule:

```nasm
[0x0804923c]> aaa?
Usage: aa[a[a[a]]]   # automatically analyze the whole program
| a      show code analysis statistics
| aa     alias for 'af@@ sym.*;af@entry0;afva'
| aaa    perform deeper analysis, most common use
| aaaa   same as aaa but adds a bunch of experimental iterations
| aaaaa  refine the analysis to find more functions after aaaa
```

In almost all scenarios, `aaa` is more than plenty. By using the `-A` flag, `aaa` is automatically executed.

## The Help Page

```nasm
[0x00000000]> a?
Usage: a  [abdefFghoprxstc] [...]
| a                alias for aai - analysis information
| a:[cmd]          run a command implemented by an analysis plugin (like : for io)
| a*               same as afl*;ah*;ax*
| aa[?]            analyze all (fcns + bbs) (aa0 to avoid sub renaming)
| a8 [hexpairs]    analyze bytes
| ab[?]            analyze basic block
| ac[?]            manage classes
| aC[?]            analyze function call
| ad[?]            analyze data trampoline (wip) (see 'aod' to describe mnemonics)
| ad [from] [to]   analyze data pointers to (from-to)
| ae[?] [expr]     analyze opcode eval expression (see ao)
| af[?]            analyze functions
| aF               same as above, but using anal.depth=1
| ag[?] [options]  draw graphs in various formats
| ah[?]            analysis hints (force opcode size, ...)
| ai [addr]        address information (show perms, stack, heap, ...)
| aj               same as a* but in json (aflj)
| aL[jq]           list all asm/anal plugins (See `e asm.arch=?` and `La[jq]`)
| an[?] [name]     show/rename/create whatever var/flag/function used in current instruction
| ao[?] [len]      analyze Opcodes (or emulate it)
| aO[?] [len]      analyze N instructions in M bytes
| ap               find prelude for current offset
| ar[?]            like 'dr' but for the esil vm. (registers)
| as[?] [num]      analyze syscall using dbg.reg
| av[?] [.]        show vtables
| avg[?] [.]       manage global variables
| ax[?]            manage refs/xrefs (see also afx?)
```