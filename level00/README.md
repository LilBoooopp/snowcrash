```bash
level00@snowcrash:~$ ls -la README
-r--r--r-- 1 level00 level00 66 Jun  9 19:03 README
level00@snowcrash:~$ cat README
Hiro Protagonist. Last of the freelance hackers. Or so he thinks.
```

```bash
level00@snowcrash:~$ find / -type f -perm -4000 2>/dev/null
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/mount
/usr/bin/passwd
/usr/bin/sudo
/usr/bin/umount
/usr/bin/chsh
/usr/bin/gpasswd
/usr/bin/fusermount3
/usr/bin/su
/usr/lib/polkit-1/polkit-agent-helper-1
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/opt/snowcrash/level06/raven
/opt/snowcrash/level07/snowcrash
/opt/snowcrash/level09/da5id
/opt/snowcrash/level10/enzo_runner
/opt/snowcrash/level00/hiro
```
**/opt/snowcrash/level00/hiro**

```bash
level00@snowcrash:~$ cp /opt/snowcrash/level00/hiro ./
level00@snowcrash:~$ strings hiro
```
Reveals hardcoded responses to linux commands, e.g. `env`:
```bash
SHELL=/bin/bash
HOME=/home/flag00
USER=flag00
LOGNAME=flag00
PATH=/usr/local/bin:/usr/bin:/bin
TERM=xterm-256color
```

```bash
level00@snowcrash:~$ strings hiro | grep flag
flag00
flag00@snowcrash:~$ 
 welcome, flag00
uid=3000(flag00) gid=3000(flag00) groups=3000(flag00)
flag00
/home/flag00
HOME=/home/flag00
USER=flag00
LOGNAME=flag00
drwxr-x--- 2 flag00 flag00 4096 Jan  1 00:00 .
-r-------- 1 flag00 flag00   32 Jan  1 00:00 .flag
-rwsr-x--- 1 flag00 level00  12288 Jan  1 00:00 hiro
flag_data
g_shutdown_flag
.flag_data
```

```bash
level00@snowcrash:~$ readelf -S hiro
...
  [19] .flag_data        PROGBITS         0000000000004640  00004640
       000000000000001b  0000000000000000   A       0     0     16
...
```

```bash
level00@snowcrash:~$ readelf -x .flag_data hiro

Hex dump of section '.flag_data':
  0x00004640 66326176 35696c30 32707561 6e6f376e f2av5il02puano7n
  0x00004650 61616636 61646166 336100            aaf6adaf3a.
```
f2av5il02puano7naaf6adaf3a

