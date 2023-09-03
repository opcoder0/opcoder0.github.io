---
layout: post
author: opcoder0
---

## GDB vs. WINGDB Commands

I have always enjoyed working on Unix/Linux environments makes me feel at home. Off late I had to debug a crash on windows and used WinDbg for it. I liked WinDbg for the fact that it is command driven and is similar to GDB. So am tempted to compare and record the options available in the two debuggers here. This post, discusses only debugging tools available for user mode debugging and not kernel mode debuggers.

First and foremost, for any source level meaningful debugging to work at you should have compiled your code in debug mode. Use -g flag on Unix/Linux or generate PDB file on windows using compiler flag /ZI.

Procedure to create a dump (core-dump) file in Windows using WinDbg or analyzing a crash –

Execute the command below –

```
> cscript.exe adplus.vbs -crash -sc C:\path\to\your\executable.exe
```

**Note:** `adplus.vbs` resides under the WinDbg install location.

If your program crashes, the above command dumps core under your WinDbg install location under CrashDump folder. This can be a good way to examine the stack trace of a program if it crashes during startup.

Type the kb command in your windbg debugger command after executing the above command to view the stack trace of the crash or run the other windbg commands below to continue execution if the program didn’t crash during startup.

To dump core at a specific location in your program, type the command at your break point or line of your choice in windbg –

```
.dump /mfh C:\path\of\yourchoice\myfile.dmp
```

Here are a list of various useful commands that come in handy

| Command / Option description                   | GDB Command              | WinGDB Command / UI Op                                     |
|------------------------------------------------|--------------------------|------------------------------------------------------------|
| Enabling post-mortem default debug             | N/A                      | `windbg -I`; `drwtsn32 -i`                                 |
| Invoking Debugger with core file / dump core   | `gdb <exec> <core-file>` | `Windbg -y SymbolPath -i ImagePath -z DumpFileName <exe>`  |
| Attaching to a Running Process                 | `gdb <executable> <pid>` | `windbg -p <pid>`                                          |
| Repeat Last Command                            | Hit enter key            | Hit enter key                                              |
| Display debugger command(s) while performing GUI operations in the debugger | N/A | `.bpcmds` | 
| Enabling Source level debugging | `break <line>` `break <source>:line` `break funcname` `break class::func` | `bp @@masm(FileName:LineNumber+)` |
| Execute program to a breakpoint or end of execution | `run` or `r` | `go` or `g` |
| Step over to next instruction | `next` | `p` |
| Step into function / method   | `step` | `t` | 
| Display Variable Contents     | `print var_name` | `dv var_name` |
| Dump Memory Contents          | `dump` | `d``{a|b|c|d|D|f|p|q|u|w|W}`|
| Display Stack Frame           | `bt <number-of-stack-frame>` | `kc` or `k[b|p|P|v] [c] [n] [f] [L] [Frame Count]` |

More to follow on threads and other sub-commands.
