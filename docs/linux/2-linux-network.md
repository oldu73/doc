# Linux - 02 - Network

---

## Network connection

```console
(sudo) netstat -tunlp
```

---

## Live network connection

Live log established network connections on a dedidcated port number of a load balancer server and forwarded IP of communication server filtered out:

```console
watch -d 'netstat -an | grep :<port> | grep ESTABLISHED | grep -v <ip>'
```

---

## Send raw data hex UDP

Sending hex raw data through UDP.

### References

[Linux nc command help and examples](https://www.computerhope.com/unix/nc.htm#:~:text=option%20is%20given.-,Client/server%20model,-It%20is%20quite)  
[How To Use Netcat to Establish and Test TCP and UDP Connections | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-use-netcat-to-establish-and-test-tcp-and-udp-connections)  
[linux - convert a hex string to binary and send with netcat - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/82561/convert-a-hex-string-to-binary-and-send-with-netcat#:~:text=%7C%20perl%20%2De%20%27print%20pack%20%22H*%22%2C%20%3CSTDIN%3E%27%20%7C)  
[Linux/UNIX: Bash Read a File Line By Line - nixCraft](https://www.cyberciti.biz/faq/unix-howto-read-line-by-line-from-file/#:~:text=%23!/bin/bash%0Ainput%3D%22/path/to/txt/file%22%0Awhile%20IFS%3D%20read%20%2Dr%20line%0Ado%0A%20%20echo%20%22%24line%22%0Adone%20%3C%20%22%24input%22)  
[shell script - Wait for keyboard input inside a while-read loop - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/445153/wait-for-keyboard-input-inside-a-while-read-loop#:~:text=the%20loop%2C%20the-,read%20keypress,-reads%20from%20this)  
[linux - How to automatically close netcat connection after data is sent? - Server Fault](https://serverfault.com/questions/512722/how-to-automatically-close-netcat-connection-after-data-is-sent#:~:text=echo%20hello%7Cnc-,%2Dw%201,-%2Du%2010.0.30.255%20%2Dp)  
[Echo newline in Bash prints literal \n - Stack Overflow](https://stackoverflow.com/questions/8467424/echo-newline-in-bash-prints-literal-n#:~:text=Use-,printf,-instead%3A)  
[perl - Shell magic wanted: format output of hexdump in a pipe - Stack Overflow](https://stackoverflow.com/questions/5974607/shell-magic-wanted-format-output-of-hexdump-in-a-pipe#:~:text=you%20can%20use-,xxd%20%2Dp,-to%20make%20a)

### One shot

Command line, manual sending, one raw data frame (Like Network Stuff):

```console
echo -n "830f9d10362f4bc8a1c.." | perl -e 'print pack "H*", <STDIN>' | timeout 0.2 nc -w 1 -u <ip> <port> | xxd -p
```

### Script straight or line by line

Read file straight forward or line by line.

sample (file):

```txt
8308352739096415820f01020102128362d80bb162d80bb11c1e6..
8308359739077103745f01020102b2df62d80bb262d80bb11bd9d..
..
```

readsample script, chmod +x then ./readsample to execute:

```sh
#!/bin/bash

trap 'exit 130' INT

modearg="mode arg = -s for straight or -l for line by line"
filearg="file arg = input file name"
iparg="ip arg = server IP address"
portarg="port arg = server port number"

if [ $# -eq 0 ]; then
  printf '\n'
  echo "usage: "$modearg", "$filearg", "$iparg", "$portarg
  printf '\n'
  echo "e.g.: $ ./readsample -s|-l sample 192.168.1.141 50300"
  printf '\n'
  exit 1
fi

input=$2
ip=$3
port=$4

printf '\n'

while getopts "h?s?l" opt; do
  case "$opt" in
    h|\?)
      echo "help!.. not implemented yet, sorry :("
      printf '\n'
      exit 0
      ;;
    s)
      echo "straight mode.."
      printf '\n'

      printf '\n'

      while IFS= read -r line
      do
        echo "$line"
        printf '\n'
        echo -n "$line" | perl -e 'print pack "H*", <STDIN>' | timeout 0.2 nc -w 1 -u "$ip" "$port" | xxd -p
        printf '\n'
      done < "$input"

      printf '\n'
      echo 'Done.'
      printf '\n'

      exit 0
      ;;
    l)
      echo "line by line mode.."
      printf '\n'

      printf '\n'

      exec 3<&0

      printf '\n'

      while IFS= read -r line
      do
        echo "$line"
        printf '\n'
        echo -n "$line" | perl -e 'print pack "H*", <STDIN>' | timeout 0.2 nc -w 1 -u "$ip" "$port" | xxd -p
        printf '\n\nPress <enter> to continue: ' >&2
        read keypress <&3
        printf '\n'
      done < "$input"
      exec 3<&-

      printf '\n'
      echo 'Done.'
      printf '\n'

      exit 0
      ;;
  esac
done
```

---

## Forward traffic

Forward network communications when you are forced to keep the IP address of a server because of devices deployed in the field that are no longer maintainable to change the IP address of the server with which they communicate.

Solutions:

- [socat](#socat), communication relay  
- [Traefik](https://doc.traefik.io/traefik/) open-source Edge Router

---

## check open port

### TCP

With telnet, open connection:

```console
telnet localhost <tcp port number>
```

Close connection:

```console
type Ctrl + ] on your keyboard

telnet>

type in the word 'close' to close the session
```

### UDP

With nmap, open connection:

```console
nmap -sU -p <port #> <ip address or alias>
```

---

## netcat

Netcat (nc) operates by initiating a TCP connection to a remote host.

The most basic syntax is:

```console
netcat [options] <host> <port>
```

---

## socat

The [`socat`](https://www.redhat.com/sysadmin/getting-started-socat) utility is a relay for bidirectional data transfers between two independent data channels.

For multiple connections, use the `fork` option as used in the examples below:

```console
socat TCP4-LISTEN:81,fork,reuseaddr TCP4:TCP4:192.168.1.10:80
```

This example listens on port 81, accepts connections, and forwards the connections to port 80 on the remote host.

---

## hostname

The hostname command in Linux is used to display or set the system's hostname. It can be used to view the current hostname or to temporarily change it. For persistent changes, the hostnamectl command or editing the /etc/hostname file are recommended.

To display the current hostname, simply type `hostname` in the terminal and press Enter.

---
