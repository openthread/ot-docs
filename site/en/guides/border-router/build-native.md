# Native Install

> Note: This page focuses on running OTBR in a native host
environment. To run OTBR in a Docker environment, follow the [Setup
(Docker) steps](build-docker).

## Step 1: Get OTBR code

On Raspberry Pi:

1.  Install git:
    ```
    $ sudo apt install git
    ```

1.  Clone `ot-br-posix` from GitHub:

    ```
    $ git clone --depth=1 https://github.com/openthread/ot-br-posix
    ```

## Step 2: Build and install OTBR

OTBR has two scripts that bootstrap and set up the Thread Border Router:

```
$ cd ot-br-posix
$ ./script/bootstrap
$ INFRA_IF_NAME=wlan0 ./script/setup
```

OTBR works on both a Thread interface and infrastructure network interface (e.g. Wi-Fi/Ethernet) which is specified with `INFRA_IF_NAME`. The Thread interface is created by OTBR itself and named `wpan0` by default and the infrastructure interface has a default value of `wlan0` if `INFRA_IF_NAME` is not explicitly specified. If your Raspberry Pi is connected by an Ethernet cable, specify the Ethernet interface name (e.g. `eth0`):

```
$ INFRA_IF_NAME=eth0 ./script/setup
```

Verify that the `otbr-agent` service is active:

```
$ sudo service otbr-agent status
● otbr-agent.service - Border Router Agent
   Loaded: loaded (/lib/systemd/system/otbr-agent.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2021-03-01 05:46:26 GMT; 2s ago
 Main PID: 2997 (otbr-agent)
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/otbr-agent.service
           └─2997 /usr/sbin/otbr-agent -I wpan0 -B wlan0 spinel+hdlc+uart:///dev/ttyACM0

Mar 01 05:46:26 raspberrypi otbr-agent[2997]: Stop publishing service
Mar 01 05:46:26 raspberrypi otbr-agent[2997]: [adproxy] Stopped
Mar 01 05:46:26 raspberrypi otbr-agent[2997]: PSKc is not initialized
Mar 01 05:46:26 raspberrypi otbr-agent[2997]: Check if PSKc is initialized: OK
Mar 01 05:46:26 raspberrypi otbr-agent[2997]: Initialize OpenThread Border Router Agent: OK
Mar 01 05:46:26 raspberrypi otbr-agent[2997]: Border router agent started.
Mar 01 05:46:26 raspberrypi otbr-agent[2997]: [INFO]-CORE----: Notifier: StateChanged (0x00038200) [NetData PanId NetName ExtPanId]
Mar 01 05:46:26 raspberrypi otbr-agent[2997]: [INFO]-PLAT----: Host netif is down
```

> Note: If you receive an "InvalidArguments" error, make sure your RCP board is connected as `/dev/ttyACM0`. It happens that the device path may change to `/dev/ttyACM1` after flashing the RCP. Re-plugging the RCP should solve this.

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
