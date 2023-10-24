# Printing: The p Module

The Printing (`p`) Module is responsible for printing memory and data. This module is used to print the disassembly, the hexdump, and the strings. This module is used extensively in both **static** and **dynamic** analysis.

We will use this section to print the contents of registers, the stack, and key addresses throughout execution.  We can control the print formatting to print as hex, decimal, or ASCII.

## The Help Page

```nasm
[0x00000000]> p?
Usage: p[=68abcdDfiImrstuxz] [arg|len] [@addr]
| p[b|B|xb] [len] ([S])   bindump N bits skipping S bytes
| p[iI][df] [len]         print N ops/bytes (f=func) (see pi? and pdi)
| p[kK] [len]             print key in randomart (K is for mosaic)
| p-[?][jh] [mode]        bar|json|histogram blocks (mode: e?search.in)
| p2 [len]                8x8 2bpp-tiles
| p3 [file]               print 3D stereogram image of current block
| p6[de] [len]            base64 decode/encode
| p8[?][dfjx] [len]       8bit hexpair list of bytes
| p=[?][bep] [N] [L] [b]  show entropy/printable chars/chars bars
| pa[edD] [arg]           pa:assemble  pa[dD]:disasm or pae: esil from hex
| pA[n_ops]               show n_ops address and type
| pb[?] [n]               bitstream of N bits
| pB[?] [n]               bitstream of N bytes
| pc[?][p] [len]          output C (or python) format
| pC[aAcdDxw] [rows]      print disassembly in columns (see hex.cols and pdi)
| pd[?] [sz] [a] [b]      disassemble N opcodes (pd) or N bytes (pD)
| pf[?][.name] [fmt]      print formatted data (pf.name, pf.name $<expr>)
| pF[?][apx]              print asn1, pkcs7 or x509
| pg[?][x y w h] [cmd]    create new visual gadget or print it (see pg? for details)
| ph[?][=|hash] ([len])   calculate hash for a block
| pi[?][bdefrj] [num]     print instructions
| pI[?][iI][df] [len]     print N instructions/bytes (f=func)
| pj[?] [len]             print as indented JSON
| pk [len]                print key in randomart mosaic
| pK [len]                print key in randomart mosaic
| pm[?] [magic]           print libmagic data (see pm? and /m?)
| po[?] hex               print operation applied to block (see po?)
| pp[?][sz] [len]         print patterns, see pp? for more help
| pq[?][is] [len]         print QR code with the first Nbytes
| pr[?][glx] [len]        print N raw bytes (in lines or hexblocks, 'g'unzip)
| ps[?][pwz] [len]        print pascal/wide/zero-terminated strings
| pt[?][dn] [len]         print different timestamps
| pu[w] [len]             print N url encoded bytes (w=wide)
| pv[?][ejh] [mode]       show value of given size (1, 2, 4, 8)
| pwd                     display current working directory
| px[?][owq] [len]        hexdump of N bytes (o=octal, w=32bit, q=64bit)
| py([-:file]) [expr]     print clipboard (yp) run python script (py:file) oneliner `py print(1)` or stdin slurp `py-`
| pz[?] [len]             print zoom view (see pz? for help)
| pkill [process-name]    kill all processes with the given name
| pushd [dir]             cd to dir and push current directory to stack
| popd[-a][-h]            pop dir off top of stack and cd to it
```