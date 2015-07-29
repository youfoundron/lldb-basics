#LLDB Basics
A basic overview of lldb for personal reference.  
Official documentation can be found here <a href="http://lldb.llvm.org/">here</a>.

##Command Structure
General syntax  
```
<noun> <verb> [-options [option-value]] [argument [argument...]]
```  
  
Options can be placed anywhere but if an argument begins with '-', you have to signifiy an option termination with '--'  
```
<noun> <verb> [-options [option-value]] -- [argument [argument...]]
```

##Loading in a Program
Specify the file you wish to debug on start  
```
$ lldb /nilisp.c
```  
  
Or after with the file command  
```
(lldb) file /nilisp.c
```  
  
##Starting or Attaching to Your Program  
To launch a program use the "process launch" command or one of its aliases
```
(lldb) process launch
(lldb) run
(lldb) r
```  
  
You can also attach to a process by pid or name  
```
(lldb) process attach --pid 123
(lldb) process attach -p 123
(lldb) process attach --name Nihilisp
```  
  
The 'waitfor' option attaches debugging to the next process with given name  
```
(lldb) process attach --name Nihilisp --waitfor
```

##Breakpoints##
To see all the options for breakpoint setting  
```
(lldb) help breakpoint
```  

**List** out breakpoints you've set  
```
(lldb) breakpoint list
(lldb) br l
```

**Delete** a breakpoint  
```
(lldb) breakpoint delete 1
(lldb) br del 1
```

Set a breakpoint on a **file** and **line**  
```
(lldb) breakpoint set --file nilisp.c --line 666
(lldb) breakpoint set -f nilisp.c -l 666
```

Set a breakpoint on a **function** named 'do_stuff'
```
(lldb) breakpoint set --name do_stuff
(lldb) breakpoint set -n do_stuff
```

Set a breakpoint on a **set of functions**  
```
(lldb) set --name do_stuff --name do_more_stuff
```

Set a breakpoint on all **methods** named 'do_stuff'  
```
(lldb) set --method do_stuff
(lldb) set -M do_stuff
```

Add a **command** on a breakpoint  
```
(lldb) breakpoint command add 1.1
Enter your debugger command(s). Type 'DONE' to end.
> bt // prints a backtrace when we hit a breakpoint
> DONE
```

Commands by default take <a href="http://lldb.llvm.org/lldb-gdb.html">lldb command line commands</a>  
Use **--script** if you want to implement a command using Python instead  
  
To add a **set of commands** to a breakpoint
```
breakpoint command add <cmd-options> <breakpt-id>
```

##Watchpoints
To see all the options for watchpoint setting  
```
(lldb) help watchpoint
```

**List** out watchpoints you've set  
```
(lldb) watchpoint list
(lldb) watch l
```

**Delete** a watchpoint  
```
(lldb) watchpoint delete 1
(lldb) watch del 1
```

Set a watchpoint on a **variable** when it is written to  
```
(lldb) watchpoint set variable my_variable
(lldb) wa s v my_variable
```

Set a **condition** on a watchpoint  
```
(lldb) watchpoint modify -c '(my_variable==)'
```

##Control
Continue
```
(lldb) process continue
```

Program stepping  
```
(lldb) thread step-in    // The same as gdb's "step" or "s"
(lldb) thread step-over  // The same as gdb's "next" or "n"
(lldb) thread step-out   // The same as gdb's "finish" or "f
```

##Frame State

Show the **arguments and local variables** for the current frame  
```
(lldb) frame variable
(lldb) fr v
```

Show **just local variables**  
```
(lldb) frame variable --no-args
(lldb) fr v -a
```

Show the **contents of local variable** 'my_variable'  
```
(lldb) frame variable my_variable
(lldb) fr v my_variable
(lldb) p my_variable
```

Show the **contents of global variable** 'my_global_variable'  
```
(lldb) target variable my_global_variable
(lldb) ta v my_global_variable
```

##Aliases
General syntax  
```
command alias <alias_name> <noun> <verb> [-options [option-value]] [symbolic-argument [argument...]]
```

Example  
```
(lldb) breakpoint set --file foo.c --line 12   // Let's shorten this
(lldb) command alias bfl breakpoint set -f %1 -l %2
(lldb) bfl foo.c 12
```

To get rid of an alias  
```
(lldb) command unalias <alias_name>
```
