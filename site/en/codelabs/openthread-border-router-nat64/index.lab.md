---
id: openthread-border-router-nat64
summary: In this codelab, you'll build an OpenThread border router with NAT64 support, and use the end-device in the network to access IPv4 only resources from the internet.
status: [final]
authors: Song Guo, Jonathan Hui
categories: Nest
layout: scrolling
feedback_link: https://github.com/orgs/openthread/discussions
keywords: openthread, nat64
project: /_project.yaml
book: /_book.yaml

---

# Thread Border Router - Provide Internet access via NAT64

[Codelab Feedback](https://github.com/orgs/openthread/discussion)


## Introduction



<img src="img/7299534792dd9439.png" alt="7299534792dd9439.png"  width="624.00" />

### What is Thread?

Thread is an IP-based low-power wireless mesh networking protocol that enables secure device-to-device and device-to-cloud communications. Thread networks can adapt to topology changes to avoid single-point failures.

### What is OpenThread?

[OpenThread released by Google](https://github.com/openthread/openthread) is an open-source implementation of Thread®.

### What is an OpenThread Border Router?

[OpenThread Border Router](https://openthread.io/guides/border-router) (OTBR) released by Google is an open-source implementation of the Thread Border Router.

### NAT64

NAT64 is a mechanism that enables hosts in IPv6-only networks to access resources in IPv4 networks. The NAT64 gateway is a translator between IPv4 protocols and IPv6 protocols.

The NAT64 translator, as a part of OpenThread Border Router, supports translating TCP, UDP, and ICMP (ICMPv6) protocols.

### What you'll build

In this codelab, you are going to set up a OpenThread Border Router (OTBR) and a Thread device, then enable and verify communication between Thread devices and IPv4 hosts on the Internet via OpenThread Border Router.

### What you'll learn

* How to build an OpenThread Border Router with NAT64 features.
* How to communicate with IPv4 hosts from Thread end devices.

### What you'll need

* A Linux workstation, for building and flashing a Thread RCP, the OpenThread CLI, and testing IPv4 connectivity.
* A Raspberry Pi for the Thread border router. Your Linux workstation should be reachable over IPv4 from this device.
* 2 Nordic Semiconductor nRF52840 USB Dongles (one for the RCP and one for the Thread end device).

The network topology for this codelab:

<img src="img/c3cd2e081bc052fd.png" alt="c3cd2e081bc052fd.png"  width="624.00" />


## Setup OpenThread Border Router
Duration: 07:00


The quickest way to set up an OTBR is by using Docker following the [OTBR with Docker Guide](https://openthread.io/guides/border-router/docker).

After OTBR setup is complete, use [`ot-ctl`](https://openthread.io/guides/border-router/docker/run#run_ot-ctl) to validate that the NAT64 service is enabled on the border router:

```console
> nat64 state
PrefixManager: Active
Translator: Active
Done
```

A Thread border router publishes the NAT64 prefix in the Thread Network Data:

```console
> netdata show
Prefixes:
fd16:a3d:e170:1::/64 paros low f800
Routes:
::/0 s med f800
fd16:a3d:e170:2:0:0::/96 sn low f800
Services:
44970 5d fd4db3e59738319339c4ee02ca9e2b1dd120 s f800 0
Contexts:
fd16:a3d:e170:1::/64 1 sc
Commissioning:
60365 - - -
Done
```

The NAT64 prefix shows up as a route entry with the `n` flag. In the example above, `fd16:a3d:e170:2:0:0::/96` is the NAT64 prefix.

The NAT64 prefix will be used by Thread devices when communicating with an IPv4 host.

> aside negative
> 
> **Note:** OpenThread does not use the NAT64 well-known prefix `64:ff9b::/96`. Instead, Thread Border Routers publish their dynamically generated NAT64 prefix used by the NAT64 translator in the Thread Network Data (as seen above, the `n` stands for NAT64). Thread End Devices must obtain this NAT64 prefix from the Thread Network Data and synthesize their own IPv6 addresses, see [`otNat64SynthesizeIp6Address()`](https://openthread.io/reference/group/api-nat64#otnat64synthesizeip6address). This is automatically done in the CLI, but needs to be specifically implemented in custom applications.


## Setup Thread end device
Duration: 05:00


Follow the  [Set up the FTDs step of the Build a Thread network with nRF52840 boards and OpenThread codelab](https://openthread.io/codelabs/openthread-hardware#4) to build and flash a nRF52840 CLI end device, with a change to the following step:

In **Build and flash**, you have to append `-DOT_DNS_CLIENT=ON`, `-DOT_SRP_CLIENT=ON` and `-DOT_ECDSA=ON` to the command line when calling `script/build`:

```console
$ cd ~/src/ot-nrf528xx
$ rm -rf build
$ script/build nrf52840 USB_trans -DOT_DNS_CLIENT=ON -DOT_SRP_CLIENT=ON -DOT_ECDSA=ON
```

Continue with the [Build a Thread network with nRF52840 boards and OpenThread codelab](https://openthread.io/codelabs/openthread-hardware#4) as written. After the end device is flashed with the CLI image, follow [Join the second node to the Thread network](https://openthread.io/guides/border-router/docker/test-connectivity#join-the-second-node-to-the-thread-network) to add the Thread device to the Thread network.

Wait for a few seconds after setting up the Thread end device and verify if joining the Thread network is successful. As above, you can view the published NAT64 prefix in the Thread Network Data.

```console
> netdata show
Prefixes:
fd16:a3d:e170:1::/64 paros low f800
Routes:
::/0 s med f800
fd16:a3d:e170:2:0:0::/96 sn low f800
Services:
44970 5d fd4db3e59738319339c4ee02ca9e2b1dd120 s f800 0
Contexts:
fd16:a3d:e170:1::/64 1 sc
Commissioning:
60365 - - -
Done
```

Make sure that the network data matches the one from OTBR.


## Communicate with IPv4 hosts from the Thread end device
Duration: 05:00


You can now communicate with hosts on the IPv4 network from the end device we just set up.

### Send ICMP echo requests to IPv4 hosts

From the CLI of our Thread end device:

```console
> ping 8.8.8.8
Pinging synthesized IPv6 address: fd16:a3d:e170:2:0:0:808:808
16 bytes from fd16:a3d:e170:2:0:0:808:808: icmp_seq=1 hlim=109 time=28ms
1 packets transmitted, 1 packets received. Packet loss = 0.0%. Round-trip min/avg/max = 28/28.0/28 ms.
Done
```

The border router creates a NAT64 mapping item for this device by the `nat64 mappings` command:

```console
> nat64 mappings
|                  | Address                                                     | Ports or ICMP Ids |        | 4 to 6                  | 6 to 4                  |
+------------------+-------------------------------------------------------------+-------------------+--------+-------------------------+-------------------------+
| ID               | IPv6                                     | IPv4             | v6      | v4      | Expiry | Pkts     | Bytes        | Pkts     | Bytes        |
+------------------+------------------------------------------+------------------+---------+---------+--------+----------+--------------+----------+--------------+
| 90b156e3cf609a23 |      fd16:a3d:e170:1:492d:bcdb:9f72:6297 |  192.168.255.254 |   N/A   |   N/A   |  7162s |        1 |           16 |        1 |           16 |
|                  |                                                                                      TCP |        0 |            0 |        0 |            0 |
|                  |                                                                                      UDP |        0 |            0 |        0 |            0 |
|                  |                                                                                     ICMP |        1 |           16 |        1 |           16 |
Done
```

The `fd16:a3d:e170:1:492d:bcdb:9f72:6297` should be the IPv6 address of your Thread device.

> aside negative
> 
> **Note:**  The **IPv4** field is the temporary IPv4 address allocated to the Thread device for sending packets. This address is not meaningful for other hosts.

Run this command on the border router at any time to see how it counts the traffic.

### Send DNS queries to IPv4 DNS servers

Use `dns resolve4` to resolve a hostname on the IPv4 network. The DNS server address can also be an IPv4 address:

```console
> dns resolve4 example.com 8.8.8.8
Synthesized IPv6 DNS server address: fd16:a3d:e170:2:0:0:808:808
DNS response for example.com. - fd16:a3d:e170:2:0:0:17c0:e454 TTL:295 fd16:a3d:e170:2:0:0:17d7:88 TTL:295 fd16:a3d:e170:2:0:0:17d7:8a TTL:295 fd16:a3d:e170:2:0:0:6007:80af TTL:295 fd16:a3d:e170:2:0:0:6007:80c6 TTL:295 fd16:a3d:e170:2:0:0:17c0:e450 TTL:295 
Done
```

> aside positive
> 
> **Note:** The DNS response for `resolve4` command is a set of synthesized IPv6 addresses.

### Communicate via TCP

It is possible to establish TCP connections between the end device and hosts in the IPv4 network.

Assume the IP address of your Linux IPv4 host is `192.168.0.2`.

On your Linux IPv4 host, use `nc` to listen for TCP connections:

```console
$ nc -l 0.0.0.0 12345
```

From your Thread end device, establish a TCP connection and send messages to your Linux IPv4 host:

```console
> tcp init
Done
> tcp connect 192.168.0.2 12345
Connecting to synthesized IPv6 address: fd16:a3d:e170:2:0:0:c0a8:2
Done
> tcp send hello
```

Your Linux IPv4 host outputs:

```console
hello
```

You can also send messages from your Linux IPv4 host to Thread end device. Type "world" and press Enter on your Linux IPv4 host running `nc`, and your Thread end device outputs:

```console
TCP: Received 6 bytes: world
```

### Communicate via UDP

It is possible to communicate using UDP between Thread devices and hosts in the IPv4 network.

Assume the IP address of your Linux IPv4 host is `192.168.0.2`.

Use `nc` to listen for UDP connections:

```console
$ nc -u -l 0.0.0.0 12345
```

From your Thread end device, establish a UDP connection and send messages to your Linux IPv4 host:

```console
> udp open
Done
> udp connect 192.168.0.2 12345
Connecting to synthesized IPv6 address: fd16:a3d:e170:2:0:0:c0a8:2
Done
> udp send hello
Done
```

Your Linux IPv4 host outputs:

```console
hello
```

You can also send messages from your Linux IPv4 host to Thread end device. Type "world" and press Enter on your Linux IPv4 host running `nc`, and your Thread end device outputs:

```console
6 bytes from fd16:a3d:e170:2:0:0:c0a8:2 12345 world
```


## Toggle NAT64 on Border Router
Duration: 03:00


You can enable or disable NAT64 any time you want. Use `nat64 disable` to disable NAT64. And use `nat64 state` to check the state of NAT64.

```console
> nat64 disable
Done
> nat64 state
PrefixManager: Disabled
Translator: Disabled
Done
```

> aside positive
> 
> **Note:** You can find the means of the states from the  [OpenThread CLI reference](https://openthread.io/reference/cli/commands#nat64_state).

After disabling, the device is no longer publishing a NAT64 prefix:

```console
> netdata show
Prefixes:
fd16:a3d:e170:1::/64 paros low f800
Routes:
::/0 s med f800
Services:
44970 5d fd4db3e59738319339c4ee02ca9e2b1dd120 s f800 0
Contexts:
fd16:a3d:e170:1::/64 1 sc
Commissioning:
60365 - - -
Done
```

Also the devices in the Thread network can no longer access the IPv4 host via this border router.

From the CLI of our Thread end device:

```console
> ping 8.8.8.8
Error 13: InvalidState
```

Use `nat64 enable` to enable NAT64. It may take a while before the prefix manager starts advertising a NAT64 prefix:

```console
> nat64 enable
Done
> nat64 state
PrefixManager: Idle
Translator: NotWorking
Done
```

After a few seconds, the NAT64 components should be up and running:

```console
> nat64 state
PrefixManager: Active
Translator: Active
Done
> netdata show
Prefixes:
fd16:a3d:e170:1::/64 paros low f800
Routes:
::/0 s med f800
fd16:a3d:e170:2:0:0::/96 sn low f800
Services:
44970 5d fd4db3e59738319339c4ee02ca9e2b1dd120 s f800 0
Contexts:
fd16:a3d:e170:1::/64 1 sc
Commissioning:
60365 - - -
Done
```

Note that disabling NAT64 will clear the mapping table:

```console
> nat64 mappings
|                  | Address                                                     | Ports or ICMP Ids |        | 4 to 6                  | 6 to 4                  |
+------------------+-------------------------------------------------------------+-------------------+--------+-------------------------+-------------------------+
| ID               | IPv6                                     | IPv4             | v6      | v4      | Expiry | Pkts     | Bytes        | Pkts     | Bytes        |
+------------------+------------------------------------------+------------------+---------+---------+--------+----------+--------------+----------+--------------+
Done
```

## Forward DNS queries to upstream DNS servers
Duration: 03:00


When NAT64 is enabled on the border router, OpenThread will try to forward the DNS queries for internet domains to upstream DNS servers.

On your end device, ensure the default DNS server is the border router:

```console
> dns config
Server: [fd4d:b3e5:9738:3193:39c4:ee02:ca9e:2b1d]:53
ResponseTimeout: 6000 ms
MaxTxAttempts: 3
RecursionDesired: yes
ServiceMode: srv_txt_opt
Nat64Mode: allow
Done
```

The server IPv6 address (`fd4d:b3e5:9738:3193:39c4:ee02:ca9e:2b1d` in the example above), should be one of the addresses of your OpenThread Border Router.

Now you can send DNS queries for internet domains from the end device:

```console
> dns resolve example.com
DNS response for example.com. - 2600:1406:3a00:21:0:0:173e:2e65 TTL:161 2600:1406:3a00:21:0:0:173e:2e66 TTL:161 2600:1406:bc00:53:0:0:b81e:94c8 TTL:161 2600:1406:bc00:53:0:0:b81e:94ce TTL:161 2600:1408:ec00:36:0:0:1736:7f24 TTL:161 2600:1408:ec00:36:0:0:1736:7f31 TTL:161 
Done
> dns resolve4 example.com
DNS response for example.com. - fd16:a3d:e170:2:0:0:6007:80af TTL:300 fd16:a3d:e170:2:0:0:6007:80c6 TTL:300 fd16:a3d:e170:2:0:0:17c0:e450 TTL:300 fd16:a3d:e170:2:0:0:17c0:e454 TTL:300 fd16:a3d:e170:2:0:0:17d7:88 TTL:300 fd16:a3d:e170:2:0:0:17d7:8a TTL:300 
Done
```

> aside positive
> 
> **Note:** The A records in responses will be synthesized into IPv6 addresses using the NAT64 prefix in the network.


## Congratulations



Congratulations, you've successfully set up a border router with NAT64 support and used it to provide internet access to Thread end devices! 

### Further reading

*  [OpenThread Guides](https://openthread.io/guides)
*  [OpenThread CLI Reference](https://openthread.io/reference/cli/commands#nat64_enabledisable)
*  [OpenThread API Reference for NAT64](https://openthread.io/reference/group/api-nat64)
*  [OpenThread API Reference for upstream DNS](https://openthread.io/reference/group/api-dnssd-server#otdnssdupstreamqueryisenabled)
*  [OpenThread platform abstraction for DNS](https://openthread.io/reference/group/plat-dns)

### Reference docs

*  [RFC 6146: Stateful NAT64: Network Address and Protocol Translation from IPv6 Clients to IPv4 Servers](https://datatracker.ietf.org/doc/html/rfc6146)
