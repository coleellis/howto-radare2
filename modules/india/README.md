# Information: The i Module

The Information (`i`) Module is responsible for displaying information about the binary. We make extensive use of the information module when doing **static analysis**.

We will collect the following information using this module:
* Binary Compilation Information
* Imports and Exports
* Security Protections
* Symbols and Strings

These pieces are instrumental to understanding the binary, the underlying source code, and protections in place.

## Outside Radare2

The Information Module has identical functionality to the `rabin2` library.  More information on `rabin2` can be found [here](../../suite/rabin2.md).

## The Help Page
```nasm
[0x00000000]> i?
Usage: i  Get info from opened file (see rabin2's manpage)
Output mode:
| '*'                output in radare commands
| 'j'                output in json
| 'q'                simple quiet output
Actions:
| i|ij               show info of current file (in JSON)
| iA                 list archs
| ia                 show all info (imports, exports, sections..)
| ib                 reload the current buffer for setting of the bin (use once only)
| ic                 List classes, methods and fields (icj for json)
| ic.                show class and method name in current seek
| ic-[klass.method]  delete given klass or klass.name
| ic+[klass.method]  add new symbol in current seek for a given klass and method name
| icc                List classes, methods and fields in Header Format
| icg [str]          List classes as agn/age commands to create class hierarchy graphs (matches str if provided)
| icq                List classes, in quiet mode (just the classname)
| icqq               List classes, in quieter mode (only show non-system classnames)
| icl                Show addresses of class and it methods, without names
| ics                Show class symbols in an easy to parse format
| iC[j]              show signature info (entitlements, ...)
| id[?]              show DWARF source lines information
| iD lang sym        demangle symbolname for given language
| ie                 entrypoint
| iee                show Entry and Exit (preinit, init and fini)
| iE                 exports (global symbols)
| iE,[table-query]   exported symbols using the table query
| iE.                current export
| ih                 headers (alias for iH)
| iHH                verbose Headers in raw text
| ii[?][cj*,]        imports
| iI                 binary info
| ik [query]         key-value database from RBinObject
| il                 libraries
| iL [plugin]        list all RBin plugins loaded or plugin details
| im                 show info about predefined memory allocation
| iM                 show main address
| io [file]          load info from file (or last opened) use bin.baddr
| iO[?]              perform binary operation (dump, resize, change sections, ...)
| ir                 list the Relocations
| iR                 list the Resources
| is                 list the Symbols
| is,[table-query]   list symbols in table using given expression
| is.                current symbol
| iS [entropy,sha1]  sections (choose which hash algorithm to use)
| iS.                current section
| iS,[table-query]   list sections in table using given expression
| iS=                show ascii-art color bars with the section ranges
| iSS                list memory segments (maps with om)
| it                 file hashes
| iT                 file signature
| iV                 display file version info
| iw                 show try/catch blocks
| iX                 display source files used (via dwarf)
| iz[?][j]           strings in data sections (in JSON/Base64)
| iz*                print flags and comments r2 commands for all the strings
| izz                search for Strings in the whole binary
| izz*               same as iz* but exposing the strings of the whole binary
| izzz               dump Strings from whole binary to r2 shell (for huge files)
| iz- [addr]         purge string via bin.str.purge
| iZ                 guess size of binary program
```