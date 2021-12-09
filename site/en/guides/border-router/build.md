# OpenThread Border Router Build and Configuration

This guide covers the basic build and configuration of OpenThread Border Router
(OTBR). Upon completion of this procedure, you will have an OTBR that functions
as a Full Thread Device (FTD) in an RCP design.

## Step 1: Configure Platform

Configure a supported hardware platform:

*   [BeagleBone Black](beaglebone-black.md)
*   [Raspberry Pi](raspberry-pi.md)

## Step 2: Build and flash RCP

OTBR runs on an RCP design. Select a [supported OpenThread
platform](https://openthread.io/platforms/) to use as an RCP and follow the building and flashing
instructions for that platform.

For an overview of building OpenThread, see the
[Building Guide](../build/index.md).

Specific instructions on building supported platforms with GNU Autotools can be
found in each example's
[platform folder](https://github.com/openthread/openthread/tree/master/examples/platforms).

## Step 3: Install OTBR

> Warning: Before you continue, make sure your configured hardware platform
is connected to the internet using Ethernet. The `bootstrap` script disables
the platform's Wi-Fi interface and the `setup` script requires internet
connectivity to download and install several packages.

OTBR communicates with the RCP via spinel. To install OTBR on the configured
hardware platform, complete the following steps.

Clone the OTBR repository:

```
$ git clone https://github.com/openthread/ot-br-posix
```

Install dependencies. For a complete list of OTBR default
flags, refer to [platform examples on GitHub](https://github.com/openthread/ot-br-posix/tree/main/examples/platforms).
Select your platform, then click `default`, if available.

To use default flags:

```
$ cd ot-br-posix
$ ./script/bootstrap
```

To use Network Manager with [BeagleBone Black](beaglebone-black.md) (optional):

```
$ cd ot-br-posix
$ NETWORK_MANAGER=1 NETWORK_MANAGER_WIFI=1 ./script/bootstrap
```

Compile and install OTBR. Note that the setup script enables Border Routing
by default. To enable Border Routing, specify your platform's Ethernet or
Wi-Fi interface. If you added or modified flags for the `bootstrap` script,
you'll need to make the same changes here when you run the setup script.

To use Ethernet:

```
$ INFRA_IF_NAME=eth0 ./script/setup
```

To use Wi-Fi:

```
$ INFRA_IF_NAME=wlan0 ./script/setup
```

To use Network Manager for [BeagleBone Black](beaglebone-black.md) (optional):

```
$ NETWORK_MANAGER=1 NETWORK_MANAGER_WIFI=1 INFRA_IF_NAME=wlan0 ./script/setup
```

## Step 4: Attach and configure RCP device

Attach the flashed RCP device to the Border Router platform via USB.

> Caution: The Border Router with the RCP device attached must use an external
AC adapter of the proper voltage. Do not power the Border Router from a USB Hub
or a computer's USB port.

To configure the RCP device's serial port in `otbr-agent`, first, determine the
serial port name for the RCP device by checking `/dev`:

```
$ ls /dev/tty*
```

Next, append this to `/etc/default/otbr-agent`. For example, for a serial port
name of `ttyUSB0`:

```
OTBR_AGENT_OPTS="-I wpan0 spinel+hdlc+uart:///dev/ttyUSB0"
```

> Note: Not all devices attach with the same serial port name. The most
common port names are `ttyACM*` and `ttyUSB*`. See the documentation for
your device to determine the expected serial port name.

Power cycle the Border Router. If using the BeagleBone Black platform,
remember to [hold down the BOOT button](beaglebone-black.md) while
doing so.

The OTBR service should start on boot.

## Step 5: Verify services

Verify that all required services are enabled:

```
$ sudo systemctl status
```

If the `setup` script was successful, the following services appear in the
output:

*   `avahi-daemon.service`
*   `otbr-agent.service`
*   `otbr-web.service`

For example:

```
● raspberrypi
    State: running
     Jobs: 0 queued
   Failed: 0 units
    Since: Thu 1970-01-01 00:00:01 UTC; 47 years 7 months ago
   CGroup: /
           ├─user.slice
           │ └─user-1000.slice
           │   ├─user@1000.service
           │   │ └─init.scope
           │   │   ├─576 /lib/systemd/systemd --user
           │   │   └─580 (sd-pam)
           │   └─session-c1.scope
           │     ├─480 /bin/login --
           │     └─585 -bash
           ├─init.scope
           │ └─1 /sbin/init
           └─system.slice
             ├─systemd-timesyncd.service
             │ └─334 /lib/systemd/systemd-timesyncd
             ├─dbus.service
             │ └─339 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation
             ├─hciuart.service
             │ └─442 /usr/bin/hciattach /dev/serial1 bcm43xx 921600 noflow -
             ├─ssh.service
             │ └─621 /usr/sbin/sshd -D
  # enabled  ├─avahi-daemon.service
             │ ├─341 avahi-daemon: running [raspberrypi.local]
             │ └─361 avahi-daemon: chroot helper
  # enabled  ├─otbr-web.service
             │ └─472 /usr/sbin/otbr-web
             ├─triggerhappy.service
             │ └─354 /usr/sbin/thd --triggers /etc/triggerhappy/triggers.d/ --socket /run/thd.socket --user nobody --deviceglob /dev/input/event*
             ├─systemd-logind.service
             │ └─353 /lib/systemd/systemd-logind
  # enabled  ├─otbr-agent.service
             │ └─501 /usr/sbin/otbr-agent -I wpan0
             ├─cron.service
             │ └─350 /usr/sbin/cron -f
             ├─systemd-udevd.service
             │ └─154 /lib/systemd/systemd-udevd
             ├─rsyslog.service
             │ └─345 /usr/sbin/rsyslogd -n
             ├─bluetooth.service
             │ └─445 /usr/lib/bluetooth/bluetoothd
             ├─systemd-journald.service
             │ └─136 /lib/systemd/systemd-journald
             └─dhcpcd.service
               ├─409 wpa_supplicant -B -c/etc/wpa_supplicant/wpa_supplicant.conf -iwlan0
               └─466 /sbin/dhcpcd -q -w
```

If those services are running, but the RPi is in a **degraded** state, some
other service has failed to start. Check to see which:

```
$ sudo systemctl --failed
```

## Step 6: Verify RCP

Verify that the RCP is in the correct state:

```
$ sudo ot-ctl state
```

`ot-ctl` is a command line utility provided with OTBR. It is used to
communicate with the Thread PAN interface (default is `wpan0`) that `otbr-agent`
is bound to in the RCP design.

If the RCP is successfully running and the node is not a member of a Thread
network, the output should be similar to the below:

```
disabled
```

If the output is `OpenThread daemon is not running`, troubleshoot with the following:

1.  Verify the Border Router has sufficient power (use the proper external AC
    adapter).
1.  Disconnect and reconnect the RCP board to the Border Router platform.
1.  Verify that the RCP serial device is present. For example, if the device
    should be attached to `/dev/ttyUSB0`:

    ```
    $ ls /dev/ttyUSB*
    /dev/ttyUSB0
    ```

1.  Reset the RCP with `sudo ot-ctl reset`.

Check the RCP status again with `sudo ot-ctl state`.

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
