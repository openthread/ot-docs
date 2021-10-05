# External Thread Commissioning

<figure class="attempt-right">
<a href="../images/thread-commissioning.png"><img src="../images/thread-commissioning.png" srcset="../images/thread-commissioning.png 1x, ../images/thread-commissioning_2x.png 2x" border="0" alt="Thread External Commissioning" /></a>
</figure>

OpenThread Border Router (OTBR) features a Thread Border Agent, which supports
external Thread Commissioning. In external Thread Commissioning, a device
outside of the Thread network (for example, a mobile phone) commissions new
devices onto the network.

The Thread Commissioner serves to authenticate a user (external Commissioner) or
a Thread device onto the Thread network. After authentication, the Commissioner
instructs the Border Router to transfer Thread network credentials, such as the
network key, to the device directly.

This is an example of in-band commissioning, where Thread network credentials
are transferred between devices over the radio.

**Key Point:** During commissioning, the Thread Commissioner never gains
possession of the network key.

This guide details how to commission an OpenThread device onto a network created
and managed by the OTBR Web GUI. 

To learn how to commission without an external Commissioner, see
[Thread Commissioning](/guides/build/commissioning).

## Form the Thread network

### Web GUI

The recommended way to form a Thread network is via the [OTBR Web
GUI](https://openthread.io/guides/border-router/web-gui). When doing so, change
all the default values on the **Form** menu option, except for the On-Mesh
Prefix.

Make note of the **Passphrase** used. This passphrase is the Commissioner
Credential and is used (along with the Extended PAN ID and Network Name) to
generate the Pre-Shared Key for the Commissioner (PSKc). The PSKc is needed to
authenticate the Thread Commissioner (the external device) to the network.

**Note:** The Commissioner Credential is a user-defined string between 6
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
    $ ./pskc J01NME DEAD1111DEAD2222 OpenThreadGuide
    198886f519a8fd7c981fee95d72f4ba7
    ```
    
1.  Set the PSKc:

    ```
    $ sudo ot-ctl dataset pskc 198886f519a8fd7c981fee95d72f4ba7</code>
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
overview of building OpenThread, see the [Building Guide](../build/index.md).

Use the `-DOT_JOINER=ON` build option to enable the Joiner role.

For example, to build the CC2538 example platform for use as a Joiner:

```
$ ./script/build -DOT_JOINER=ON
```

Specific instructions on building supported platforms with GNU Autotools can be
found in each example's
[platform folder](https://github.com/openthread/openthread/tree/master/examples/platforms).

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

* [OT Commissioner CLI](https://openthread.io/guides/commissioner/build) 
* [OT Commissioner Android App](https://github.com/openthread/ot-commissioner/tree/master/android)

OT Commissioner CLI runs on the same same host machine as an OTBR. In the next
guide, learn how to use [OT Commissioner CLI](ot-commissioner-cli.md) to connect
to a border router and commission a new device, or skip to [External
Commissioning for Android](ot-commissioner-andriod.md).

For Thread 1.1 networks, additional options include [Thread Group's Thread 1.1
Commissioning App for Android](#temp).

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
