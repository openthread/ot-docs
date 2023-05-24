# OpenThread Border Router Web GUI

Use the OpenThread Border Router (OTBR) Web GUI to configure and form, join, or
check the status of a Thread network.

## Access the Web GUI

Access the Web GUI by visiting the OTBR's local IPv4 address in a browser window.
(WebGUI uses the standard TCP/IP port designated for HTTP, which is port 80.)
See the [Raspberry Pi IP Address
page](https://www.raspberrypi.org/documentation/remote-access/ip-address.md)
for more information.

<figure>
<img src="../images/otbr-gui-home-full.png" srcset="../images/otbr-gui-home-full.png 1x, ../images/otbr-gui-home-full_2x.png 2x" border="0" class="screenshot" alt="OTBR Web GUI Home" />
</figure>

## Join a Thread network

Use the **Join** menu option to scan for and join an existing Thread network.

## Form a Thread network

Use the **Form** menu option to create a new Thread network.

1.  Replace all default values on the Form page—except for the On-Mesh
    Prefix—to ensure a secure Thread network.
1.  Select **FORM**.
1.  After the network forms, confirm by checking the **Status** menu option.

Next, check the `wpan0` interface for the global IPv6 address with the On-Mesh
Prefix set during network formation:

<pre>
$ ifconfig wpan0
wpan0: flags=4305&lt;UP,POINTOPOINT,RUNNING,NOARP,MULTICAST&gt;  mtu 1280
    inet6 fdde:ad11:11de:0:74d0:6fc9:6be6:3582  prefixlen 64  scopeid 0x0&lt;global&gt;
    inet6 fe80::287f:87ca:f4b3:498a  prefixlen 64  scopeid 0x20&lt;link&gt;
    inet6 fd11:22::287f:87ca:f4b3:498a  prefixlen 64  scopeid 0x0&lt;global&gt;
</pre>

The On-Mesh Prefix global address, for example **fd11:22::287f:87ca:f4b3:498a**, is used by
child nodes to identify the Border Router that is responsible for forwarding traffic out of
the network. The other addresses correspond to the [Mesh-Local and Link-Local IPv6 addresses](../thread-primer/ipv6-addressing.md)
of this node within the Thread network.

## Check the Status

The **Status** menu option displays status information for the Thread network.

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
