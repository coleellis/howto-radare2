# Running the Binary

Upon opening `radare2`, the binary is already running. `radare2` defaults to the entry point of the binary (listed in `rabin2`'s output) and puts a temporary breakpoint at that location. This is often inconvenient for us since we don't care much about this compiler-generated code. We can set our own breakpoint at the `main` function and then continue execution there.

Use `dc` to continue execution to the next breakpoint.

```nasm
[0xf7f8d8a0]> db main
[0xf7f8d8a0]> dc
INFO: hit breakpoint at: 0x804923c
```

## Returning to the Instruction Pointer

As we are navigating the output, scanning through instructions and the stack, we might lose the place of the instruction pointer. An important note in `radare2` is that **the seek address is not the same as the instruction pointer**. This causes confusion with new users as they attempt to understand where they are in the binary.

The best way to return to the instruction pointer is to use `s eip`. If you are in visual mode, the `.` command also returns the display to the instruction pointer.

{% hint style="info" %}
More information on visual mode can be found [here](modes/visual-mode/).
{% endhint %}

## Stepping

There are two kinds of steps when using a debugger: _stepping in_ and _stepping over_. Stepping commands are outlined in the `ds` submodule.

* **Step In**: This method steps into any called functions and pauses at the first instruction. This allows you to walk through called functions.
* **Step Over**: This method steps over a function and immediately executes all its contents. This is useful for library functions where their instructions aren't important.

To step in, use the `ds` instruction. To step over, use the `dso` instruction. You can use `ds <num>` or `dso <num>` to step `<num>` instructions.

{% hint style="danger" %}
These instructions are different in visual mode. In visual mode, `s` is for stepping in and `S` is for stepping over.
{% endhint %}

## Continuing

You can use the `dc` command to continue execution until the next breakpoint or watchpoint is hit.

```nasm
[0xf7f8d8a0]> dc
```

You can use the `dcu` command to continue execution until a specified address is reached.

```nasm
[0xf7f8d8a0]> dcu 0x08049210
INFO: Continue until 0x08049210 using 1 bpsize
Good luck winning here!
INFO: hit breakpoint at: 0x8049210
```

You can use the `dcr` command to continue performing step-over instructions until the next `ret` instruction is reached.

```nasm
[0xf7f8d8a0]> dcr
```

{% hint style="info" %}
This does not fully replicate the `finish` command in `gdb`. This command goes until the _next_ `ret` instruction rather than the current function's `ret`.

The best way to handle this is to set a breakpoint at the `ret` instruction and then continue execution, or use the `dcu` command.
{% endhint %}

## Stack Traces

A **stack trace** is a list of all the functions that have been called up to this point. This is useful for debugging and understanding the flow of the program.

To view the stack trace, use the `dbt` command.

```nasm
[0x080491f3]> dbt
0  0x8049210  sp: 0x0         0    [sym.read_in]  eip sym.read_in+33
```

As far as I know, `radare2` does not provide support for moving up and down the stack trace. If this is not the case, please let me know, and I'll update this section!

## Running Backwards

You can run programs in reverse order to better understand how to reach a certain location in a binary. Radare2 has a reverse debugger that can seek the program counter backward.

To do this, you must save the program state. This is handled via the `dts` submodule (the _Trace Sessions_ submodule). Use `dts+` to start a trace session.

```nasm
[0x080491ef]> dts+
INFO: Reading 4096 byte(s) from 0x0804c000
INFO: Reading 4096 byte(s) from 0xf7e2a000
INFO: Reading 40960 byte(s) from 0xf7e2b000
INFO: Reading 8192 byte(s) from 0xf7faf000
INFO: Reading 4096 byte(s) from 0xf7fee000
INFO: Reading 135168 byte(s) from 0xffa57000
```

Use `dsb` to restore the previously recorded state by reverse-executing the instructions.

```nasm
[0xf7fb5549]> dsb
[0x080491fb]> 
```

Use `dcb` to continue reverse-executing instructions until the next breakpoint.

```nasm
[0xf7cb5683]> dcb
[0x080491fb]> 
```

## Debugging Forks

When a binary forks, it creates a new process. This is useful for creating child processes that can run in parallel. However, this can be a pain for debugging because you have to debug each process individually.

This can be done in `radare2` by deciding at the time of the fork. Use `dcf` to continue execution until the next fork. Once here, consult the `dp` submodule to handle processing commands.

The `dp` submodule is used to list and manage processes. Use `dp` to list the current processes. Use `dp <num>` to switch to a process. Use `dp-` to kill the current process.
