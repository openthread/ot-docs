# OpenThread Border Router

A Thread Border Router connects a Thread network to other IP-based networks,
such as Wi-Fi or Ethernet. A Thread network requires a Border Router to connect
to other networks.

<figure>
<img src="../images/otbr-arch-borderagent.png" srcset="../images/otbr-arch-borderagent.png 1x, ../images/otbr-arch-borderagent_2x.png 2x" border="0" alt="OTBR Border Agent Architecture" />
</figure>

A Thread Border Router minimally supports the following functions:

*   Bidirectional IP connectivity between Thread and Wi-Fi/Ethernet networks.
*   Bidirectional service discovery via mDNS (on a Wi-Fi/Ethernet link) and SRP
    (on a Thread network).
*   Thread-over-infrastructure that merges Thread partitions over IP-based
    links.
*   External Thread Commissioning (for example, a mobile phone) to authenticate
    and join a Thread device to a Thread network.

<figure class="attempt-right">
  <a href="https://www.threadgroup.org/What-is-Thread#certifiedproducts">
    <img src="../images/ot-thread-certified.png"
         srcset="/images/ot-thread-certified.png 1x, /images/ot-thread-certified_2x.png 2x"
         border="0" alt="Thread Certified" /></a></figure>
  
OpenThread's implementation of a Border Router is called OpenThread Border
Router (OTBR). OTBR is a Thread Certified Component on the [Raspberry Pi
3B](raspberry-pi.md) with a [Nordic
nRF52840](https://openthread.io/vendors/nordic-semiconductor) NCP.

## Get started

### Docker

The quickest way to get started with OTBR is to try the Docker version. Run OTBR
in a Docker container on any Linux-based system or a Raspberry Pi 3B or newer,
using either a physical or simulated RCP.

See the [Docker Support Overview](docker/index.md) for more
information.

### Codelabs

To set up an OTBR without Docker, try one of our Border Router codelabs. Run
OTBR on a Raspberry Pi 3B or 4, using physical RCPs.

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-border-router/">Border Router
  Codelab</a>
  
<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-border-router-ipv6-multicast">Border Router Thread
  1.2 Multicast Codelab</a>


### Platforms

OTBR also runs directly on a supported platform:

1.  Choose a platform:
    *   [BeagleBone Black](beaglebone-black.md)
    *   [Raspberry Pi 3B or newer](raspberry-pi.md)
1.  [Build and configure OTBR](build.md)
1.  Learn about [tools and scripts included with
    OTBR](tools.md)

### Get the code

To go straight to the source code, see the
[OpenThread Border Router GitHub repository](https://github.com/openthread/ot-br-posix).

You can contribute to the ongoing development of OpenThread Border Router by
submitting bug reports and feature requests to the [Issue
Tracker](https://github.com/openthread/ot-br-posix/issues).

### Community projects

**Note:** Projects listed here are not officially supported by the OpenThread team.

#### QEMU OTBR

A member of the OT community has enabled [OTBR support using
QEMU](https://github.com/ERNE196077/qemu_openthread_borderrouter), an
open-source machine emulator and virtualizer. The project emulates Raspbian on
an ARM architecture.

## Features and services

OTBR includes a number of features, including:

*   [Web GUI](https://openthread.io/guides/border-router/web-gui) for configuration and management
*   Thread Border Agent to support [external
    commissioning](https://openthread.io/guides/border-router/external-commissioning)
*   DHCPv6 Prefix Delegation to obtain IPv6 prefixes for a Thread network
*   NAT64 for connecting to IPv4 networks
*   DNS64 to allow Thread devices to initiate communications by name to an
    IPv4-only server
*   Thread interface driver using OpenThread's built-in feature
*   [Docker support](docker/index.md)

### Border Router services

OTBR provides the following services:

*   mDNS Publisher — Allows an External Commissioner to discover an OTBR and its
    associated Thread network
*   [PSKc Generator](tools.md#pskc-generator) — For
    generation of PSKc keys
*   Web Service — Web UI for management of a Thread network

Third-party components for Border Router Services include Simple Web Server and
Material Design Lite for the framework of the web UI.
  
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
