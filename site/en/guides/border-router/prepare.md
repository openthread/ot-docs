# OpenThread Border Router Setup

OpenThread Border Router (OTBR) currently offers support for both [Radio Co-processor (RCP)](/platforms/co-processor#radio_co-processor_rcp) and [Network Co-Processor (NCP)](/platforms/co-processor#network_co-processor_ncp) designs. You can select either design for your OTBR.

> Note: The NCP design is currently experimental and limited to Thread 1.3 features.

Upon completion of this procedure, you will have an OTBR that functions
as a Full Thread Device (FTD) in the design you chose.

## Step 1: What you'll need

* A Raspberry Pi for the Thread border router.
* 2 Nordic Semiconductor nRF52840 USB Dongles (one for the Co-Processor and one for the Thread end device).

When building firmware for the nRF52840 USB dongles, you must use the flag `-DOT_BOOTLOADER=USB` to enable the USB DFU trigger.
This flag must be present when building for both the RCP and NCP design. If the flag is not present, the compiled firmware cannot be loaded onto the dongle.

## Step 2: Build and flash Co-Processor firmware

Follow the instructions based on the design you chose.

### RCP design

In RCP design, OTBR depends on an IEEE 802.15.4 radio to send/receive Thread messages.

Follow [step 4 of the *Build a Thread network with nRF52840 boards and OpenThread* codelab](https://openthread.io/codelabs/openthread-hardware#3) to build and flash a nRF52840 RCP device.

### NCP design

In NCP design, the full Thread stack runs on the 802.15.4 radio chip.

Follow the instructions below to build the NCP firmware from the [`ot-nrf528xx`](https://github.com/openthread/ot-nrf528xx) repository you cloned in the previous step:

```
$ script/build nrf52840 USB_trans \
    -DOT_THREAD_VERSION=1.3 \
    -DOT_APP_CLI=OFF \
    -DOT_APP_RCP=OFF \
    -DOT_RCP=OFF \
    -DOT_MTD=OFF \
    -DOT_BORDER_ROUTER=ON \
    -DOT_BORDER_ROUTING=ON \
    -DOT_NCP_INFRA_IF=ON \
    -DOT_SRP_SERVER=ON \
    -DOT_SRP_ADV_PROXY=ON \
    -DOT_PLATFORM_DNSSD=ON \
    -DOT_NCP_DNSSD=ON \
    -DOT_ECDSA=ON \
    -DOT_SERVICE=ON \
    -DOT_BACKBONE_ROUTER=ON \
    -DOT_BACKBONE_ROUTER_MULTICAST_ROUTING=ON \
    -DOT_NCP_CLI_STREAM=ON
```

Then follow the same steps as RCP design to convert the firmware into hex format and flash.

## Step 3: Prepare Raspberry Pi

1.  Install [Raspberry Pi OS](https://www.raspberrypi.org/downloads/raspberry-pi-os/) on the
    RPi. Both Desktop and Lite versions will work.

1.  Once installed, boot up the RPi and open a terminal window and update the system:
    ```
    $ sudo apt-get update
    $ sudo apt-get upgrade
    ```

## Step 4: Attach the Co-Processor

1.  Attach the Co-Processor device to the Raspberry Pi.

1.  Determine the serial port name for the Co-Processor device by checking `/dev`:
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
