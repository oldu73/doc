# Linux - 01 - Misc

---

## Curl

```console
curl -i localhost
curl -sb -H "Accept: application/json" "http://localhost" | json_pp
```

---

## Compress Decompress

### Compress (czf)

\- c compress, -z zip, -f file

```console
tar -czf /targetfolder/targetfile.tar.gz /sourcefolder
```

or gzip:

```console
gzip <file name>
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

---

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

---

## Switch user to root

Switch current user to root

```console
sudo su -
```

---

## Grep lines before after match

-B before  
-A after

Stick ne lines just after option

```console
grep -B2 -A3 pattern infile.txt
```

---

## Copy files from list

Copy specific files from a text list of files

```console
rsync -a sourcefolder --files-from=list.txt destinationfolder
```

---

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

---

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

---

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

---

## Search between timestamp

```console
sed -rne '/10:50/,/11:05/ p' file
```

Put existing time range in file (10:50 - 11:05).

---

## Highlight search result

```console
grep --color=always -z pattern file | less -R
```

always to transmit color through pipe  
-z to show everything, not only the matching pattern  
-R to avoid showing esc char instead of color

---

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

---

## Where is a program

How to know where reside a program, e.g. ls?

```console
which ls
/usr/bin/ls

env

env | grep PATH
```

---

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

---

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

---

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

---

## List files

Sorted by sizes and human readable

```console
ll -S -h
```

---

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

revert sorted output, numeric value (-n) otherwise it's alphabetically (1.. 10.. 100.. then 2.. 20.., etc.)

```console
cat test.txt | sort | uniq -c | sort -n -r
      3 22
      2 71
      2 1
      1 7
      1 45
      1 3
      1 11
```

---

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

---

## Grep end of line after match

[SRC](https://stackoverflow.com/questions/20567667/grep-command-to-add-end-line-after-every-match)

```console
cat error.log | grep -A 1 -B 1 --group-separator==============\\r\\n "not valid"
```

---

## Empty file

Empty File Content by Redirecting to Null:

```console
> access.log
```

---

## Disk usage

```console
df -h
```

Huge file:

```console
sudo du -xh / | grep -P "G\t"
```

---

## Last modification of a file

```console
date -r fileName
```

---

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

---

## Since when a process run

```console
ps -p pid -o etime
```

Then use this [online tool](https://www.timeanddate.com/date/timeduration.html?) to calculate time between above result and now.

---

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

---

## Copy filtered file list

Copy (for move replace below cp by mv) grep"ed" filtered files list to another folder.

```console
ls | grep pattern1 | grep pattern2 | ... | xargs cp -t /destinationFolder
```

---

## Add prefix to file names list

```console
for f in * ; do mv -- "$f" "PRE_$f" ; done
```

---

## Curly braces

Using braces to build a sequence.

[All about {Curly Braces} in Bash](https://www.linux.com/topic/desktop/all-about-curly-braces-bash/)

```console
echo {0..10}
0 1 2 3 4 5 6 7 8 9 10
```

---

## Search replace

Search (s) and replace in place (-i) all occurrences (g) of each lines in a file:

```console
sed -i 's/SEARCH/REPLACE/g' filename
```

---

## Add new line after delimiter

```console
cat someFile | tr ',' '\n' > someOtherFile
```

---

## Find file

Find file everywhere

```console
find / -type f -iname "foo*txt"
```

---

## Occurrence between positions

file.log

```txt
8308353738102486352f010201027fcb627c31e5627c31e51b1fed6e057142e2000050430000006f01200800000affbf2f0a19006a12060000957ff100005896000000000000000000000001007e2e6c
8308352739097829334f0102010252e2627c31e5627c31e51c06e102042bcf150000cd1c0000000200e309000001ffb54f0919006a120600001bd13600006861000000000000000000000003006ed60b
8308353738102489273f01020102a34f627c31e6627c31e61c9f9ea6ff6a31fa00003f220000001000c60800000affc96f0a19006a12060000cc05c700005ea5000000000000000000000003005ecd9f
8308352739097829607f010201025a0d627c31e6627c31e61c071bfd042c35d60000c71f00000002004409000001ffb14f0a19006a12060000259d1c0000689e000000000000000000000003007a3a7b
8308352739097880667f0102010259e1627c31e6627c31e61c072119042c417a0000c87f0000000000320a000001ffb74f0919006a12060000268bd600006991000000000000000000000003007a6424
8308359739077046597f01020102dc55627c31e6627c31e61c0c20cc043558d20000c0df0000000b00fc07000001ffaf4f0b01005012050002498f8300002f24000000000000000000000001
8308352739097869488f0102010295a6627c31e7627c31e71c22e13f048031af0000bd94000000f700f40b000001ffcb4f0719006a1206000131f5b200005c2600000000000000000000000100784265
8308359739074198193f010201092d26627c31ea627c31ea1c403622047e2e0e0000cdf800000005000607000001ffc34f0c010000004700000000000000001f0306a54300005d75000000000000000000000005001400293030313030323033323030393030323033333232303137393030303030303030303033323030320d0a
8308359739074921768f010201028cd9627c31ea627c31ea1c0276a2044346fc0000c0bc00000160008b0a000001ffb34f080100501205002539df8700006d9b000000000000000000000001
8308359739074921768f010201028cda627c31ec627c31ec1c02747a044347270000c0de0000012300b40a000001ffb34f082100501205002539df8e00006d21000000000000000000000001
8308353738102196969f010201097329627c31cb627c31a01bd886d5042236490001564100000008000007200001ffaf0010000800004700000000000000001f00f4fec700002e13000000000000000000000000001400293030313030323033323030393030383030313030333036313030303030303030303036343037350d0a
8308353738102196969f01020109732a627c31cb627c31a01bd886d5042236490001564100000008000007240000ff8f0010000800004700000000000000001f00f4fec700002e13000000000000000000000000001400293030313030323033323030393030383030313030343232363030303030303030303036343136350d0a
8308353738102196969f01020109732b627c31cb627c31a01bd886d5042236490001564100000008000007240000ff8f0010000800004700000000000000001f00f4fec700002dd6000000000000000000000000001400293030313030323033323030393030383030313030343034363030303030303030303033323132310d0a
8308359739074921768f010201028cdb627c31ee627c31ee1c02739c0443456a0000c0840000011800e009000001ffb34f090100501205002539df9400006d03000000000000000000000001
8308353738102499926f010201029e30627c31ee627c31ee1bf9c2b40478e1070000f1ef00000042013c08000001ffad4f0c19006a1206000052d4cf000066b8000000000000000000000003007efec1
8308353738102524517f010201024489627c31ef627c31ef1bcf8c4803e28a9f0000d3f200000000003409000001ffbb4f0919006a120600001a58fa00003756000000000000000000000003002ab371
8308353738102312889f010201024c09627c31ef627c31ef1bf9b47a0478fad60000f27e00000042013807000001ffc94f0d19006a120600004dadf600006955000000000000000000000003009e80a3
8308359739077086684f010201090f9d627c31ce627c314b1b8cfbad04643f4c0000cd6900000008016206240001ffc3000e000800004700000000000000001f02d207da00003166000000000000000000000000001400293030313030323033323030393030383031323038333039353030303030303030303033323233380d0a
8308353738102523980f010201020f2f627c31f1627c31f11b8d9d7304674fb10000d76900000000000108000001ffb94f0a59006a1206000004b7de0000001e000000000000000000000001000c201b
8308353738103013429f010201029ffb627c31f2627c316d1ba209ca049fedab0001e82000000000015504040001ffb34f0e01086a120600000851b200006b59000000000000000000000003001d909e
...
```

```console
cut -c5-19 file.log | sort | uniq -c | sort -r | head -20
   7301 352739090974673
   2615 352431063062294
   2593 359739074198193
   2086 359739074201385
   1701 359739074263179
   1636 359739074206988
   1600 359739074197658
   1451 353738102180005
   1358 359739074259185
   1342 352739097909052
   1306 352739090926269
   1294 359739074263237
   1292 353738102197058
   1268 359739074261710
   1247 359739072525975
   1228 352739098097725
   1222 353738102229216
   1154 359739074198177
   1146 359739074261678
   1143 352739097814815
```

---

## Occurrence of column value

file.log

```txt
864394040507255;2022-05-12T07:35:05.027Z;000000000000004c080100000180b7314c5800047ba3c41b96f137021600ba0d001d00110aef01f00150011505c8004501ed02715dfb00fc0006b50007b60003426f1118001b430fd4440000011003a27f140001000010dd
864394040405104;2022-05-12T07:35:05.029Z;000000000000004c080100000180b73144880003f617d01bc600df02cf00be0e0000ef110aef00f00150011505c8004501ed02715ffb00fc0006b50004b6000242377d180000430fe2440000011001b0a87a00010000765d
864394040167563;2022-05-12T07:35:05.029Z;000000000000004c080100000180b7314c580003a87f4b1b866a18018501140c000e00110aef01f00150011505c8004501ed027164fb00fc0006b50004b600034234cb18000e43101b4400000110014d5e330001000091b9
864394040096895;2022-05-12T07:35:05.033Z;000000000000005d080100000180b7314c5800040b57011c0faa1903f100240d002b00160eef01f00150011505c8004501b300b400ed02715dfa01fb00fc00f80106b50004b6000242385718002b430fd0440000011001033b1a014e012451dd310000e60100009472
864394041295967;2022-05-12T07:35:05.080Z;000000000000004c080100000180b7314c580004159ddd1bb0dd9b016f009610000c00110aef01f00150011505c8004501ed02715efb00fc0006b5000ab600064231ef18000c430fd94400000110003be7ae0001000061cd
864394040166508;2022-05-12T07:35:05.118Z;0000000000000062080100000180b7314c580003eb71a41bbd7af801ab00700e002519180fef01f00150011505c8004501b300b4001d00ed02715bfa01fb00fc00f80107b50005b600034235f6180025430fb84400001907210110081c85f9014e0000000000000000010000911e
864394040318653;2022-05-12T07:35:05.118Z;0000000000000062080100000180b7314c58000498609a1c266cdb019f01480e003500180fef01f00150011505c8004501b300b4001d00ed027158fa01fb00fc00f80107b50008b60004423186180035430f9744000019073a011003820948014e0104696aba5f4d8a0100003eb9
864394040172985;2022-05-12T07:35:05.133Z;000000000000004c080100000180b73111c0000433a4951bc916950314014b110000ef110aef01f00050001505c8004501ed027151fb00fc0006b50004b60002422c85180000430f47440000011002551d6a000100009245
864394040892798;2022-05-12T07:35:05.149Z;00000000000000de8e0100000180b731504000042266531bae2fde02ea00da11001c0000001b000f00ef0100f00100500100150300c80000450100b30000b40000ed0200715201070100fa0100fb0000fc0000f801000800b5000b00b60007004238b70018001c00430f54004400000019022c001af8df000100100278bdf70001004e0104262baaf4721b0002014b002802010605166e2a2c020e0950205420454e2038303446333200000000000000000000000000000000014c002802010605166e2adff80e0950205420454e2038303446333300000000000000000000000000000000010000e60b
864394040894265;2022-05-12T07:35:05.198Z;000000000000004c080100000180b7314c580003f5e7d41be1a06c01b300920e000c00110aef01f00150011505c8004501ed02715cfb00fc0006b5000eb6000742386a18000c430fca4400000110011faa0300010000dcd0
864394041317902;2022-05-12T07:35:05.295Z;000000000000004c080100000180b7314c58000515aa7f1c3e230a0194015c0f000700110aef01f00150011504c8004501ed02715efb00fc0006b50004b600024236d4180007430fde4400000110005b17300001000082c3
864394041300338;2022-05-12T07:35:05.317Z;0000000000000095080200000180b730719800048b2dc31c0cd0bc021700bf0c0000fb110aef01f00050001505c8004501ed02715ffb00fc0006b50008b60004423009180000430fe744000001100065b5eb0000000180b730796800048b2dc31c0cd0bc021700bf0c0000ef110aef00f00050001505c8004501ed02715ffb00fc0006b50008b60004423045180000430fe744000001100065b5eb000200007de2
864394040179410;2022-05-12T07:35:05.320Z;000000000000005f080100000180b731487000044cc01f1b75b47d03e701050e000a00170fef01f00150011505c8004501b3000200b400ed027159fa01fb00fc00f80106b50006b6000342311618000e430f9d440000011001638427014e018478c68a0000600100007506
864394041319494;2022-05-12T07:35:05.335Z;00000000000000808e0100000180b730813800059eab7a1c517f9d000000000000000019000d000700ef0000f00000500000c80200450300715401070100040042310800430f680044000000190aa8000100100091f8a200000001014b002802010605166e2aa80a100950205450524f4245203030303432420000000000000000000000000000010000d0cb
864394040317283;2022-05-12T07:35:05.389Z;0000000000000062080100000180b731487000053e79c31c34d2aa020d01460d000d00180fef01f00150011505c8004501b300b4001d00ed027155fa01fb00fc00f80107b50007b6000342377418000d430f76440000190778011003e0419b014e01042d60ba5f4d670100007b72
864394040978779;2022-05-12T07:35:05.396Z;000000000000005d080100000180b73150400003f81ea01bb8e77c01af00690f001000160eef01f00150011505c8004501b300b400ed027160fa01fb00fc00f80106b50005b6000242345c180010430fec440000011000d6972b014e011464dd3100005d0100001c81
864394040892228;2022-05-12T07:35:05.414Z;00000000000000de8e0100000180b73150400003e42b561bb93f2e018200731000000019001b000f00ef0000f00000500000150500c80000450100b30000b40000ed0200715701070100fa0100fb0000fc0000f801000800b5000a00b60006004232120018000000430f8900440000001903e8001a03520001001001b159d90001004e01044d3ebaf472890002014b002802010605166e2ae8030e0950205420454e2038303446303600000000000000000000000000000000014c002802010605166e2a52030e0950205420454e20383034463037000000000000000000000000000000000100009fed
864394040912182;2022-05-12T07:35:05.480Z;00000000000000c0080300000180b7311d7800051878db1c3ae6d900000000000000010b07ef00f0005000c802450301017157034230fd430f8944000001100005168a0000000180b731216000051878db1c3ae6d900000000000000ef0b07ef01f0005000c802450301017157034230e4430f8944000001100005168a0000000180b731504000051878db1c3ae6d900000000000000f0110aef01f00150011500c80045020101ed027157fc0006b50000b6000042348c180000430f8944000001100005168a000300008465
866907053384763;2022-05-12T07:35:05.507Z;000000000000004c080100000180b73150400005a65ddb1c23799501bb015414001100110aef01f00150011504c8004501ed027100fb00fc0006b50008b600054239f018001143000044000001100010f980000100003a2c
864394040893135;2022-05-12T07:35:05.622Z;00000000000000de8e0100000180b7314c580003f225c91bbe004e026900f91000080000001b000f00ef0100f00100500100150500c80000450100b30000b40000ed0200715a01070100fa0100fb0000fc0000f801000800b5000a00b600060042383e0018000800430fb30044000000190539001a045e0001001001a997860001004e01041e6f12f572120002014b002802010605166e2a39050e0950205420454e2038303446303500000000000000000000000000000000014c002802010605166e2a5e040e0950205420454e203830344630410000000000000000000000000000000001000000cd
...
```

```console
cat file.log | awk -F ';' '{print $1}' | sort | uniq -c | sort -r | head -20
    434 864394041317027
    424 864394040899470
    423 864394040909931
    419 864394040912117
    396 864394040896856
    384 864394040913370
    376 864394040891147
    363 864394040989388
    363 864394040892012
    362 864394040292239
    355 864394040996292
    355 864394040907570
    353 864394040915094
    348 864394040917314
    346 864394040924849
    343 864394040894307
    336 864394040892798
    331 864394041302482
    328 864394040907448
    328 864394040892780
```

---

## Occurrence live

watch - execute a program periodically, showing output full screen

Live occurrences change of column (first in example below '$1') value in [file.log](#occurrence-of-column-value) with ';' separated column values:

```console
watch -d "awk -F ';' '{print \$1}' file.log | sort | uniq -c | sort -n -r | head -20"
```

[Linux Watch Command](https://linuxize.com/post/linux-watch-command/)

---

## jq

[Command-line JSON processor](https://stedolan.github.io/jq/)

Select a field that contain true and output another field (+ count + sort + head result):

```console
jq 'select(.fieldThatMayContainTrue|startswith("true"))|.outputField' file.ndjson | sort | uniq -c | sort -n -r | head -50
```

---

## Find patterns across multiple lines

Find in file between "abc" AND "efg", in that order.

test.txt:

```txt
blah blah..
blah blah..
blah abc blah
blah blah..
blah blah..
blah blah..
blah efg blah blah
blah blah..
blah blah..
```

```console
sed -n '/abc/,/efg/p' test.txt
```

output:

```txt
blah abc blah
blah blah..
blah blah..
blah blah..
blah efg blah blah
```

---

## Number of pattern occurrence through files

```console
grep -c pattern files*
```

---

## Bash option select

Script template launched with option(s).

optionselect script, chmod +x then ./optionselect to execute:

```sh
#!/bin/bash

trap 'exit 130' INT

optionwitharg="option with arg = -y arg"
optionwithoutarg="option without arg = -n"

if [ $# -eq 0 ]; then
  printf '\n'
  echo "usage: "$optionwitharg", "$optionwithoutarg
  printf '\n'
  echo "e.g.: $ ./optionselect -n -y hello"
  printf '\n'
  exit 1
fi

while getopts "h?y:?n" opt; do
  case "$opt" in
    h|\?)
      echo "help!.. not implemented yet, sorry :("
      exit 0
      ;;
    y)  filter=$OPTARG
      echo "$OPTARG"
      ;;
    n)
      echo "no arg"
      ;;
  esac
done

shift $((OPTIND-1))
```

---

## dos2unix

Utility to reformat text files generated under Windows for use under Linux:

```console
dos2unix file
```

---

## New line

### Add new line

Adding new line after separator `;` to one liner file, e.g. with following file content:

```txt
<key1>:<value1>;<key2>:<value2>;<key3>:<value3>;..
```

To replace ';' with ';' + new line:

```console
sed -i 's/;/;\n/g' <file name>
```

Then ine file:

```txt
<key1>:<value1>;
<key2>:<value2>;
<key3>:<value3>;
..
```

### Remove new line

Remove new line after separator `;` to do a one liner file, e.g. with following file content:

```txt
<key1>:<value1>;
<key2>:<value2>;
<key3>:<value3>;
..
```

To replace ';' + new line with ';':

```console
sed -i ':a;N;$!ba;s/\n//g' <file name>
```

Then ine file:

```txt
<key1>:<value1>;<key2>:<value2>;<key3>:<value3>;..
```

---

## Replace value for key

In a key value pair file (format "`<key>`:`<value>`;"), replace value for key, e.g. `107:what ever;` to `107:1;`:

```console
sed -i '/^107/s/:.*;/:1;/' <file name>
```

---

## Folder size

`ls` or `ll` does not show folder content size, use below instead ([src](https://unix.stackexchange.com/questions/365369/ls-ls-isnt-showing-true-size-of-directory)):

```console
du -ha -d 1 | sort -hr
```

---

## Live log filtered

Remove complete stray lines containing pattern from live log:

```console
tail -1000f <fileName.log> | sed '/<pattern 1>/d' | sed '/<pattern 2>/d'
```

---

## Date ISO 8601

e.g. "2023-03-07T12:39:16.313Z":

```console
date -u +"%Y-%m-%dT%H:%M:%S.%3NZ"
```

---

## Sort file by column

e.g. in.csv:

```csv
parameterKey,parameterValue
10,F8000016CA38C301
11,8A000018F4B85B01
12,38000019C77DBB01
14,9F511A8A40360401
15,34000016CA5E1401
16,FF4D5FBA66830401
17,99511A8A439C0401
18,DE6407725F900401
13,E0000016CA226401
28,AF4D5FBA66370401
29,034D5FBA61120401
30,AB4D172A3B070401
...
```

sort command with `-n` argument to sort in numeric order:

```console
sort --field-separator='1' --key=1 -n in.csv > out.csv
```

out.csv:

```csv
parameterKey,parameterValue
1,A2000002D40D2701
2,A70000178A254101
3,08000016CA925901
4,86000018F4D6B201
5,34000016CA14F801
6,BF000016CA4FFA01
7,39000016CABBB701
8,32000016CADA6901
9,3F00001A90AFC001
10,F8000016CA38C301
11,8A000018F4B85B01
12,38000019C77DBB01
...
```

---

## Cut column in a file

e.g. in.csv:

```csv
parameterKey,parameterValue
1,A2000002D40D2701
2,A70000178A254101
3,08000016CA925901
4,86000018F4D6B201
5,34000016CA14F801
6,BF000016CA4FFA01
7,39000016CABBB701
8,32000016CADA6901
9,3F00001A90AFC001
10,F8000016CA38C301
...
```

to cut first column:

```console
cut -d, -f1 --complement in.csv > out.csv
```

out.csv:

```csv
parameterValue
A2000002D40D2701
A70000178A254101
08000016CA925901
86000018F4D6B201
34000016CA14F801
BF000016CA4FFA01
39000016CABBB701
32000016CADA6901
3F00001A90AFC001
F8000016CA38C301
...
```

---

## env

`env` is used to print environment variables.

Without any argument : print out a list of all environment variables:

```console
env
SHELL=/bin/bash
WSL_DISTRO_NAME=Ubuntu-20.04
..
```

---

## Command substitution

```console
$( ... )
```

It allows you to execute a command inside `$( ... )` and use its output in another command. The shell replaces the expression within the parentheses with the result of the executed command.

Examples:

```console
echo "The file is: $(ls | grep misc | tail -1)"

zgrep -E '<id>.*pattern=<#>' "$(ls | grep misc | tail -1)" | wc -l
```

---

## less

Display a file.

With +F option, follow the end of file while growing (like tail -f):

```console
less +F *.log
```

---

## tail

Display end of file.

To display follow, multiple files at a time each file has to be mentioned (e.g. *.log do not work).

To display follow 2 logs file and having file name from which it comes (grep in between do not work):

```console
tail -f debug.log error.log | awk '{ print FILENAME ": " $0 }' FILENAME=-
```

---
