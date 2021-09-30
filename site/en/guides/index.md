# Get Started

## Learn about Thread

Are you new to Thread<sup>®</sup>? Or simply need to refresh your knowledge?
Check out our [Thread Primer](https://github.com/openthread/ot-docs/blob/main/site/en/guides/thread-primer/index.md), which covers all the
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
   href="https://github.com/openthread/ot-docs/blob/main/site/en/codelabs/openthread-simulation/index.lab.md">Try the Simulation Codelab with
  Docker</a>

### Simulation Codelab with build toolchain

An alternate version of the Docker Simulation Codelab, where instead of using
Docker, you set up the OpenThread build toolchain and build OpenThread directly
on a Mac or Linux machine.

<a class="button button-primary"
   href="https://github.com/openthread/ot-docs/blob/main/site/en/codelabs/openthread-simulation-posix/index.lab.md">Try the Simulation Codelab
  with build toolchain</a>

### Hardware Codelabs

Dive right into hardware, where you will learn how to:

*   Flash OpenThread on Nordic nRF52840 or Silicon Labs EFR32 development boards
*   Build a real Thread network
*   Authenticate Thread nodes with Commissioning
*   Use the OpenThread CLI for Multicast and UDP (Nordic only)

<a class="button button-primary"
   href="https://github.com/openthread/ot-docs/blob/main/site/en/codelabs/openthread-hardware/index.lab.md">Try the Nordic Hardware Codelab</a>

<a class="button button-primary"
   href="https://github.com/openthread/ot-docs/blob/main/site/en/codelabs/silabs-openthread-hardware/index.lab.md">Try the Silicon Labs Hardware
    Codelab</a>

### API Codelab

Want to use OpenThread APIs in an application? Using real hardware, learn how
to:

*   Program the buttons and LEDs on Nordic nRF52840 development boards
*   Use common OpenThread APIs and the `otInstance` class
*   Monitor and react to OpenThread state changes
*   Send UDP messages to all devices in a Thread network

<a class="button button-primary"
   href="https://github.com/openthread/ot-docs/blob/main/site/en/codelabs/openthread-apis/index.lab.md">Try the API Codelab</a>

### Network Simulator Codelab

OpenThread Network Simulator (OTNS) allows you to visualize and operate a
simulated Thread network, using a CLI and web interface. With a Mac or Linux
machine, learn how to:

*   Install OTNS and build OpenThread for OTNS
*   Use OTNS-Web to manage a Thread network and visualize activity in a web
    browser
*   Use OTNS-CLI to further control the simulation

<a class="button button-primary"
   href="https://github.com/openthread/ot-docs/blob/main/site/en/codelabs/openthread-network-simulator/index.lab.md">Try the Network Simulator
  Codelab</a>

### Testing and Visualization Codelab

OTNS can be used with [Silk]({{ github_repo_silk }}), a fully automated test
platform for validating OpenThread function, feature, and system performance
with real devices. With a Mac or Linux machine, learn how to:

*   Build OpenThread for real devices with the OTNS feature enabled
*   Use OTNS-Web to monitor the status of the Thread network formed by running
    Silk test cases

<a class="button button-primary"
   href="/codelabs/openthread-testing-visualization/">Try the Testing and
  Visualization Codelab</a>

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
   href="/codelabs/openthread-border-router/">Try the Border Router
  Codelab</a>

### Border Router Thread 1.2 Multicast Codelab

Thread 1.2 introduces Multicast across Thread Networks, allowing multicast
communication between Thread network and Infrastructure (Wi-Fi/ethernet)
network segments. With a Mac or Linux machine and a Raspberry Pi, learn how to:

*   Build nRF52840 firmware with Thread 1.2 Multicast features
*   Subscribe to IPv6 multicast addresses on Thread devices

<a class="button button-primary"
   href="/codelabs/openthread-border-router-ipv6-multicast">Try the Border
  Router Thread 1.2 Multicast Codelab</a>

### Border Router with Docker

You can also run OTBR in a Docker container on any Linux-based machine.

<a class="button button-primary" href="/guides/border-router/docker">Try the
  OTBR Docker guide</a>

## Get the code

Already know what you're doing and want to get started with the code? Visit the
[OpenThread GitHub site]({{ github_org }}), where you can
find the OpenThread repository, along with other support repositories, like
OpenThread Border Router, OpenThread RTOS, and OpenThread Commissioner.

## Platform support

OpenThread has been ported to several devices and platforms by both the
OpenThread team, silicon vendors, and the community.

See the list of vendor-supported platforms at [Vendor Support](/vendors).

Learn more about the system architecture and platform designs on the
[Platforms](/platforms/) overview.

## Docker support

Docker images for use with OpenThread are available on [Docker
Hub](https://hub.docker.com/u/openthread/). These images are created and tested
by the OpenThread team, and are an easy way to get started with OpenThread
without having to go through toolchain and system configuration.

## User guides

Need help with a specific task or feature? Our guides can help.

Category | Contents
----- | -----
[Build](/guides/build/) | How to build and configure OpenThread and enable enhanced features
[Porting](/guides/porting/) | How to port OpenThread to a new hardware platform
[Border Router](/guides/border-router/) | How to connect your OpenThread network to other IPv6 networks with a Border Router, or use external Thread commissioning
[Commissioner](/guides/commissioner) | How to build and use OT Commissioner to commission devices onto a Thread network
[Pyspinel](/guides/pyspinel/) | How to use Pyspinel to build a Thread packet sniffer.
[Certification](/certification/) | How to test your platform against all certification test cases

## Application APIs

Developing an application to run on top of OpenThread? Try our [Developing with
OpenThread APIs Codelab]({{ codelabs }}/openthread-apis/) to
learn the basics, or dig into the [API Reference](/reference/) documentation to
see what OpenThread services are available.

## Testing and certification

Learn how we test OpenThread and what user testing tools are available on our
[Testing](/testing/) page, and browse the latest OpenThread peformance quality
metrics on our [Quality Dashboards](/testing/quality-dashboard).

If you're interested in Thread Certification for your product or component, see
the [Certification](/certification/) page.

## Get help or contribute

Have a question about OpenThread? Want to contribute to its ongoing development?
Our [Resources](/resources/) page explains all the ways to get help, or to
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
