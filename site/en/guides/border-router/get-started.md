# Get Started with OTBR

Run OTBR with a physical or simulated RCP using the following platforms.

## Docker

The quickest way to get started with OTBR is to try the Docker version. Run OTBR
in a Docker container on any Linux-based system or a Raspberry Pi 3B or newer,
using either a physical or simulated RCP.

See the [Docker Support Overview](docker/index.md) for more
information.

## Codelabs

To set up an OTBR without Docker, try one of our Border Router codelabs. Run
OTBR on a Raspberry Pi 3B or 4, using physical RCPs.

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-border-router/">Border Router
  Codelab</a>

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-border-router-ipv6-multicast">Border Router Thread
  1.2 Multicast Codelab</a>


## Platforms

OTBR also runs directly on a supported platform:

1.  Choose a platform:
    *   [BeagleBone Black](beaglebone-black.md)
    *   [Raspberry Pi 3B or newer](raspberry-pi.md)
1.  [Build and configure OTBR](build.md)
1.  Learn about [tools and scripts included with
    OTBR](tools.md)

## Get the code

To go straight to the source code, see the
[OpenThread Border Router GitHub repository](https://github.com/openthread/ot-br-posix).

You can contribute to the ongoing development of OpenThread Border Router by
submitting bug reports and feature requests to the [Issue
Tracker](https://github.com/openthread/ot-br-posix/issues).

## Community projects

> Note: Projects listed here are not officially supported by the OpenThread team.
The QEMU guide is written for NCP support. For benefits of using an RCP, refer
to the [OpenThread Border Router Overview](index.md).

### QEMU OTBR

A member of the OT community has enabled [OTBR support using
QEMU](https://github.com/ERNE196077/qemu_openthread_borderrouter), an
open-source machine emulator and virtualizer. The project emulates Raspbian on
an ARM architecture.

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
