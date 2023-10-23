# Debugging: The `d` Module

This section is known as **dynamic analysis**. Dynamic analysis is actively running the binary and observing its behavior. This is done by watching the registers, the stack, and the instructions as they are executed.

In `radare2`, dynamic analysis is done in **debug mode**. We can enter debug mode by using the `-d` flag when opening a binary. Debug commands are contained in the _Debugging_ (`d`) module. We can use `d?` to see the available commands.