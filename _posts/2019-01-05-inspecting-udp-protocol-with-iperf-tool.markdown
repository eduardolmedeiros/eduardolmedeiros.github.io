---
layout: post
title: Inspecting UDP protocol with iperf tool
date: '2019-01-05 19:42:13'
categories: networking iperf udp
---

Sometimes we have to do some troubleshooting to inspect UDP traffic and make sure that there is no issue on the operational system layer. This task is so common nowadays because a lot of applications uses multicast IP and UDP protocol for clustering and replications.

There is a nice networking tool to help: [iperf](https://iperf.fr/).

For this setup, let's imagine that I have an application clusterized running on several machines, using UDP protocol, multicast and listening on port 48000. However, I wanna make sure and check the network traffic is working as expected.

**Let's play a bit with iperf:**

1. Install iperf package in all machines.

{% highlight shell %}
$ yum install iperf
$ apt-get install iperf
{% endhighlight %}

1. Check the multicast ip interface in all machines.

{% highlight shell %}
$ ip m s
1:	lo
inet 224.0.0.1
inet6 ff02::1
inet6 ff01::1
2:	eth0
link 33:33:00:00:00:01
link 01:00:5e:00:00:01
link 33:33:ff:ec:54:83
link 33:33:00:00:02:02
link 01:00:5e:00:05:f1
inet 230.0.5.241 <=
inet 224.0.0.1 <=
inet6 ff02::202
inet6 ff02::1:ffec:5483
inet6 ff02::1
inet6 ff01::1
{% endhighlight %}

1. Setup the iperf to use multicast ip 230.0.5.241, UDP and port 48000 on server1.

{% highlight shell %}
$ iperf -s -u -B 230.0.5.241 -p 48000 -i 1
{% endhighlight %}

1. Now let's start iperf on server 2.

{% highlight shell %}
$ iperf -c 230.0.5.241 -u -T 32 -t 3 -p 48000 -i 1
{% endhighlight %}

1. Checking the output.

**server 1:**

{% highlight shell %}
------------------------------------------------------------
Server listening on UDP port 48000
Binding to local address 230.0.5.241
Joining multicast group 230.0.5.241
Receiving 1470 byte datagrams
UDP buffer size: 208 KByte (default)
------------------------------------------------------------
[3] local 230.0.5.241 port 48000 connected with 192.168.1.7 port 35838
[ID] Interval Transfer Bandwidth Jitter Lost/Total Datagrams
[3] 0.0- 1.0 sec 129 KBytes 1.06 Mbits/sec 0.058 ms 0/ 90 (0%)
[3] 1.0- 2.0 sec 128 KBytes 1.05 Mbits/sec 0.054 ms 0/ 89 (0%)
[3] 2.0- 3.0 sec 128 KBytes 1.05 Mbits/sec 0.058 ms 0/ 89 (0%)
[3] 0.0- 3.0 sec 386 KBytes 1.05 Mbits/sec 0.056 ms 0/ 269 (0%)
{% endhighlight %}

**server 2**

{% highlight shell %}
------------------------------------------------------------
Client connecting to 230.0.5.241, UDP port 48000
Sending 1470 byte datagrams, IPG target: 11215.21 us (kalman adjust)
Setting multicast TTL to 32
UDP buffer size: 208 KByte (default)
------------------------------------------------------------
[3] local 192.168.1.7 port 35838 connected with 230.0.5.241 port 48000
[ID] Interval Transfer Bandwidth
[3] 0.0- 1.0 sec 131 KBytes 1.07 Mbits/sec
[3] 1.0- 2.0 sec 128 KBytes 1.05 Mbits/sec
[3] 2.0- 3.0 sec 128 KBytes 1.05 Mbits/sec
[3] 0.0- 3.0 sec 386 KBytes 1.05 Mbits/sec
[3] Sent 269 datagrams
{% endhighlight %}

1. Did you see some UDP traffic coming in both machines? Yes? That means the operational system and network layer are working as expected. If not, too bad. I recommend taking a look at:

- Firewall
- Network (route and interface)

Cheers!
