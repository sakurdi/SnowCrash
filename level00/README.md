# SnowCrash - Level00

## Objective

Find the password for the next level by logging into the `flag00` account.

## Steps

### 1. Search for files owned by `flag00`
```bash
find / -user flag00 2>/dev/null
```

This returns:
```
/usr/sbin/john
/rofs/usr/sbin/john
```

### 2. Inspect the file content

Check the content of `/usr/sbin/john`:
```bash
cat /usr/sbin/john
```

You should see an encoded string.

### 3. Identify the encoding

The string looks like a Caesar cipher (letters shifted by a fixed amount).

### 4. Decode the string

Try rotating the letters by different values (0–25) until the result becomes readable English. Or, as the name implies use a pre-made cracking tool like john the ripper

Example approach (manual or script):

- Shift each letter forward in the alphabet
- Test different shift values

### 5. Use the decoded result

Once decoded, use the resulting string as the password for the `flag00` user.