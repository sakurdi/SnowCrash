# SnowCrash - Level00

## Objective

Find the password for the next level by logging into the `flag02` account.

## Steps

### 1. Locate files needed for the challenge
```bash
ls 
```

File is unreadable as is, we'll use tcpflow to format it into something we can analyze
```bash
tcpflow -r level02.pcap -o out -a
```

### 2. Inspect the file content

Then we'll use xxd to inspect the unencrypted file's content
```bash
xxd 059.233.235.218.39247-059.233.235.223.12121

00000000: fffc 25ff fe26 fffb 18ff fb20 fffb 23ff  ..%..&..... ..#.
00000010: fb27 fffc 24ff fa20 0033 3834 3030 2c33  .'..$.. .38400,3
00000020: 3834 3030 fff0 fffa 2300 536f 6461 4361  8400....#.SodaCa
00000030: 6e3a 30ff f0ff fa27 0000 4449 5350 4c41  n:0....'..DISPLA
00000040: 5901 536f 6461 4361 6e3a 30ff f0ff fa18  Y.SodaCan:0.....
00000050: 0078 7465 726d fff0 fffd 03ff fc01 fffb  .xterm..........
00000060: 22ff fa22 0301 0000 0362 0304 020f 0500  "..".....b......
00000070: 0007 621c 0802 0409 421a 0a02 7f0b 0215  ..b.....B.......
00000080: 0f02 1110 0213 1102 ffff 1202 ffff fff0  ................
00000090: fffb 1fff fa1f 00b1 0031 fff0 fffd 05ff  .........1......
000000a0: fb21 fffa 2201 07ff f0ff fd01 fffb 00ff  .!.."...........
000000b0: fc22 6c65 7665 6c58 0d66 745f 7761 6e64  ."levelX.ft_wand
000000c0: 727f 7f7f 4e44 5265 6c7f 4c30 4c0d       r...NDRel.L0L.

```

Inspecting the characters this is clearly the communication using an unknown protocol between a client and a server, we can deduce this when the login? "levelX" is followed by a 0d character which is enter, naturally what follows is the password, there were some tricks as the 7f character (DEL) is present but reconstructing the password's was still fairly easy


