```nasm
; bp (breakpoint)
; default breakpoint in ntdll
bp <address>
bp <symbol>

; bl (breakpoint list)
; lists breakpoint
bl

; bc (breakpoint clear)
; remove breakpoints
bc <breakpoint number>
bc 3

; bd (breakpoint disable)
; disables breakpoint
bc <breakoint number>

; be (breakpoint enable)
; enables breakpoint
be <breakpoint number>

; ba (break on access)
; PS: ba creates a hardware breakpoint
ba <access type> <data size> <address>
ba w 4 00000000deadbeef ; break when 4 bytes are written at 0x00000000deadbeef
ba r 8 00000000deadbeef ; break when 8 bytes are read or written at 0x00000000deadbeef
ba e 1 00000000deadbeef; break when 

bp $exentry ; sets breakpoint at entrypoint
bp binary!main ; sets breakpoint at main if symbols are available

; adding /1 to breakpoint commands will delete the breakpoint after hitting it once

; g (go)
; continue execution till next breakpoint or breakaddress
g 0x00000000deadbeef ; continue till address

; r (register)
; read and modify register
r rax, rbx, r8w ; prints the value of the register
r rax = 0xdeadbeef ; set register value

; d* (display memory)
d[b|d|q] <address> L<number> ; displays <number> amount of [bytes|dword|qword] at <address>
dq rsp L6
da <address> ; displays ascii string at <address>

; e* (edit memory)
e[b|d|q] <address> <value> ; edit <value> at <address>
eb rsp+0x4 0x11 0x22 0x33 0x44

; k (stack trace)
k

; p (step over)
p <optional number>
p 5 ; step over 5 instructions

; t (step into)
t <optional number>
t 6 ; step into 6 instructions

; gu (step out)
gu

; pa (step till address)
; prints all the instructions executed till address
pa <address|symbol>
pa 0xdeadbeef
pa function

; ta (trace to address)
; prints all instruction it step into till it reaches that address
ta <address|symbol>

; uf (unassemble function)
; prints disassembly of the specified functin
uf mybinary!main


; .restart
; restarts program execution
.restart
```