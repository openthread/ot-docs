# mDNS Discovery

OTBR registers a `_meshcop._udp` service named `OpenThread BorderRouter`. This
service is configured in the [OTBR CMakeLists.txt file](https://github.com/openthread/ot-br-posix/blob/main/CMakeLists.txt#L36).

There are several ways to discover your Thread network.

## DNS Service Discovery (DNS-SD)

Use `dns-sd` to browse for a service instances of type `_meshcop._udp.`:

```
$ dns-sd -B _meshcop._udp local
```

<pre>
Browsing for _meshcop._udp.local
DATE: ---Tue 16 Nov 2021---
13:31:03.197  ...STARTING...
Timestamp     A/R    Flags  if Domain               Service Type         Instance Name
13:31:03.198  Add        2   6 local.               _meshcop._udp.       OpenThread BorderRouter (#3991)
</pre>

Use `dns-sd` to resolve the service instance:

```
$ dns-sd -L "OpenThread BorderRouter (#3991)" _meshcop._udp local
```

<pre>
Lookup OpenThread BorderRouter #(3991)._meshcop._udp.local
DATE: ---Tue 16 Nov 2021---
13:33:05.197  ...STARTING...
13:33:05.350  OpenThread\032BorderRouter\032(#3991)._meshcop._udp.local. can be reached at raspberrypi.local.:49155 (interface 3)
</pre>

Use `dns-sd` to resolve the hostname:

```
$ dns-sd -G v4/v6 {raspberrypi.local}
```

<pre>
DATE: ---Tue 16 Nov 2021---
14:21:29.485  ...STARTING...
Timestamp     A/R Flags if Hostname           Address                                      TTL
14:21:29.486  Add     3  3 raspberrypi.local. FDDE:AD11:11DE:0000:74D0:6FC9:6BE6:3582%&lt;0&gt;  120
14:21:29.486  Add     3  3 raspberrypi.local. FD00:0000:0000:0000:74D0:6FC9:6BE6:3582%&lt;0&gt;  120
14:21:29.486  Add     3  3 raspberrypi.local. FE80:0000:0000:0000:74D0:6FC9:6BE6:3582%eth0 120
14:21:29.486  Add     3  3 raspberrypi.local. FE80:0000:0000:0000:287F:87CA:F4B3:498A%eth0 120
14:21:29.486  Add     2  3 raspberrypi.local. 192.168.0.10                                 120
</pre>

Ping your IP address. From the `dns-sd` results, choose an address that's
reachable from your network, for example the global `<0>` address
`FD00::74D0:6FC9:6BE6:3582`:

```
$ ping -6 FD00::74D0:6FC9:6BE6:3582

```

<pre>
PING FD00::74D0:6FC9:6BE6:3582(fd00::74d0:6fc9:6be6:3582) 56 data bytes
64 bytes from fd00::74d0:6fc9:6be6:3582: icmp_seq=1 ttl=64 time=27.1 ms
64 bytes from fd00::74d0:6fc9:6be6:3582: icmp_seq=2 ttl=64 time=3.18 ms
64 bytes from fd00::74d0:6fc9:6be6:3582: icmp_seq=3 ttl=64 time=2.76 ms
</pre>

## Avahi utilities

Install `avahi-daemon` and `avahi-utils`:

```
$ sudo apt-get install -y avahi-daemon avahi-utils
```

Start the `avahi-daemon`:

```
$ sudo service avahi-daemon start
```

Use `avahi-browse`:

```
$ avahi-browse -r -t _meshcop._udp
```

<pre>
+ eth0 IPv6 OpenThread BorderRouter (#3991)   _meshcop._udp        local
= eth0 IPv6 OpenThread BorderRouter (#3991)   _meshcop._udp        local
   hostname = [raspberrypi.local]
   address = [192.168.0.10]
   port = [49155]
   txt = []
</pre>

## mDNS apps

Search [Google Play](https://play.google.com/store/search?q=mDNS%20discovery&c=apps) for mDNS
discovery apps, for example:

* [Service Browser](https://play.google.com/store/apps/details?id=com.druk.servicebrowser) for Android
* [Discovery - DNS-SD Browser](https://apps.apple.com/app/discovery-dns-sd-browser/id305441017) for iOS

> Note: Make sure that your mobile device is on the same network as your Thread
network. If you have issues with service discovery and you're on Wi-Fi 5G, try
switching to Wi-Fi 2.4GHz.

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
