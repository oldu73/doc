# Linux

***

## Package

### List installed
```
$ apt list --installed
```

### Info
```
$ apt-cache show packagename
```

### Remove
```
$ sudo apt-get --purge autoremove packagename
```

***

## Switch user to root

Switch current user to root
```
$ sudo su -
```

***

## Grep lines before after match

-B before  
-A after

Stick ne lines just after option

```
$ grep -B2 -A3 pattern infile.txt
```

***

## Copy files from list

Copy specific files from a text list of files

```
$ rsync -a sourcefolder --files-from=list.txt destinationfolder
```

***

## Get data between two patterns

In error.log
```
.
.
05:59:30.024 [nioEventLoopGroup-3-5] ERROR c.l.d.c.ConnectorServerHandlerTCP.parseAndSendMessageTeltonika(177) - class java.util.concurrent.ExecutionException FOR RAW DATA : 0000017d55747fb00003f5380c1be045ce00000000000000000804ef005000c8024503034230fc430f8d440000011007e0fca600 WITH STACKTRACE : {}
.
.
```

Get data between "DATA : " and " WITH" (hex raw data)
```
$ cat error.log | sed -nr 's/.*DATA : (.*) WITH.*/\1/p'
```

***

## Grep patterns from a file

-f option, maybe -oF options also

```
$ grep -f patterns_file *.log

or

$ grep -oFf patterns.txt *.log
```

If result's count's not OK, check by not found pattern (-h option to hide filename in output)

```
$ grep -hoFf patterns.txt *.log | grep -vFf - patterns.txt
```

***

## For list of file

For list of file in current folder, do operation.
Here we want to have file name, cat content and separate result with a new line

```
$ ll *.txt

1.txt
2.txt
3.txt
4.txt
5.txt


$ for f in {2..4}.txt; do echo "$f"; cat "$f"; printf "\n"; done

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

```
$ sed -rne '/10:50/,/11:05/ p' file
```

Put existing time range in file (10:50 - 11:05).

***

## Highlight search result

```
$ grep --color=always -z pattern file | less -R
```

always to transmit color through pipe  
-z to show everything, not only the matching pattern  
-R to avoid showing esc char instead of color

***

## Delete history

```
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

```
$ for h in $(seq 1006 1008); do history -d 1006; done
```

***

## Where is a program

How to know where reside a program, e.g. ls?

```
$ which ls
/usr/bin/ls

$ env

$ env | grep PATH
```

***

## Difference

Difference of 2 commands output

```
$ diff <(ls test1) <(ls test2)
```

Difference of sorted lists

```
$ sort ok.txt > okSorted.txt

$ sort all.txt > allSorted.txt

$ diff --new-line-format="" --unchanged-line-format=""  allSorted.txt okSorted.txt
```

***

## Mean of a column

test.txt (warning on empty lines (maybe at the end))
```
1
3
7
```

```
$ cat test.txt | awk '{ total += $1 } END { print total/NR }'
3.66667
```

***

## Median of a column

test.txt (warning on empty lines (maybe at the end))
```
1 
3 
7 
11
22
45
71
```

median.awk
```
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

```
$ cat test.txt | awk -f median.awk
11
```

Check equal number of values below and above median

```
$ cat test.txt | awk '{if($1 < 11) print $1}' | wc -l
3
$ cat test.txt | awk '{if($1 > 11) print $1}' | wc -l
3
```

***

## Get nth column from file

Get nth column from file with field separated values

test.txt
```
column 1 row 1;column 2 row 1;column 3 row 1
column 1 row 2;column 2 row 2;column 3 row 2
column 1 row 3;column 2 row 3;column 3 row 3
```

```
$ cat test.txt | awk -F ';' '{print $2}'
column 2 row 1
column 2 row 2
column 2 row 3
```

### Conditional

```
$ cat test.txt | awk -F ';' '{if($1 == "column 1 row 2") print $2}'
column 2 row 2
```

***

## List files

Sorted by sizes and human readable
```
$ ll -S -h
```

***

## Uniq values in a file

Uniq values in a file, sorted and counted (first sort is mandatory)

test.txt
```
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

```
$ cat test.txt | sort | uniq -c
      2 1
      1 11
      3 22
      1 3
      1 45
      1 7
      2 71
```

sorted output

```
$ cat test.txt | sort | uniq -c | sort
      1 11
      1 3
      1 45
      1 7
      2 1
      2 71
      3 22
```

revert sorted output

```
$ cat test.txt | sort | uniq -c | sort -r
      3 22
      2 71
      2 1
      1 7
      1 45
      1 3
      1 11
```

***
