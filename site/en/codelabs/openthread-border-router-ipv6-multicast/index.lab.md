---
id: openthread-border-router-ipv6-multicast
summary: Thread border routers support multicast forwarding, which allows multicast communication between Thread network and Infrastructure (Wi-Fi/ethernet) network segments.  This codelab will guide you through the process to set up and play with Thread Multicast features.
status: [final]
authors: Simon Lin, Jonathan Hui
categories: Nest
tags: web
layout: scrolling
feedback link: https://github.com/orgs/openthread/discussions
project: /_project.yaml
book: /_book.yaml

---

# Thread Border Router - IPv6 Multicast

[Codelab Feedback](https://github.com/orgs/openthread/discussions)


## Introduction



<img src="img/608c4c35050eb280.png" alt="608c4c35050eb280.png"  width="624.00" />

### What is Thread?

Thread is an IP-based low-power wireless mesh networking protocol that enables secure device-to-device and device-to-cloud communications. Thread networks can adapt to topology changes to avoid single-point failures.

> aside positive
>
> Dive Deeper: For more information, refer to
[Thread Primer](../../guides/thread-primer/index.md).

### What is OpenThread?

[OpenThread released by Google](https://github.com/openthread/openthread) is an open-source implementation of Thread®.

### What is an OpenThread Border Router?

[OpenThread Border Router](https://openthread.io/guides/border-router) (OTBR) released by Google is an open-source implementation of the Thread Border Router.

### IPv6 Multicast

Thread defines a series of features to support multicast across a heterogeneous network (Thread and Wi-Fi/Ethernet Network segments) for multicast addresses with scope larger than realm local.

A Thread Border Router registers its Backbone Router (BBR) Dataset, and the selected BBR service is the Primary Backbone Router (PBBR), which is responsible for the multicast inbounding/outbounding forward.

A Thread Device sends a CoAP message to register the multicast address to the PBBR (Multicast Listener Registration, MLR for short) if the address is larger than realm local. PBBR uses MLDv2 on its External Interface to communicate to the wider IPv6 LAN/WAN about IPv6 multicast groups it needs to listen to, on behalf of its local Thread Network. And PBBR only forwards multicast traffic into the Thread Network if the destination is subscribed to by at least one Thread device.

For Thread Minimal End Devices, they may depend on their parent to aggregate the multicast address and do MLR on their behalf, or register themselves if their parent is of Thread 1.1.

For more details, please refer to **Thread Specification**.

### **What you will build**

In this codelab, you're going to set up a Thread Border Router and two Thread devices, then enable and verify Multicast features on Thread devices and Wi-Fi devices.

### **What you will learn**

* How to build nRF52840 firmware with support for IPv6 Multicast.
* How to subscribe to IPv6 multicast addresses on Thread devices.

### **What you will need**

* A Linux workstation, for building and flashing a Thread RCP, the OpenThread CLI, and testing IPv6 multicast.
* A Raspberry Pi for the Thread border router.
* 2 Nordic Semiconductor nRF52840 USB Dongles (one for the RCP and two for Thread end devices).


## Setup OTBR
Duration: 05:00


The quickest way to set up an OTBR is by following the [OTBR Setup Guide](https://openthread.io/guides/border-router).

After OTBR setup is complete, use [`ot-ctl`](https://openthread.io/guides/border-router/form-network) to validate that the OTBR became the Primary Backbone Router within seconds.

```console
> bbr state
Primary
Done
> bbr
BBR Primary:
server16: 0xF800
seqno:    21
delay:    5 secs
timeout:  3600 secs
Done
```

> aside positive
>
> Dive Deeper:
>
> The BBR dataset shows that the OTBR has become the Primary Backbone Router (PBBR):
>
> * `server16` is the RLOC16 of the PBBR.
> * `seqno` is the randomly generated BBR sequence number. PBBR can change seqno to trigger re-registrations.
> * `delay` is the value of Registration Delay, by which Thread devices should delay before re-registrations.
> * `timeout` is the default duration in seconds for which a Multicast Listener Registration (MLR) remains valid. Thread devices will automatically renew their MLR registrations before expiration if they still subscribe to the multicast address.
>
> Refer to  [OpenThread CLI Reference](https://github.com/openthread/openthread/blob/main/src/cli/README.md#bbr-config-seqno-seqno-delay-delay-timeout-timeout) for more information on BBR dataset configuration.


## Build and Flash Thread devices
Duration: 05:00


Build the Thread CLI application with Multicast and flash the two nRF52840 DK boards.

### **Build nRF52840 DK firmware**

Follow instructions to clone the project and build nRF52840 firmware.

```console
$ cd ~/src/ot-nrf528xx
$ rm -rf build
$ script/build nrf52840 USB_trans -DOT_MLR=ON
```

Continue with the [Build a Thread network with nRF52840 boards and OpenThread codelab](https://openthread.io/codelabs/openthread-hardware#4) as written. After the end device is flashed with the CLI image, follow [Join the second node to the Thread network](https://openthread.io/guides/border-router/form-network#join-the-second-node-to-the-thread-network) to add the Thread device to the Thread network. Repeat for the second Thread end device.


## Subscribe to the IPv6 multicast address
Duration: 02:00


> aside positive
>
> NOTE: This codelab uses a IPv6 multicast address of `ff05::abcd`, but any IPv6 multicast address with scope &gt;= `Admin-local scope (4)` will also work.

**Subscribe to ff05::abcd on nRF52840 End Device 1:**

```console
> ipmaddr add ff05::abcd
Done
```

Verify `ff05::abcd` is successfully subscribed:

```console
> ipmaddr
ff05:0:0:0:0:0:0:abcd            <--- ff05::abcd subscribed
ff33:40:fdde:ad00:beef:0:0:1
ff32:40:fdde:ad00:beef:0:0:1
ff02:0:0:0:0:0:0:2
ff03:0:0:0:0:0:0:2
ff02:0:0:0:0:0:0:1
ff03:0:0:0:0:0:0:1
ff03:0:0:0:0:0:0:fc
Done
```

**Subscribe to ff05::abcd on the Laptop:**

We need a Python script `subscribe6.py` to subscribe to a multicast address on the Laptop.

Copy the code below and save it as `subscribe6.py`:

```
import ctypes
import ctypes.util
import socket
import struct
import sys

libc = ctypes.CDLL(ctypes.util.find_library('c'))
ifname, group = sys.argv[1:]
addrinfo = socket.getaddrinfo(group, None)[0]
assert addrinfo[0] == socket.AF_INET6
s = socket.socket(addrinfo[0], socket.SOCK_DGRAM)
group_bin = socket.inet_pton(addrinfo[0], addrinfo[4][0])
interface_index = libc.if_nametoindex(ifname.encode('ascii'))
mreq = group_bin + struct.pack('@I', interface_index)
s.setsockopt(socket.IPPROTO_IPV6, socket.IPV6_JOIN_GROUP, mreq)
print("Subscribed %s on interface %s." % (group, ifname))
input('Press ENTER to quit.')
```

Run `subscribe6.py` to subscribe `ff05::abcd` on the Wi-Fi network interface (e.g. wlan0):

```console
$ sudo python3 subscribe6.py wlan0 ff05::abcd
Subscribed ff05::abcd on interface wlan0.
Press ENTER to quit.
```

> aside positive
>
> NOTE: If you press ENTER, the subscription ends.

The final network topology with multicast subscriptions are shown below:

<img src="img/b118448c98b2d583.png" alt="b118448c98b2d583.png"  width="624.00" />

Now that we have subscribed the IPv6 multicast address on both the nRF52840 End Device 1 in Thread network and the Laptop in the Wi-Fi network, we are going to verify bi-directional IPv6 multicast reachability in the following sections.


## Verify Inbound IPv6 Multicast
Duration: 02:00


Now, we should be able to reach both nRF52840 End Device 1 in the Thread network and the Laptop using IPv6 multicast address `ff05::abcd` from the Wi-Fi network.

**Ping ff05::abcd on OTBR via the Wi-Fi interface:**

```console
$ ping -6 -b -t 5 -I wlan0 ff05::abcd
PING ff05::abcd(ff05::abcd) from 2401:fa00:41:801:83c1:a67:ae22:5346 wlan0: 56 data bytes
64 bytes from fdb5:8d36:6af9:7669:e43b:8e1b:6f2a:b8fa: icmp_seq=1 ttl=64 time=57.4 ms
64 bytes from 2401:fa00:41:801:8c09:1765:4ba8:48e8: icmp_seq=1 ttl=64 time=84.9 ms (DUP!)
64 bytes from fdb5:8d36:6af9:7669:e43b:8e1b:6f2a:b8fa: icmp_seq=2 ttl=64 time=54.8 ms
64 bytes from 2401:fa00:41:801:8c09:1765:4ba8:48e8: icmp_seq=2 ttl=64 time=319 ms (DUP!)
64 bytes from fdb5:8d36:6af9:7669:e43b:8e1b:6f2a:b8fa: icmp_seq=3 ttl=64 time=57.5 ms
64 bytes from 2401:fa00:41:801:8c09:1765:4ba8:48e8: icmp_seq=3 ttl=64 time=239 ms (DUP!)

# If using MacOS, use this command. The interface is typically not "wlan0" for Mac.
$ ping6 -h 5 -I wlan0 ff05::abcd
```

We can see that OTBR can receive two ping replies from both the nRF52840 End Device 1 and the Laptop because they both have subscribed to `ff05::abcd`. This shows that the OTBR can forward the IPv6 Ping Request multicast packets from the Wi-Fi network to the Thread network.

> aside positive
>
> NOTE: We need to use `-t ttl` to set the IP Time to Live, which is 1 by default on Linux, otherwise the multicast ping can not reach nRF52840 End Device 1.

> aside positive
>
> NOTE: `ip -6 mroute show table all` is a helpful tool to view active multicast groups on a Linux host and their input/output interface(s).


## Verify Outbound IPv6 Multicast
Duration: 02:00


**Ping ff05::abcd on nRF52840 End Device 2:**

```console
> ping ff05::abcd 100 10 1
108 bytes from fdb5:8d36:6af9:7669:e43b:8e1b:6f2a:b8fa: icmp_seq=12 hlim=64 time=297ms
108 bytes from 2401:fa00:41:801:64cb:6305:7c3a:d704: icmp_seq=12 hlim=63 time=432ms
108 bytes from fdb5:8d36:6af9:7669:e43b:8e1b:6f2a:b8fa: icmp_seq=13 hlim=64 time=193ms
108 bytes from 2401:fa00:41:801:64cb:6305:7c3a:d704: icmp_seq=13 hlim=63 time=306ms
108 bytes from fdb5:8d36:6af9:7669:e43b:8e1b:6f2a:b8fa: icmp_seq=14 hlim=64 time=230ms
108 bytes from 2401:fa00:41:801:64cb:6305:7c3a:d704: icmp_seq=14 hlim=63 time=279ms
```

nRF52840 End Device 2 can receive ping replies from both the nRF52840 End Device 1 and the Laptop. This shows that the OTBR can forward the IPv6 Ping Reply multicast packages from the Thread network to the Wi-Fi network.

> aside positive
>
> NOTE: We rely on the OTBR Border Routing to forward unicast ping replies.


## Congratulations
Duration: 01:00


Congratulations, you've successfully set up a Thread Border Router and verified bi-directional IPv6 multicast!

For more on OpenThread, visit  [openthread.io](https://openthread.io/).

Reference docs:

*  [Thread Border Router - Bidirectional IPv6 Connectivity and DNS-Based Service Discovery](https://openthread.io/codelabs/openthread-border-router)
*  [Thread Primer](https://openthread.io/guides/thread-primer)
*  [OpenThread Guide](https://openthread.io/guides)
*  [OpenThread CLI Reference](https://openthread.io/reference/cli/commands)

## License

Copyright (c) 2021, The OpenThread Authors.
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:
1. Redistributions of source code must retain the above copyright
   notice, this list of conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright
   notice, this list of conditions and the following disclaimer in the
   documentation and/or other materials provided with the distribution.
3. Neither the name of the copyright holder nor the
   names of its contributors may be used to endorse or promote products
   derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
POSSIBILITY OF SUCH DAMAGE.
