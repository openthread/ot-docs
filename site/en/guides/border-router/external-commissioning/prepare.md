# Prepare the Thread Network and Joiner Device

In this guide, learn how to use OTBR Web GUI to form the Thread Network.
Then, choose a [supported platform](https://openthread.io/platforms) and
build a Joiner device.

To set up a Border Router, refer to [OpenThread Border Router Build and Configuration](../build.md).

## Form the Thread network

### Web GUI

The recommended way to form a Thread network is via the [OTBR Web
GUI](../web-gui.md). When doing so, change
all the default values on the **Form** menu option, except for the On-Mesh
Prefix.

Make note of the **Passphrase** used. This passphrase is the Commissioner
Credential and is used (along with the Extended PAN ID and Network Name) to
generate the Pre-Shared Key for the Commissioner (PSKc). The PSKc is needed to
authenticate the Thread Commissioner (the external device) to the network.

> Note: The Commissioner Credential is a user-defined string between 6
and 255 characters, UTF-8 encoded.

### Manual

The Thread network can also be formed manually on the command line of
OpenThread POSIX, using `ot-ctl`.

1.  Initialize a new operational dataset:
    ```
    $ sudo ot-ctl dataset init new
    Done
    ```

1.  Set the network credentials:
    ```
    $ sudo ot-ctl dataset panid 0xdead
    Done
    ```

    ```
    $ sudo ot-ctl dataset extpanid dead1111dead2222
    Done
    ```

    ```
    $ sudo ot-ctl dataset networkname OpenThreadGuide
    Done
    ```

    ```
    $ sudo ot-ctl dataset networkkey 11112233445566778899DEAD1111DEAD
    Done
    ```

1.  Generate a hex-encoded PSKc by using a Passphrase (Commissioner Credential),
    the Extended PAN ID, and the Network Name with the PSKc Generator tool on
    the OTBR. Make sure to use the same Extended PAN ID and Network Name that
    was used in the operational dataset:
    ```
    $ cd ~/ot-br-posix/build/otbr/tools
    $ ./pskc j01Nme DEAD1111DEAD2222 OpenThreadGuide
    198886f519a8fd7c981fee95d72f4ba7
    ```

1.  Set the PSKc:
    ```
    $ sudo ot-ctl dataset pskc 198886f519a8fd7c981fee95d72f4ba7
    Done
    ```

1.  Commit the active dataset, set the on-mesh prefix, and form the Thread
    network:
    ```
    $ sudo ot-ctl dataset commit active
    Done
    ```

    ```
    $ sudo ot-ctl prefix add fd11:22::/64 pasor
    Done
    ```

    ```
    $ sudo ot-ctl ifconfig up
    Done
    ```

    ```
    $ sudo ot-ctl thread start
    Done
    ```

    ```
    $ sudo ot-ctl netdata register
    Done
    ```

1.  Confirm the network configuration:
    ```
    $ sudo ot-ctl state
    leader
    Done
    ```

    ```
    $ sudo ot-ctl pskc
    198886f519a8fd7c981fee95d72f4ba7
    Done
    ```

## Prepare the Joiner device

Build and flash a device with OpenThread, to function as the Joiner. For an
overview of building OpenThread, see the [Building Guide](../../build/index.md).

Use the `-DOT_JOINER=ON` build option to enable the Joiner role.

Specific instructions on building supported platforms with GNU Autotools can be
found in each example's
[platform folder](https://github.com/openthread/openthread/tree/master/examples/platforms).

When commissioning a Joiner, it's important to understand the following terms
and concepts:

*   **Joining Device Credential**: You'll need to provide a Passphrase to
    commission a device, for example `J01NU5`. This Passphrase is separate
    from the Commissioner Credential you created when forming your Thread
    network, and has different requirements:

    *   Must be a string of all uppercase alphanumeric characters (0-9 and A-Y,
        excluding I, O, Q, and Z for readability), with a length between 6 and
        32 characters.

    The Joining Device Credential might also be referred to as Join Passphrase,
    Joiner Password, or PSKd. This Passphrase is used to authenticate a device
    during Thread Commissioning. You can also use it with a device's EUI64
    value to generate a unique QR Code.

*   **PSKd**: Pre-Shared Key for the Joiner. The PSKd is the Joining Device
    Credential when it's specifically encoded in binary form.

*   **EUI-64**: 64-bit Extended Unique Identifier, for example
    `0000b57fffe15d68`. This is a Joiner device's factory-assigned IEEE EUI-64,
    used to generate a QR code and uniquely identify a device.

Once the Joiner device is ready, obtain its factory-assigned IEEE EUI-64. Use
the `eui64` command in the OpenThread CLI:

```
> eui64
0000b57fffe15d68
Done
```

## Select Commissioner type

[OpenThread Commissioner](https://openthread.io/guides/commissioner) provides several ways
to externally commission a device:

* [OT Commissioner CLI](cli.md)
* [OT Commissioner Android App](android.md)

The OT Commissioner CLI runs on the same host machine as OTBR. In the next
guide, learn how to use the [OT Commissioner CLI](cli.md) to connect
to a border router and commission a new device, or skip to [External
Commissioning for Android](android.md).

For Thread 1.1 networks, additional options include the [Thread 1.1 Commissioning App
for Android](android.md#thread-group-android-app).

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
