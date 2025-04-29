# OpenThread Border Router Setup

This guide covers the basic build and configuration of OpenThread Border Router
(OTBR). Upon completion of this procedure, you will have an OTBR that functions
as a Full Thread Device (FTD) in a
[radio co-processor (RCP) design](/platforms/co-processor#radio_co-processor_rcp).

## Step 1: What you'll need

* A Raspberry Pi for the Thread border router.
* 2 Nordic Semiconductor nRF52840 USB Dongles (one for the RCP and one for the Thread end device).

## Step 2: Build and flash RCP

OTBR depends on an IEEE 802.15.4 radio to send/receive Thread messages. This guide will focus on using a [Radio Co-Processor](https://openthread.io/platforms#radio-co-processor-rcp) (RCP).

Follow [step 4 of the *Build a Thread network with nRF52840 boards and OpenThread* codelab](https://openthread.io/codelabs/openthread-hardware#3) to build and flash a nRF52840 RCP device.

## Step 3: Prepare Raspberry Pi

1.  Install [Raspberry Pi OS](https://www.raspberrypi.org/downloads/raspberry-pi-os/) on the
    RPi. Both Desktop and Lite versions will work.

1.  Once installed, boot up the RPi and open a terminal window and update the system:
    ```
    $ sudo apt-get update
    $ sudo apt-get upgrade
    ```

## Step 4: Attach the RCP

1.  Attach the RCP device to the Raspberry Pi.

1.  Determine the serial port name for the RCP device by checking `/dev`:
    ```
    $ ls /dev/tty*
    /dev/ttyACMO
    ```

## Step 5: Install OTBR on Raspberry Pi

> Note: The quickest way to set up an OTBR is by using Docker.

To install OTBR using Docker, follow the [OTBR Docker Install guide](https://openthread.io/guides/border-router/build-docker).
    
To install OTBR natively on Linux host, follow the [OTBR Native Install guide](https://openthread.io/guides/border-router/build-native).

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
