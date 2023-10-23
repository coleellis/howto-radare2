# Functions

### Listing Functions

Use the `afl` command to list the available functions. The `afll` command provides the list of available commands in verbose mode.

{% tabs %}
{% tab title="afl" %}
```nasm
[0x0804923c]> afl
0x08049040    1      6 sym.imp.__libc_start_main
0x08049050    1      6 sym.imp.fflush
0x08049060    1      6 sym.imp.gets
0x08049070    1      6 sym.imp.puts
0x08049080    1      6 sym.imp.system
0x08049090    1     44 entry0
0x080490bd    1      4 fcn.080490bd
0x080490f0    4     40 sym.deregister_tm_clones
0x08049130    4     53 sym.register_tm_clones
0x08049170    3     34 sym.__do_global_dtors_aux
0x080491a0    1      6 sym.frame_dummy
0x080490e0    1      4 sym.__x86.get_pc_thunk.bx
0x08049258    1     24 sym._fini
0x080491a6    4     73 sym.win
0x080490d0    1      5 sym._dl_relocate_static_pie
0x0804923c    1     24 main
0x08049254    1      4 sym.__x86.get_pc_thunk.ax
0x080491ef    1     77 sym.read_in
0x08049000    3     36 sym._init
```
{% endtab %}

{% tab title="afll" %}
```nasm
[0x0804923c]> afll
address    noret size  nbbs edges    cc cost  min bound range max bound  calls locals args xref frame name
========== ===== ===== ===== ===== ===== ==== ========== ===== ========== ===== ====== ==== ==== ===== ====
0x08049040     1    6     1     0     1    3 0x08049040     6 0x08049046     0    0      0    1     0 sym.imp.__libc_start_main
0x08049050     0    6     1     0     1    3 0x08049050     6 0x08049056     0    0      0    1     0 sym.imp.fflush
0x08049060     0    6     1     0     1    3 0x08049060     6 0x08049066     0    0      0    1     0 sym.imp.gets
0x08049070     0    6     1     0     1    3 0x08049070     6 0x08049076     0    0      0    2     0 sym.imp.puts
0x08049080     0    6     1     0     1    3 0x08049080     6 0x08049086     0    0      0    1     0 sym.imp.system
0x08049090     1   44     1     0     1   21 0x08049090    44 0x080490bc     2    0      0    0    28 entry0
0x080490bd     0    4     1     0     1    4 0x080490bd     4 0x080490c1     0    0      0    1     0 fcn.080490bd
0x080490f0     0   40     4     4     4   23 0x080490f0    49 0x08049121     0    0      0    1    28 sym.deregister_tm_clones
0x08049130     0   53     4     4     4   29 0x08049130    57 0x08049169     0    0      0    0    28 sym.register_tm_clones
0x08049170     0   34     3     2     3   18 0x08049170    41 0x08049199     1    0      0    0    12 sym.__do_global_dtors_aux
0x080491a0     0    6     1     0     1    3 0x080491a0     6 0x080491a6     0    0      0    0     0 sym.frame_dummy
0x080490e0     0    4     1     0     1    4 0x080490e0     4 0x080490e4     0    0      0    3     0 sym.__x86.get_pc_thunk.bx
0x08049258     0   24     1     0     1   12 0x08049258    24 0x08049270     1    0      0    0    12 sym._fini
0x080491a6     0   73     4     4     2   34 0x080491a6    73 0x080491ef     3    1      1    0    28 sym.win
0x080490d0     0    5     1     0     1    4 0x080490d0     5 0x080490d5     0    0      0    0     0 sym._dl_relocate_static_pie
0x0804923c     0   24     1     0     1   15 0x0804923c    24 0x08049254     2    0      0    2     4 main
0x08049254     0    4     1     0     1    4 0x08049254     4 0x08049258     0    0      0    2     0 sym.__x86.get_pc_thunk.ax
0x080491ef     0   77     1     0     1   36 0x080491ef    77 0x0804923c     4    2      0    1    76 sym.read_in
0x08049000     0   36     3     3     2   19 0x08049000    36 0x08049024     1    0      0    0    12 sym._init
```
{% endtab %}
{% endtabs %}

You can use the `aflm` command to list the commands based on the function they're called in.

```nasm
[0x0804923c]> aflm
entry0:
    fcn.080490bd
    sym.imp.__libc_start_main

sym.__do_global_dtors_aux:
    sym.deregister_tm_clones

sym._fini:
    sym.__x86.get_pc_thunk.bx

sym.win:
    sym.__x86.get_pc_thunk.ax
    sym.imp.puts
    sym.imp.system

main:
    sym.__x86.get_pc_thunk.ax
    sym.read_in

sym.read_in:
    sym.__x86.get_pc_thunk.bx
    sym.imp.puts
    sym.imp.fflush
    sym.imp.gets

sym._init:
    sym.__x86.get_pc_thunk.bx
```