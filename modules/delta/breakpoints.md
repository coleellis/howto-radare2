# Breakpoints and Watchpoints

Breakpoints and watchpoints are used to pause execution at a specific point in the program. This is useful for debugging and reverse engineering. `radare2` has a number of commands for managing breakpoints and watchpoints.

## Using Breakpoints

The `db` submodule is responsible for breakpoints. This module allows you to create, delete, and manage breakpoints. We can use `db?` to see the available commands.

You set breakpoints using `db` plus the address or symbol. The argument can also be a symbol plus an offset, which `radare2` will resolve to its proper address. Here are some examples of setting breakpoints:

```nasm
[0xf7f8d8a0]> db main
[0xf7f8d8a0]> db sym.read_in+8
[0xf7f8d8a0]> db 0x080491e2
```

Use `db` to view the breakpoint list. You can use `dbi` to list the breakpoints by index; however, this only shows their address, not their name.

```nasm
0x0804923c - 0x0804923d 1 --x sw break enabled valid cmd="" cond="" name="main" module="/home/joybuzzer/args"
0x080491f7 - 0x080491f8 1 --x sw break enabled valid cmd="" cond="" name="sym.read_in+8" module="/home/joybuzzer/args"
0x080491e2 - 0x080491e3 1 --x sw break enabled valid cmd="" cond="" name="0x080491e2" module="/home/joybuzzer/args"
```

To remove a breakpoint, use `db -<name>` or `db -<address>`. You can use `dbi -<index>` to remove a breakpoint by its index.

```nasm
[0xf7f8d8a0]> db -main
[0xf7f8d8a0]> dbi -2
```

Use the `dbe` and `dbd` commands to enable and disable breakpoints, respectively. The `dbie` and `dbid` commands work the same way, using indices for their arguments.

```nasm
[0xf7f8d8a0]> dbe main
[0xf7f8d8a0]> dbd main
[0xf7f8d8a0]> dbie 1
[0xf7f8d8a0]> dbid 1
```

## Setting Watchpoints

Watchpoints are breakpoints that are triggered when a specific memory address is accessed. This is useful for detecting when a variable is changed. We can use `dbw` to set a watchpoint. This takes two arguments: the address to watch and the watch flags (read, write, or both).

```nasm
[0xf7f8d8a0]> dbw 0x0804a000 rw
```

The list of watchpoints is stored in `db` with the breakpoints.