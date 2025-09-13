# loading binary
```bash
gdb --quiet binary
# or
gdb -q
(gdb) file binary

# quit
(gdb) q
```
# running programs
```bash
# run or rerun program
r <argumetns to program>
run <arguments to program>
start <arguments to program> # runs but with temporary breakpoint at entrypoint

# continue program
continue
c

# breakpoints
break <symbol|address>
b <symbol|address>

# list breakpoints
info breakpoint
info b
i b

# remove breakpoint
clear <*address|symbol>
cl <*address|symbol>
delete <breakpoint number>
d <breakpoint number>

# disable/enable breakpoint
disable <breakpoint number>
dis <breakpoint number>
enable <breapoint number>
en <breapoint number>

# list source code (if compiled with source code)
list # prints source code at current location
list <function> # prints function source code
list <source file>:<line number> 

# disassemble code
disassemble <address|symbol>
disas <address|symbol>
# flags
/r # print raw bytes of instructions
/s or /m # print source code if available

# backtrace
backtrace
bt

# step over
# (next instruction)
netxi
ni

# step into
stepi
si

# step out
# (finish)
finish
fin

# run until
until <address> # or ultil the end of function if address is never reached
u <address>

# call
# call function directly from gdb
call (void)myfunction()

# jump
jump <location>
j <location>
j main+45
# the above can be replicated with set command
set $rip = main+45

```
# examine memory & registers
```bash
# format spcider /FMT
/nfu
# n = count

# f = format
x = hexadecimal
d = signed decimal
u = unsigned decimal
c = ascii characters (char)
s = ascii string (null terminated)
i = instruction
o = octal
t = binary
a = address

# u = unit size
b = byte
h = half word (2 bytes) # word in intel/windows
w = word (4 bytes) # double word in intel/windows
g = giant word (8 bytes) # quad word in intel/windows

/10xd # mean 10 word value in hex form.

# examine memory
x/FMT <address|symbol|$register>
x/15xb main # prints 15 byte values at main in hex form.
x/s 0xdeadbeef # print one ascii scring at address 0xdeadbeef  (unit not applicable here)
x/10i main # print 10 instruction at main (unit not applicable here)
x/10i $rip # prints 10 instructions from current instruction pointer

# examine register
# using info
info registers <optional registers>
info r <optional registers>
i r <optional registers>
i r rax, rbx, r8b

# using print
print/FMT <expression> # only format applicable for /FRT
p/FMT <expression> 
p/x $rbx # print value in rbx in hex
p/u $rcx # print value in rcx as unsigned integer
p/u $rcx+0x01 # print value in rcx as unsigned integer and add 0x01
print *(long *) $rsp # print will cast rsp as a long pointer, and dereference it to print the data at rsp

# using display
display/FMT <expression>
# this will print expression everytime the program stops
# if size letter is specified in /FMT, then <expression> is treated as address
display/xg $rsp # (size) prints value at address in $rsp
display/x $rsp # (no size) prints value of $rsp
```
# modifying registers & memory
### set
```bash
# modify register
set <$register> = <value>
set $eax = 0xdeadbeef

# modify memory
set {data type}<address|$register> = <value>
set {long}$rsp = 0xdeadbeef
# if the vaue is shorted that the size of the data type, the value will be zero extended, and not sign extended
set {long long}$rsp = 0xff
x/xg $rsp
0x7fffffffe180: 0x00000000000000ff
# set can also use the c-style set syntax to achieve the same effect
set *(long long *) $rsp = 0xff # treats rsp as a pointer to long long then dereferences to place a new value.

# we can also set custom variables using set
set $myvar = $rsp
# we can reference this later using print or examine
print/xg *(long *) $myvar
# registers are also technically variables hence preceded with $ symbol
# set can also use c-cast notation to perform more complex actions
set *(long *) $rsp = 0xdeadbeef # will dereference address in rsp and place 0xdeadbeef there

```
# gdb config
```bash
# configuration can be loaded with -x option
gdb -x config.cfg -q binary
# gdb scripts can have conditional hooks
b main
commands
	p/a $rip
	continue
end
# this will run the command block every time the breakpoint is hit

# gdb settings
set disassembly-flavor intel # change syntax to intel
```
