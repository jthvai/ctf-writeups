# ANU Networking Challenges

Wed, 23 Sept. 2020 - Wed, 07 Oct. 2020

_Solved on 05 Oct. 2020._

## Challenge 1: Network Ports

### Statement

> #### Merchandise Dispenser (Hallway)
>
> The Infranetics Merchandise Machine has a numberpad to enter item codes. However it appears to be out of company swag.
> The Dispenser has a cable hanging out the back that has been unplugged from the ethernet |cport|n on the wall. A label
> reads "Maintained by Elite Maintenance"... though they clearly haven't visited in some time. The labels on the back of
> the dispenser have all been scratched out over time, but you can barely make out the IP address 13.211.45.103
>
> Find the port used by the challenge, don't cheat! :)

### Solution

[Nmap](https://nmap.org/) the challenge server to find open ports.

```shell
$ sudo nmap -sS 13.211.45.103
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-05 11:35 AEDT
Nmap scan report for ec2-13-211-45-103.ap-southeast-2.compute.amazonaws.com (13.211.45.103)
Host is up (0.021s latency).
Not shown: 990 closed ports
PORT      STATE    SERVICE
22/tcp    open     ssh
53/tcp    open     domain
80/tcp    open     http
2222/tcp  open     EtherNetIP-1
31337/tcp open     Elite

Nmap done: 1 IP address (1 host up) scanned in 5.51 seconds
```

Port 31337 looks very... elite. [Netcat](http://netcat.sourceforge.net/) the port.

```shell
$ nc 13.211.45.103 31337
You found the secret underground hacker resistance server
Planning will begin shortly blah blah blah
Have a flag ANU_CTF{0BF0DA295E864B5D}
```

## Challenge 2: Network SNMP

### Statement

> What else is available on 13.211.45.103?

### Solution

Per the challenge name, check the ports for [SNMP](https://en.wikipedia.org/wiki/Simple_Network_Management_Protocol),
noting that it speaks UDP.

```shell
$ sudo nmap -sU -p161,162 13.211.45.103
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-05 12:01 AEDT
Nmap scan report for ec2-13-211-45-103.ap-southeast-2.compute.amazonaws.com (13.211.45.103)
Host is up (0.020s latency).

PORT    STATE  SERVICE
161/udp open   snmp
162/udp closed snmptrap

Nmap done: 1 IP address (1 host up) scanned in 0.26 seconds
```

Use `snmpwalk` from [Net-SNMP](http://www.net-snmp.org/) to interact with the server (optional step: wrangle the tool
for an hour to try and get the version right). The server will output a _lot_ of text, one line of which is the flag.

```shell
$ snmpwalk -v2c 13.211.45.103 -c public
# ...
HOST-RESOURCES-MIB::hrSWRunParameters.1 = STRING: "/bin/start.sh"
HOST-RESOURCES-MIB::hrSWRunParameters.6 = STRING: "/bin/sync.sh -o flag=\"ANU_CTF{85318A5AD3B5666F}\""
HOST-RESOURCES-MIB::hrSWRunParameters.7 = STRING: "-f"
HOST-RESOURCES-MIB::hrSWRunParameters.18204 = STRING: "60"
# ...
```

## Challenge 3: Network tcpdump

### Statement

> #### Traffic Dump (Security Office - Post-it Note)
>
> Hi Marshall, a security incident occured just as my shift ended. I've uploaded the traffic to the security storage
> folder. In case you forgot the IP it's still "13.211.45.103"

### Solution

See what is being served over HTTP.

```shell
$ curl http://13.211.45.103:80
<h1>Intercep-tron 9000</h1> </br>
    The Intercep-tron 9000 utilises the latest Infranetics surveillance technology. </br>
    Get yours today only for 40,000 Credits!! </br>
    For a sample of the output, please click <a href="file">here</a>
```

Acquire and identify the mysterious file.

```shell
$ curl -O http://13.211.45.103:80/file
$ file file
file: pcap capture file, microsecond ts (little-endian) - version 2.4 (Ethernet, capture length 65535)
```

Open the file in [WireShark](https://www.wireshark.org/), and sort by protocol.

![Screenshot of Wireshark displaying two HTTP packets. One is highlighted, and shows a GET request to
/ayyyy.](.assets/2020-09-23_ANU_Networking_Challenges-0.png)

Download the PNG queried above.

```shell
curl -o ayyyy.png http://13.211.45.103:80/ayyyy
```

![Downloaded image, displaying KIWICON-CTF[C87C171906D37FB6] in rainbow text.](.assets/2020-09-23_ANU_Networking_Challenges-1.png)
