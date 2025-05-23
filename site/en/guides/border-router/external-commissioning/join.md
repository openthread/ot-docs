# Join the Thread Network

After externally commissioning a device using the [OT Commissioner CLI](cli.md),
a Joiner is ready to join the Thread network.

## Join the network

On the Joiner device, start the Thread protocol to automatically join the
network.

```
> thread start
Done
```

Check the state after a few moments to confirm. It may initially start as a
child, but within two minutes it should upgrade to a router.

```
> state
router
Done
```

Also check the device's IPv6 addresses. It should have a Global address using
the On-Mesh Prefix specified during formation of the Thread network through the
OTBR Web GUI.

```
> ipaddr
fdde:ad11:11de:0:0:ff:fe00:9400
fd11:22:0:0:3a15:3211:2723:dbe1 #Global address with on-mesh prefix
fe80:0:0:0:6006:41ca:c822:c337
fdde:ad11:11de:0:ed8c:1681:24c4:3562
```

## Ping the external internet

Test the connectivity between the Joiner device in the Thread network and the
external internet by pinging a public IPv4 address.

```
> ping 8.8.8.8
Pinging synthesized IPv6 address: fd4c:9574:3720:2:0:0:808:808
16 bytes from fd4c:9574:3720:2:0:0:808:808: icmp_seq=15 hlim=119 time=48ms
1 packets transmitted, 1 packets received. Packet loss = 0.0%. Round-trip min/avg/max = 48/48.0/48 ms.
Done
```

> Note: NAT64 needs to be functional in order to be able to ping IPv4 addresses. For more information on NAT64, see the related codelab [Thread Border Router - Provide Internet access via NAT64](https://openthread.io/codelabs/openthread-border-router-nat64).

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
