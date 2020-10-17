<!-- SPDX-License-Identifier: CC-BY-NC-ND-4.0 -->
# Bad man (easy, 200pt)

_Solved on 18 Sep. 2020 (Live)._

## Statement

> **Author**: n00bmaster
>
> We have recently received reports about a hacker who goes by the alias und3rm4t3r. He has been threatening innocent
> people for money and must be stopped. Help us find him and get us the flag.

## Solution

Run the username through [CheckUsernames](https://checkusernames.com/).

![Screenshot of CheckUsernames; und3rm4t3r displays as Not Available on Twitter, and is
underlined.](../../../../.assets/Events/2020/09_DownUnderCTF/osint/Bad_man-0.png)

There were a few sites where `und3rm4t3r` was taken, but given the cyber community is quite active on Twitter, intuition
placed Twitter at the highest relevance. Visit [them on Twitter](https://twitter.com/und3rm4t3r).

![Screenshot of und3rm4t3r's Twitter page; a retweet of DownUnderCTF content is
visible.](../../../../.assets/Events/2020/09_DownUnderCTF/osint/Bad_man-1.png)

Given the retweet of DownUnderCTF's content, we may be almost certain that this is the intended account. Scroll through
some tweets until [this tweet](https://twitter.com/und3rm4t3r/status/1286261444997046276), which reads:

> whew that was close.... put out a tweet that contained personal information... welp im glad we have a delete button

Unfortunate. Go to the [WayBack Machine](https://web.archive.org/), and query [this
page](http://web.archive.org/web/*/https://twitter.com/und3rm4t3r). There is a single save from [July
23](http://web.archive.org/web/20200723112257/https://twitter.com/und3rm4t3r), which reveals the following:

![Screenshot of the WayBack Machine, displaying und3rm4t3r's Twitter page at July 23; there is a Tweet presenting the
flag.](../../../../.assets/Events/2020/09_DownUnderCTF/osint/Bad_man-2.png)

I don't know about you, but _I'm_ glad that the delete button was fake.

```txt
DUCTF{w4y_b4ck_1n_t1m3_w3_g0}
```
