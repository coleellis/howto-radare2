# Analysis: The `a` Module

## Overview
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