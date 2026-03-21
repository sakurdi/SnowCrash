# SnowCrash - Level08

## Objective

Find the password for the next level by logging into the `flag08` account.

## Steps

### 1. Locate files needed for the challenge

```bash
level08@SnowCrash:~$ ls
level08  token
```

We're giving a x86 binary and a file called token, probably containing the flag08's password.

Let's inspect the binary in GDB

```bash
(gdb) 
18	in level08.c
1: x/30i $eip
=> 0x80485a6 <main+82>:	mov    eax,DWORD PTR [esp+0x1c]
   0x80485aa <main+86>:	add    eax,0x4
   0x80485ad <main+89>:	mov    eax,DWORD PTR [eax]
   0x80485af <main+91>:	mov    DWORD PTR [esp+0x4],0x8048793
   0x80485b7 <main+99>:	mov    DWORD PTR [esp],eax
   0x80485ba <main+102>:	call   0x8048400 <strstr@plt>
   0x80485bf <main+107>:	test   eax,eax
   0x80485c1 <main+109>:	je     0x80485e9 <main+149>
   0x80485c3 <main+111>:	mov    eax,DWORD PTR [esp+0x1c]
   0x80485c7 <main+115>:	add    eax,0x4
   0x80485ca <main+118>:	mov    edx,DWORD PTR [eax]
   0x80485cc <main+120>:	mov    eax,0x8048799
   0x80485d1 <main+125>:	mov    DWORD PTR [esp+0x4],edx
   0x80485d5 <main+129>:	mov    DWORD PTR [esp],eax
   0x80485d8 <main+132>:	call   0x8048420 <printf@plt>
   0x80485dd <main+137>:	mov    DWORD PTR [esp],0x1
   0x80485e4 <main+144>:	call   0x8048460 <exit@plt>
   0x80485e9 <main+149>:	mov    eax,DWORD PTR [esp+0x1c]
   0x80485ed <main+153>:	add    eax,0x4
   0x80485f0 <main+156>:	mov    eax,DWORD PTR [eax]
   0x80485f2 <main+158>:	mov    DWORD PTR [esp+0x4],0x0
   0x80485fa <main+166>:	mov    DWORD PTR [esp],eax
   0x80485fd <main+169>:	call   0x8048470 <open@plt>
   0x8048602 <main+174>:	mov    DWORD PTR [esp+0x24],eax
   0x8048606 <main+178>:	cmp    DWORD PTR [esp+0x24],0xffffffff
   0x804860b <main+183>:	jne    0x804862e <main+218>
   0x804860d <main+185>:	mov    eax,DWORD PTR [esp+0x1c]
   0x8048611 <main+189>:	add    eax,0x4
   0x8048614 <main+192>:	mov    eax,DWORD PTR [eax]
   0x8048616 <main+194>:	mov    DWORD PTR [esp+0x8],eax
```
After inspecting it a bit we can see that it checks for a substring in the program's argument name, so if you pass the token file to it the check fails. We have to find a way to provide the exact file under a different name, we dont have write permissions on the folder so renaming it doesnt work, sounds like symbolic links would be perfect for the job :)


```bash
level08@SnowCrash:~$ ln -s /home/user/level08/token /tmp/test
level08@SnowCrash:~$ ./level08 /tmp/test
quif5eloekouj29ke0vouxean
```

Done :)