
# Get Started

## Learn about Thread

Are you new to Thread<sup>®</sup>? Or simply need to refresh your knowledge?
Check out our [Thread Primer](thread-primer/index.md), which covers all the
basics of Thread and how it works.

## Try OpenThread

Want to see what OpenThread released by Google is all about? The quickest way to
do so is to run through one of our Codelabs or Guides.

### Simulation Codelab with Docker

Try OpenThread without the need for test hardware. Using Docker on a Mac or
Linux machine, learn how to:

*   Simulate a Thread network
*   Authenticate Thread nodes with Commissioning
*   Use OpenThread Daemon to manage a simulated Thread network featuring an RCP

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-simulation/">Try the Simulation Codelab with
  Docker</a>

### Simulation Codelab with build toolchain

An alternate version of the Docker Simulation Codelab, where instead of using
Docker, you set up the OpenThread build toolchain and build OpenThread directly
on a Mac or Linux machine.

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-simulation-posix/">Try the Simulation Codelab
  with build toolchain</a>

### Hardware Codelabs

Dive right into hardware, where you will learn how to:

*   Flash OpenThread on Nordic nRF52840 or Silicon Labs EFR32 development boards
*   Build a real Thread network
*   Authenticate Thread nodes with Commissioning
*   Use the OpenThread CLI for Multicast and UDP (Nordic only)

<a class="button button-primary"
   href="https://openthread.io/codelabs/esp-openthread-hardware/">Try the Espressif Hardware Codelab</a>

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-hardware/">Try the Nordic Hardware Codelab</a>

<a class="button button-primary"
   href="https://openthread.io/codelabs/silabs-openthread-hardware/">Try the Silicon Labs Hardware
    Codelab</a>

<a class="button button-primary"
   href="https://openthread.io/codelabs/telink-openthread-hardware/">Try the Telink Hardware Codelab</a>

### API Codelab

Want to use OpenThread APIs in an application? Using real hardware, learn how
to:

*   Program the buttons and LEDs on Nordic nRF52840 development boards
*   Use common OpenThread APIs and the `otInstance` class
*   Monitor and react to OpenThread state changes
*   Send UDP messages to all devices in a Thread network

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-apis/">Try the API Codelab</a>

### Network Simulator Codelab

OpenThread Network Simulator (OTNS) allows you to visualize and operate a
simulated Thread network, using a CLI and web interface. With a Mac or Linux
machine, learn how to:

*   Install OTNS and build OpenThread for OTNS
*   Use OTNS-Web to manage a Thread network and visualize activity in a web
    browser
*   Use OTNS-CLI to further control the simulation

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-network-simulator/">Try the Network Simulator
  Codelab</a>

### Border Router Codelab

A Thread Border Router connects a Thread network to other IP-based networks,
such as Wi-Fi or Ethernet. A Thread network requires a Border Router to connect
to other networks. OpenThread Border Router (OTBR) is an open-source
implementation of a Thread Border Router.

With a Mac or Linux machine, learn how to:

*   Set up OTBR and form a Thread network
*   Build an OpenThread CLI device with the SRP feature
*   Register a service with SRP
*   Discover and reach a Thread end device

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-border-router/">Try the Border Router
  Codelab</a>

### Border Router IPv6 Multicast Codelab

Thread supports IPv6 Multicast across Thread Networks, allowing multicast
communication between Thread network and Infrastructure (Wi-Fi/ethernet)
network segments. With a Mac or Linux machine and a Raspberry Pi, learn how to:

*   Build nRF52840 firmware with IPv6 Multicast features
*   Subscribe to IPv6 multicast addresses on Thread devices

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-border-router-ipv6-multicast/">Try the Border
  Router IPv6 Multicast Codelab</a>

### Border Router NAT64 Codelab

NAT64 is a mechanism that enables hosts in IPv6-only networks to access
resources in IPv4 networks. The NAT64 gateway is a translator between IPv4
protocols and IPv6 protocols. With a Mac or Linux machine and a Raspberry Pi,
and building off the Border Router Codelab, learn how to:

*   Build an OpenThread Border Router with NAT64 features
*   Communicate with IPv4 hosts from Thread end devices

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-border-router-nat64/">Try the Border
  Router NAT64 Codelab</a>

### Border Router with Docker

You can also run OTBR in a Docker container on any Linux-based machine.

<a class="button button-primary" href="border-router/docker/index.md">Try the
  OTBR Docker guide</a>

## Get the code

Already know what you're doing and want to get started with the code? Visit the
[OpenThread GitHub site](https://github.com/openthread/openthread), where you can
find the OpenThread repository, along with other support repositories, like
OpenThread Border Router, OpenThread RTOS, and OpenThread Commissioner.

## Platform support

OpenThread has been ported to several devices and platforms by both the
OpenThread team, silicon vendors, and the community.

See the list of vendor-supported platforms at [Vendor Support](https://openthread.io/vendors).

Learn more about the system architecture and platform designs on the
[Platforms](https://openthread.io/platforms/) overview.

## Docker support

Docker images for use with OpenThread are available on [Docker
Hub](https://hub.docker.com/u/openthread/). These images are created and tested
by the OpenThread team, and are an easy way to get started with OpenThread
without having to go through toolchain and system configuration.

## User guides

Need help with a specific task or feature? Our guides can help.

Category | Contents
----- | -----
[Build](build/index.md) | How to build and configure OpenThread and enable enhanced features
[Porting](porting/index.md) | How to port OpenThread to a new hardware platform
[Border Router](border-router/index.md) | How to connect your OpenThread network to other IPv6 networks with a Border Router, or use external Thread commissioning
[Commissioner](commissioner/index.md) | How to build and use OT Commissioner to commission devices onto a Thread network
[Pyspinel](pyspinel/index.md) | How to use Pyspinel to build a Thread packet sniffer.
[Certification](https://openthread.io/certification/) | How to test your platform against all certification test cases

## Application APIs

Developing an application to run on top of OpenThread? Try our [Developing with
OpenThread APIs Codelab](https://openthread.io/codelabs/openthread-apis/) to
learn the basics, or dig into the [API Reference](https://openthread.io/reference/) documentation to
see what OpenThread services are available.

## Testing and certification

Learn how we test OpenThread and what user testing tools are available on our
[Testing](https://openthread.io/testing/) page.

If you're interested in Thread Certification for your product or component, see
the [Certification](https://openthread.io/certification/) page.

## Get help or contribute

Have a question about OpenThread? Want to contribute to its ongoing development?
Our [Resources](https://openthread.io/resources/) page explains all the ways to get help, or to
help out.

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
