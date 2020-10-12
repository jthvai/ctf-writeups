# Name that Machine (1pt)

_Solved on 09 Sept. 2020 (Live)._

## Statement

> What is the name of the computer with an IP of 192.168.56.104?

- `what_is_my_name.pcap`

## Solution

Load the attached `pcap` into [WireShark](https://www.wireshark.org/). Note that packets with the `BROWSER` protocol
contain host announcements:

![Screenshot of WireShark, two rows are highlighted, displaying "Host Announcement KERMIT" and "Host Name: KERMIT"
respectively.](../.assets/Name_that_Machine-0.png)

Search for a `BROWSER` packet from the correct IP; one of them contains another host announcement.

![Screenshot of WireShark, two rows are highlighted, displaying "Host Announcement SCOOTER" and "Host Name: SCOOTER"
respectively.](../.assets/Name_that_Machine-1.png)

```txt
SCOOTER
```
