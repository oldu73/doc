# Linux - 01 - Misc

***

## Curl

```console
curl -i localhost
curl -sb -H "Accept: application/json" "http://localhost" | json_pp
```

***

## Compress Decompress

### Compress (czf)

\- c compress, -z zip, -f file

```console
tar -czf /targetfolder/targetfile.tar.gz /sourcefolder
```

### Decompress (xzf)

\- x extract, -z zip, -f file

```console
tar -xzf targetfile.tar.gz
```

or gzip, -k to keep original, -d to decompress:

```console
gzip -kd file.gz
```

***

## Package

### List installed

```console
apt list --installed
```

### Info

```console
apt-cache show packagename
```

### Remove

```console
sudo apt-get --purge autoremove packagename
```

***

## Switch user to root

Switch current user to root

```console
sudo su -
```

***

## Grep lines before after match

-B before  
-A after

Stick ne lines just after option

```console
grep -B2 -A3 pattern infile.txt
```

***

## Copy files from list

Copy specific files from a text list of files

```console
rsync -a sourcefolder --files-from=list.txt destinationfolder
```

***

## Get data between two patterns

In error.log

```text
.
.
05:59:30.024 [nioEventLoopGroup-3-5] ERROR c.l.d.c.ConnectorServerHandlerTCP.parseAndSendMessageTeltonika(177) - class java.util.concurrent.ExecutionException FOR RAW DATA : 0000017d55747fb00003f5380c1be045ce00000000000000000804ef005000c8024503034230fc430f8d440000011007e0fca600 WITH STACKTRACE : {}
.
.
```

Get data between "DATA : " and " WITH" (hex raw data)

```console
cat error.log | sed -nr 's/.*DATA : (.*) WITH.*/\1/p'
```

or

```console
cat file | grep -o -P '(?<=left).*(?=right)'
```

***

## Grep patterns from a file

-f option, maybe -oF options also

```console
grep -f patterns_file *.log

or

grep -oFf patterns.txt *.log
```

If result's count's not OK, check by not found pattern (-h option to hide filename in output)

```console
grep -hoFf patterns.txt *.log | grep -vFf - patterns.txt
```

***

## For list of file

For list of file in current folder, do operation.
Here we want to have file name, cat content and separate result with a new line

```console
ll *.txt

1.txt
2.txt
3.txt
4.txt
5.txt


for f in {2..4}.txt; do echo "$f"; cat "$f"; printf "\n"; done

2.txt
jkl
mno
pqr

3.txt
stu
vwx
yza

4.txt
bcd
efg
hij
```

***

## Search between timestamp

```console
sed -rne '/10:50/,/11:05/ p' file
```

Put existing time range in file (10:50 - 11:05).

***

## Highlight search result

```console
grep --color=always -z pattern file | less -R
```

always to transmit color through pipe  
-z to show everything, not only the matching pattern  
-R to avoid showing esc char instead of color

***

## Delete history

```console
history
1003  25-04-2016 17:54:52 echo "Command 1"
1004  25-04-2016 17:54:54 echo "Command 2"
1005  25-04-2016 17:54:57 echo "Command 3"
1006  25-04-2016 17:54:59 echo "Command 4"
1007  25-04-2016 17:55:01 echo "Command 5"
1008  25-04-2016 17:55:03 echo "Command 6"
1009  25-04-2016 17:55:07 echo "Command 7"
1010  25-04-2016 17:55:09 echo "Command 8"
1011  25-04-2016 17:55:11 echo "Command 9"
1012  25-04-2016 17:55:14 echo "Command 10"
```

```console
for h in $(seq 1006 1008); do history -d 1006; done
```

***

## Where is a program

How to know where reside a program, e.g. ls?

```console
which ls
/usr/bin/ls

env

env | grep PATH
```

***

## Difference of cmd output

```console
diff <(ls test1) <(ls test2)
```

Difference of sorted lists

```console
sort ok.txt > okSorted.txt

sort all.txt > allSorted.txt

diff --new-line-format="" --unchanged-line-format=""  allSorted.txt okSorted.txt
```

***

## Column

### Min

```console
cat ... | grep ... | sort -n -r | tail -n10
```

### Max

```console
cat ... | grep ... | sort -n -r | head -n10
```

### Average

test.txt (warning on empty lines (maybe at the end))

```text
1
3
7
```

```console
cat test.txt | awk '{ total += $1 } END { print total/NR }'
3.66667
```

### Median

test.txt (warning on empty lines (maybe at the end))

```text
1 
3 
7 
11
22
45
71
```

median.awk

```text
#/usr/bin/env awk
{
    count[NR] = $1;
}
END {
    if (NR % 2) {
        print count[(NR + 1) / 2];
    } else {
        print (count[(NR / 2)] + count[(NR / 2) + 1]) / 2.0;
    }
}
```

```console
cat test.txt | awk -f median.awk
11
```

Check equal number of values below and above median

```console
cat test.txt | awk '{if($1 < 11) print $1}' | wc -l
3
cat test.txt | awk '{if($1 > 11) print $1}' | wc -l
3
```

***

## Get nth column from file

Get nth column from file with field separated values

test.txt

```text
column 1 row 1;column 2 row 1;column 3 row 1
column 1 row 2;column 2 row 2;column 3 row 2
column 1 row 3;column 2 row 3;column 3 row 3
```

```console
cat test.txt | awk -F ';' '{print $2}'
column 2 row 1
column 2 row 2
column 2 row 3
```

### Conditional

```console
cat test.txt | awk -F ';' '{if($1 == "column 1 row 2") print $2}'
column 2 row 2
```

***

## List files

Sorted by sizes and human readable

```console
ll -S -h
```

***

## Uniq values in a file

Uniq values in a file, sorted and counted (first sort is mandatory)

test.txt

```text
1
71
3
7
22
1
11
22
45
71
22
```

```console
cat test.txt | sort | uniq -c
      2 1
      1 11
      3 22
      1 3
      1 45
      1 7
      2 71
```

sorted output

```console
cat test.txt | sort | uniq -c | sort
      1 11
      1 3
      1 45
      1 7
      2 1
      2 71
      3 22
```

revert sorted output

```console
cat test.txt | sort | uniq -c | sort -r
      3 22
      2 71
      2 1
      1 7
      1 45
      1 3
      1 11
```

***

## All file containing a pattern

List all file that contain a pattern in current folder:

Be aware to filtered out subfolder by precising some file name pattern (e.g. '\*.log', not only '\*')

```console
grep searchedString *.log
```

Output only file name that contain pattern with option -l (that is a lowercase L):

```console
grep -l searchedString *.log
```

***

## Grep end of line after match

[SRC](https://stackoverflow.com/questions/20567667/grep-command-to-add-end-line-after-every-match)

```console
cat error.log | grep -A 1 -B 1 --group-separator==============\\r\\n "not valid"
```

***

## Empty file

Empty File Content by Redirecting to Null:

```console
> access.log
```

***

## Disk usage

```console
df -h
```

Huge file:

```console
sudo du -xh / | grep -P "G\t"
```

***

## Last modification of a file

```console
date -r fileName
```

***

## Creation date of a file

Below process to find creation date of a test file

1. In a tmp directory create a test file  
2. Find inode of the file  
3. Find partition on which current folder belongs to  
4. Use file system debugger to find creation date

```console
mkdir tmp

cd tmp

touch test

ls -i
201769 test

df .
Filesystem     1K-blocks     Used Available Use% Mounted on
/dev/sdd       263174212 10339304 239396752   5% /

sudo debugfs -R 'stat <201769>' /dev/sdd
.
.
debugfs 1.45.5 (07-Jan-2020)
.
.
Filesystem     1K-blocks     Used Available Use% Mounted on
/dev/sdd       263174212 10339304 239396752   5% /
16:15:36 ✔ oldu:(main)~/git/doc/tmp$ sudo debugfs -R 'stat <201769>' /dev/sdd


Inode: 201769   Type: regular    Mode:  0644   Flags: 0x80000
Generation: 333662723    Version: 0x00000000:00000001
User:  1000   Group:  1000   Project:     0   Size: 0
File ACL: 0
Links: 1   Blockcount: 0
Fragment:  Address: 0    Number: 0    Size: 0
 ctime: 0x624da077:74d33a00 -- Wed Apr  6 16:15:19 2022
 atime: 0x624da077:74d33a00 -- Wed Apr  6 16:15:19 2022
 mtime: 0x624da077:74d33a00 -- Wed Apr  6 16:15:19 2022
crtime: 0x624da077:74d33a00 -- Wed Apr  6 16:15:19 2022
Size of extra inode fields: 32
Inode checksum: 0x38e4efd5
```

***

## Since when a process run

```console
ps -p pid -o etime
```

Then use this [online tool](https://www.timeanddate.com/date/timeduration.html?) to calculate time between above result and now.

***

## How many file descriptors opened by a process

1. disk usage  
2. find processName's process id (e.g. 28043)  
3. number of fd opened for pid  
4. max limit of opened fd for pid

```console
df -H
ps ax | grep processName
sudo ls /proc/28043/fd | wc -l
sudo grep "Max open files" /proc/28043/limits | awk '{ print $4; }'
```

***

## Copy filtered file list

Copy (for move replace below cp by mv) grep"ed" filtered files list to another folder.

```console
ls | grep pattern1 | grep pattern2 | ... | xargs cp -t /destinationFolder
```

***

## Add prefix to file names list

```console
for f in * ; do mv -- "$f" "PRE_$f" ; done
```

***

## Curly braces

Using braces to build a sequence.

[All about {Curly Braces} in Bash](https://www.linux.com/topic/desktop/all-about-curly-braces-bash/)

```console
echo {0..10}
0 1 2 3 4 5 6 7 8 9 10
```

***

## Network connection

```console
(sudo) netstat -tunlp
```

***

## Search replace

Search (s) and replace in place (-i) all occurrences (g) of each lines in a file:

```console
sed -i 's/SEARCH/REPLACE/g' filename
```

***
