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
