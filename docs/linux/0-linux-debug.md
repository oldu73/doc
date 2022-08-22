# Linux - 00 - Debug

***

## Toolbox

[20 one-line Linux commands to add to your toolbox](https://www.redhat.com/sysadmin/one-line-linux-commands)

```console
df // report file system disk space usage
free // Display amount of free and used memory in the system
top // display Linux processes
netstat // Print network connections
(sudo) netstat -tunlp // opened port
cat /var/log/run.log
systemctl status metricbeat // Control the systemd system and service manager
journalctl -f -u metricbeat // Query the systemd journal
```

***

## Quick live log monitor

### Monitor

E.g. for "success" pattern presence in log file.

**1st terminal** watch (120s) grep 'success' in 'file.log' and output in 'logsuccess' file:

```console
watch -t -n 120 '({ date; echo "-"; cat file.log | grep success | wc -l; } | tr "\n" " " ; printf "\n") | tee -a logsuccess'
```

When done, remove 'logsuccess' file:

```console
rm logsuccess
```

**2nd terminal** tailed watch (120s) of 'logsuccess' file:

```console
watch -n 120 -d 'cat logsuccess | tail -n $(($LINES - 2))'
```

### Monitor of monitor

Prerequisite, PID of current bash session:

```console
echo $$
```

#### Monitor of "success" monitor

**3rd terminal** live top PID:

```console
top -p <pid1>,<pid2>
```

#### Monitor of Monitor of "success" monitor

**4th terminal** log top PID:

```console
top | grep -E '<pid1>|<pid2>'
```

#### Monitor of Monitor of Monitor of "success" monitor

**5th terminal** live top processes resource consumption:

```console
top | grep top
```

***

## Log repartition

To see repartition in log file of a matching pattern

```console
less + /<searched pattern>
```

***

## Terminal ring

For long command, terminal ring when finished:

```console
<long command>; echo -e "\a"
```

***

## Grep binary file

Grep binary file, or supposed to be (some text files are considered as):

```console
grep -a <pattern> file
```

Add -a argument to 'grep' command for processing a binary file as if it were text.

***

## Log between time range

For logs like in file.log:

```txt
185.98.28.113: - - [16/Jun/2022:07:30:00 +0000] "GET /...
```

Logs from 07:30 to 07:40:

```console
grep -E '(16/Jun/2022:07:3)' file.log
```

***

## URL by time

Statisitic of requested URL by time (round to minutes), for logs like in file.log:

```txt
82.146.192.163:- - - [03/Nov/2021:06:25:21 +0000] "GET /base/services/report/type_2/data...
```

Command, for timestamp catch pattern from first '[' until last ':' (trunc seconds), then catch type of report in URL, from 'report/' until none of \[a-zA-Z0-9_.\], hoping it is a '/':

```console
cat file.log | sed -n 's/^.*\[\(.*\):[^:].*\/base\/services\/report\/\([a-zA-Z0-9_.]*\)\(.*\)/\1 -> \2/p' | sort | uniq -c | sort -n -r | head -20
```

Output:

```console
5 31/May/2022:07:36 -> type_1
5 31/May/2022:07:24 -> type_2
5 31/May/2022:07:18 -> type_2
5 31/May/2022:06:20 -> type_2
5 31/May/2022:05:21 -> type_2
5 31/May/2022:04:55 -> type_2
4 31/May/2022:20:20 -> type_2
4 31/May/2022:18:41 -> type_2
4 31/May/2022:12:45 -> type_3
4 31/May/2022:12:42 -> type_2
4 31/May/2022:12:35 -> type_3
4 31/May/2022:11:57 -> type_2
```

***

## List of requested base URL

file.log:

```txt
82.146.192.163:- - - [03/Nov/2021:06:25:21 +0000] "GET /base/services/report/type_2/data...
```

Command:

```console
grep -a -o -P '(?<=\/base\/services\/report\/).*' file.log | cut -d/ -f1 | sort | uniq
```

Output:

```txt
type_1
type_2
type_3
```

***

## Count by minute of base URL

file.log:

```txt
82.146.192.163:- - - [03/Nov/2021:06:25:21 +0000] "GET /base/services/report/type_2/data...
```

Command:

```console
grep -a /base/services/report/ file.log | grep -oP '\[.*\]' | cut -c2- | rev | cut -c11- | rev | sort | uniq -c | sort -n -r | head
```

Output:

```txt
11 31/May/2022:12:48
07 31/May/2022:12:45
06 31/May/2022:12:44
05 31/May/2022:12:42
05 31/May/2022:12:35
05 31/May/2022:11:57
05 31/May/2022:07:36
05 31/May/2022:07:24
05 31/May/2022:07:18
05 31/May/2022:06:20
```

***

## Monitor live file activity

Monitor live file activity (sorted by size) in a folder:

```console
watch -d 'ls -laS'
```

***

## Filter java stack trace in log

### Quick

Remove "at" lines:

```console
grep -v '^.*at' debug.log | less
```

### Long

Remove other line that contain (recurring) pattern:

```console
cat debug.log | grep -v -E '^.*at|pattern1|pattern2|pattern3' | less
```

***

## tail in WSL (2)

By default tail in WSL on a Windows handled file does not follow.

To make it works:

```console
tail -f ---disable-inotify file
```

***
