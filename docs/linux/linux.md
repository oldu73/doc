# Linux

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
