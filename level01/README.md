```bash
level01@snowcrash:~$ cat README
Gargoyles never stop collecting.
```

```bash
level01@snowcrash:~$ /opt/snowcrash/level01/gargoyle 
Floating point exception (core dumped)
```

```bash
level01@snowcrash:~$ ./gargoyle test
level01@snowcrash:~$
```
When running the binary through pwngdb, it was clear that the argument was the encryption token for the program and then it would `daemonize` (run in background).

```bash
level01@snowcrash:~$ ps aux | grep gargoyle
flag01       691  0.0  0.0   2684  1220 ?        S    16:01   0:00 /opt/snowcrash/level01/gargoyle kooda7puivaav1idi4f57q8iq0
level01     4119  0.0  0.0   2684  1228 ?        S    16:44   0:00 ./gargoyle test
level01     4122  0.0  0.1   6544  2280 pts/0    S+   16:44   0:00 grep --color=auto gargoyle
```
