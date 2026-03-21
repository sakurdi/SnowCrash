# SnowCrash - Level09

## Objective

Find the password for the next level by logging into the `flag09` account.

## Steps

### 1. Locate files needed for the challenge

```bash
level09@SnowCrash:~$ ls
level09  token
```

We're giving a x86 binary and a file called token just like the last challenge.

Let's inspect the binary in binary ninja this time.

```bash
080487ce    int32_t main(int32_t argc, char** argv, char** envp)

080487ce  55                 push    ebp {__saved_ebp}
080487cf  89e5               mov     ebp, esp {__saved_ebp}
080487d1  57                 push    edi {__saved_edi}
080487d2  53                 push    ebx {__saved_ebx}
080487d3  83e4f0             and     esp, 0xfffffff0
080487d6  81ec30010000       sub     esp, 0x130
080487dc  8b450c             mov     eax, dword [ebp+0xc {argv}]
080487df  8944241c           mov     dword [esp+0x1c {var_124}], eax
080487e3  65a114000000       mov     eax, dword [gs:0x14]
080487e9  8984242c010000     mov     dword [esp+0x12c {var_14}], eax
080487f0  31c0               xor     eax, eax  {0x0}
080487f2  c744242400000000   mov     dword [esp+0x24 {var_11c}], 0x0
080487fa  c7442420ffffffff   mov     dword [esp+0x20 {i_1}], 0xffffffff  {0xffffffff}
08048802  c744240c00000000   mov     dword [esp+0xc {var_134}], 0x0
0804880a  c744240801000000   mov     dword [esp+0x8 {var_138}], 0x1
08048812  c744240400000000   mov     dword [esp+0x4 {var_13c}], 0x0
0804881a  c7042400000000     mov     dword [esp {var_140}], 0x0
08048821  e8bafcffff         call    ptrace
08048826  85c0               test    eax, eax
08048828  7916               jns     0x8048840

0804882a  c70424708b0408     mov     dword [esp {var_140}], data_8048b70  {"You should not reverse this"}
08048831  e84afcffff         call    puts
08048836  b801000000         mov     eax, 0x1
0804883b  e937020000         jmp     0x8048a77

08048840  c704248c8b0408     mov     dword [esp {var_140}], data_8048b8c  {"LD_PRELOAD"}
08048847  e824fcffff         call    getenv
0804884c  85c0               test    eax, eax
0804884e  7432               je      0x8048882

08048850  a140a00408         mov     eax, dword [stderr]
08048855  89c2               mov     edx, eax
08048857  b8988b0408         mov     eax, data_8048b98  {"Injection Linked lib detected exit..\n"}
0804885c  8954240c           mov     dword [esp+0xc {var_134_1}], edx
08048860  c744240825000000   mov     dword [esp+0x8 {var_138}], 0x25
08048868  c744240401000000   mov     dword [esp+0x4 {var_13c}], 0x1
08048870  890424             mov     dword [esp {var_140}], eax  {data_8048b98, "Injection Linked lib detected exit..\n"}
08048873  e8e8fbffff         call    fwrite
08048878  b801000000         mov     eax, 0x1
0804887d  e9f5010000         jmp     0x8048a77

08048882  c744240400000000   mov     dword [esp+0x4 {var_13c}], 0x0
0804888a  c70424be8b0408     mov     dword [esp {var_140}], data_8048bbe  {"/etc/ld.so.preload"}
08048891  e80afcffff         call    open
08048896  85c0               test    eax, eax
08048898  7e32               jle     0x80488cc

0804889a  a140a00408         mov     eax, dword [stderr]
0804889f  89c2               mov     edx, eax
080488a1  b8988b0408         mov     eax, data_8048b98  {"Injection Linked lib detected exit..\n"}
080488a6  8954240c           mov     dword [esp+0xc {var_134_2}], edx
080488aa  c744240825000000   mov     dword [esp+0x8 {var_138}], 0x25
080488b2  c744240401000000   mov     dword [esp+0x4 {var_13c}], 0x1
080488ba  890424             mov     dword [esp {var_140}], eax  {data_8048b98, "Injection Linked lib detected exit..\n"}
080488bd  e89efbffff         call    fwrite
080488c2  b801000000         mov     eax, 0x1
080488c7  e9ab010000         jmp     0x8048a77

080488cc  c744240400000000   mov     dword [esp+0x4 {var_13c}], 0x0
080488d4  c70424d18b0408     mov     dword [esp {var_140}], data_8048bd1  {"/proc/self/maps"}
080488db  e8c4fcffff         call    syscall_open
080488e0  89442428           mov     dword [esp+0x28 {var_118_1}], eax
080488e4  837c2428ff         cmp     dword [esp+0x28 {var_118_1}], 0xffffffff
080488e9  0f8561010000       jne     0x8048a50

080488ef  a140a00408         mov     eax, dword [stderr]
080488f4  89c2               mov     edx, eax
080488f6  b8e48b0408         mov     eax, data_8048be4  {"/proc/self/maps is unaccessible, probably a LD_PRELOAD attempt exit..\n"}
080488fb  8954240c           mov     dword [esp+0xc {var_134_3}], edx
080488ff  c744240846000000   mov     dword [esp+0x8 {var_138}], 0x46
08048907  c744240401000000   mov     dword [esp+0x4 {var_13c}], 0x1
0804890f  890424             mov     dword [esp {var_140}], eax  {data_8048be4, "/proc/self/maps is unaccessible, probably a LD_PRELOAD attempt exit..\n"}
08048912  e849fbffff         call    fwrite
08048917  b801000000         mov     eax, 0x1
0804891c  e956010000         jmp     0x8048a77

08048921  c74424042b8c0408   mov     dword [esp+0x4 {var_13c}], data_8048c2b  {"libc"}
08048929  8d44242c           lea     eax, [esp+0x2c {var_114}]
0804892d  890424             mov     dword [esp {var_140_1}], eax {var_114}
08048930  e896fdffff         call    isLib
08048935  85c0               test    eax, eax
08048937  740d               je      0x8048946

08048939  c744242401000000   mov     dword [esp+0x24 {var_11c}], 0x1
08048941  e90b010000         jmp     0x8048a51

08048946  837c242400         cmp     dword [esp+0x24 {var_11c}], 0x0
0804894b  0f8400010000       je      0x8048a51

08048951  c7442404308c0408   mov     dword [esp+0x4 {var_13c}], 0x8048c30
08048959  8d44242c           lea     eax, [esp+0x2c {var_114}]
0804895d  890424             mov     dword [esp {var_140_2}], eax {var_114}
08048960  e866fdffff         call    isLib
08048965  85c0               test    eax, eax
08048967  0f84a1000000       je      0x8048a0e

0804896d  837d0802           cmp     dword [ebp+0x8 {argc}], 0x2
08048971  7571               jne     0x80489e4

08048973  eb21               jmp     0x8048996

08048975  8b44241c           mov     eax, dword [esp+0x1c {var_124}]
08048979  83c004             add     eax, 0x4
0804897c  8b10               mov     edx, dword [eax]
0804897e  8b442420           mov     eax, dword [esp+0x20 {i_1}]
08048982  01d0               add     eax, edx
08048984  0fb600             movzx   eax, byte [eax]
08048987  0fbec0             movsx   eax, al
0804898a  03442420           add     eax, dword [esp+0x20 {i_1}]
0804898e  890424             mov     dword [esp {var_140_3}], eax
08048991  e82afbffff         call    putchar

08048996  8344242001         add     dword [esp+0x20 {i_1}], 0x1
0804899b  8b5c2420           mov     ebx, dword [esp+0x20 {i_1}]
0804899f  8b44241c           mov     eax, dword [esp+0x1c {var_124}]
080489a3  83c004             add     eax, 0x4
080489a6  8b00               mov     eax, dword [eax]
080489a8  c7442418ffffffff   mov     dword [esp+0x18 {var_128}], 0xffffffff  {0xffffffff}
080489b0  89c2               mov     edx, eax
080489b2  b800000000         mov     eax, 0x0
080489b7  8b4c2418           mov     ecx, dword [esp+0x18 {var_128}]  {0xffffffff}
080489bb  89d7               mov     edi, edx
080489bd  f2ae               repne scasb byte [edi]  {0x0}
080489bf  89c8               mov     eax, ecx
080489c1  f7d0               not     eax
080489c3  83e801             sub     eax, 0x1
080489c6  39c3               cmp     ebx, eax
080489c8  72ab               jb      0x8048975

080489ca  a160a00408         mov     eax, dword [stdout]
080489cf  89442404           mov     dword [esp+0x4 {var_13c_1}], eax
080489d3  c704240a000000     mov     dword [esp {var_140}], 0xa
080489da  e8f1faffff         call    fputc
080489df  e991000000         jmp     0x8048a75

080489e4  a140a00408         mov     eax, dword [stderr]
080489e9  89c2               mov     edx, eax
080489eb  b8348c0408         mov     eax, data_8048c34  {"You need to provied only one arg.\n"}
080489f0  8954240c           mov     dword [esp+0xc {var_134_4}], edx
080489f4  c744240822000000   mov     dword [esp+0x8 {var_138}], 0x22
080489fc  c744240401000000   mov     dword [esp+0x4 {var_13c}], 0x1
08048a04  890424             mov     dword [esp {var_140}], eax  {data_8048c34, "You need to provied only one arg.\n"}
08048a07  e854faffff         call    fwrite
08048a0c  eb67               jmp     0x8048a75

08048a0e  c7442404578c0408   mov     dword [esp+0x4 {var_13c}], data_8048c57  {"00000000 00:00 0"}
08048a16  8d44242c           lea     eax, [esp+0x2c {var_114}]
08048a1a  890424             mov     dword [esp {var_140_4}], eax {var_114}
08048a1d  e824fcffff         call    afterSubstr
08048a22  85c0               test    eax, eax
08048a24  752b               jne     0x8048a51

08048a26  a140a00408         mov     eax, dword [stderr]
08048a2b  89c2               mov     edx, eax
08048a2d  b8688c0408         mov     eax, data_8048c68  {"LD_PRELOAD detected through memory maps exit ..\n"}
08048a32  8954240c           mov     dword [esp+0xc {var_134_5}], edx
08048a36  c744240830000000   mov     dword [esp+0x8 {var_138}], 0x30
08048a3e  c744240401000000   mov     dword [esp+0x4 {var_13c}], 0x1
08048a46  890424             mov     dword [esp {var_140}], eax  {data_8048c68, "LD_PRELOAD detected through memory maps exit ..\n"}
08048a49  e812faffff         call    fwrite
08048a4e  eb25               jmp     0x8048a75

```

After spending quite a bit of time looking at the anti debug techniques using ptrace, and itself checking for untampered libc reading its own /proc/self/maps you start to figure out that its just bait meant to confuse you. I tried looking at an open syscall on the token file but it never does that, instead it encodes data you feed into it if you look closer

```bash
08048975  8b44241c           mov     eax, dword [esp+0x1c {var_124}]
08048979  83c004             add     eax, 0x4
0804897c  8b10               mov     edx, dword [eax]
0804897e  8b442420           mov     eax, dword [esp+0x20 {i_1}]
08048982  01d0               add     eax, edx
08048984  0fb600             movzx   eax, byte [eax]
08048987  0fbec0             movsx   eax, al
0804898a  03442420           add     eax, dword [esp+0x20 {i_1}]
0804898e  890424             mov     dword [esp {var_140_3}], eax
08048991  e82afbffff         call    putchar
```

It simply adds add its own index to the char at av[1][i] while iterating over it, let's try decoding it with a simple loop, substracting its index to each byte in the token file.

The loop logic would look like this 

```bash
for (i = 0; i < len; i++) {
    res[i] = token[i] - i;
}
```

Now if we pass the decoded result as input we get exactly what's in the token file, and we can use it to log into the flag09 account :)

```bash
level09@SnowCrash:~$ ./level09 "f3iji1ju5yuevaus41q1afiuq"
f4kmm6p|=�p�n��DB�Du{��
level09@SnowCrash:~$ su flag09
Password: 
Don't forget to launch getflag !
flag09@SnowCrash:~$ getflag
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z
```
