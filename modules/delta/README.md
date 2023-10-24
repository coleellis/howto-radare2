# Debugging: The d Module

The Debugging (`d`) Module is used to debug the binary. This module is used to set breakpoints, view registers, and run the binary.  This module drives our **dynamic analysis**. Dynamic analysis is actively running the binary and observing its behavior. This is done by watching the registers, the stack, and the instructions as they are executed.

In `radare2`, dynamic analysis is done in **debug mode**. We can enter debug mode by using the `-d` flag when opening a binary.

## The Help Page 
```nasm
[0x00000000]> d?
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