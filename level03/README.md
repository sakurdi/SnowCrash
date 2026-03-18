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