# Raspberry Pi

OpenThread Border Router (OTBR) provides support for the [Raspberry Pi
3B or newer](https://www.raspberrypi.org/products) (RPi)
platform.

Hardware requirements:

*   External 5V AC adapter for power
*   A supported [OpenThread platform](https://openthread.io/platforms) for Thread network
    connectivity in an RCP design
*   A microSD card and microSD card reader ("SD card" in this guide)

Access the RPi with an external monitor (via HDMI) and keyboard (via USB), or
connect a [serial cable](../../../guides/border-router/serial-cables.md) from the RPi to
your computer and enable the serial console.

To use RPi with OTBR:

1.  [Download and install the OS](#download-and-install-the-os).
1.  [Build and install OTBR](../../../guides/border-router/build/index.md).
1.  [Set up a Wi-Fi access point](../../../guides/border-router/access-point.md).

## Step 1: Download and install the OS

The RPi does not include any on-board flash memory, and requires an SD card
with a bootable image to operate.

1.  Download the [Raspberry Pi OS Lite
    image](https://www.raspberrypi.org/downloads/raspberry-pi-os/)
    to a local machine with a built-in or external SD card reader.
    *   If using [OTBR Docker](../../../guides/border-router-docker/index.md) on the RPi,
        download [Raspberry Pi OS with
        Desktop](https://www.raspberrypi.org/downloads/raspberry-pi-os/)
        instead.
1.  Install the OS image on an SD card:
    *   [Linux](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)
    *   [Mac OS](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md)
    *   [Windows](https://www.raspberrypi.org/documentation/installation/installing-images/windows.md)
1.  Insert the SD card with the Raspberry OS image into the RPi.
1.  Boot the RPi to go through the OS installation.

For more detailed information on setup and configuration, see the
[Raspberry Pi documentation](https://www.raspberrypi.org/help/).

## Step 2: Build and install OTBR

See [Build and Configuration](../../../guides/border-router/build.md) for instructions on
building and installing OTBR. This includes automatic setup of a Wi-Fi access
point by Network Manager to connect the Thread network to the internet.

## Step 2: Set up a Wi-Fi access point

If automatic setup of the Wi-Fi access point by Network Manager is skipped, see
[Wi-Fi Access Point Setup](../../../guides/border-router/access-point.md) for manual
configuration instructions.

### License

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
