# SnowCrash - Level01

## Objective

Find the password for the next level by logging into the `flag01` account.

## Steps

### 1. Search for files owned by `flag01`
```bash
find / -user flag01 2>/dev/null
```

This returns nothing.

So I tried my luck looking into /etc/passwd and I found this interesting line:
```bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
```

Looks like a hash, so naturally I had to find out what it was
```bash
hashid 42hDRfypTqqnw
Analyzing '42hDRfypTqqnw'
[+] DES(Unix) 
[+] Traditional DES 
[+] DEScrypt 
```

### 2. Inspect the file content

I then used hashcat to crack the password
```bash
hashcat -m 1500 -a 0 hash.txt ../../lists/Passwords/Software/bt4-password.txt

hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
====================================================================================================================================================
* Device #1: cpu-haswell-AMD Ryzen 5 5500U with Radeon Graphics, 6086/12237 MB (2048 MB allocatable), 12MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 8

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 3 MB

Dictionary cache built:
* Filename..: ../../lists/Passwords/Software/bt4-password.txt
* Passwords.: 1652903
* Bytes.....: 15663259
* Keyspace..: 1652903
* Runtime...: 0 secs

42hDRfypTqqnw:abcdefg                                     

```

There we go

### 3. Use the decoded result

Once decoded, use the resulting string as the password for the `flag01` user.