#  OT Commissioner CLI

{{intro paragraph}}

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

## Build and install OT Commissioner

Build and install OT Commissioner. See [OT Commissioner Build and
Configuration](https://openthread.io/guides/commissioner/build) for instructions.

## Connect to the Border Router

**Note:** OT Commissioner must be running on the same host machine as OTBR.

1.  Open the Non-CCM configuration file located at
    `/usr/local/etc/commissioner/non-ccm-config.json` and change the `PSKc` to
    `198886f519a8fd7c981fee95d72f4ba7`:
    
    `"PSKc" : "198886f519a8fd7c981fee95d72f4ba7"`   
    
1.  Start the OT Commissioner CLI with the Non-CCM configuration:

    ```
    $ commissioner-cli /usr/local/etc/commissioner/non-ccm-config.json
    >
    ```
    
1.  Connect to OTBR:

    ```
    > start :: 49191
    [done]
    >
    ```
    
1.  Verify that the Commissioner is active:

    ```
    >active
    true
    [done]
    >
    ```

## Commission the Joiner

Once connected to the Border Router, OT Commissioner can commission the Joiner
device.

1.  In OT Commissioner, enable Thread MeshCoP joiner for all Joiners with a
    password of `J01NU5`:
    
    ```
    > joiner enableall meshcop J01NU5
    [done]
    >
    ```
    
1.  On the Joiner device, start the Joiner role with the password configured in
    OT Commissioner:
    
    ```
    > ifconfig up
    Done
    > joiner start J01NU5
    Done
    ```

1.  Wait a minute for the DTLS handshake to complete between the Commissioner
    and Joiner:
    
    ```
    >
    Join success!
    ```

## Join the network

On the Joiner device, start the Thread protocol to automatically join the
network.

```
> thread start
Done
```

Check the state after a few moments to confirm. It may initially start as a
child, but within two minutes it should upgrade to a router.

```
> state
router
Done
```

Also check the device's IPv6 addresses. It should have a Global address using
the On-Mesh Prefix specified during formation of the Thread network through the
OTBR Web GUI.

```
> ipaddr
fdde:ad11:11de:0:0:ff:fe00:9400
fd11:22:0:0:3a15:3211:2723:dbe1 #Global address with on-mesh prefix
fe80:0:0:0:6006:41ca:c822:c337
fdde:ad11:11de:0:ed8c:1681:24c4:3562
```

## Ping the external internet

Test the connectivity between the Joiner device in the Thread network and the
external internet by pinging a public IPv4 address.

For example, the Well-Known NAT64 prefix of `64:ff9b::/96` and an IPv4 address
of `8.8.8.8` combine to form an IPv6 address of `64:ff9b::808:808`.

Add an external route for the NAT64 Prefix:

```
$ sudo ot-ctl route add 64:ff9b::/96 s med</code>
Done
$ sudo ot-ctl netdata register
Done
```

Ping the synthesized IPv6 address `64:ff9b::808:808` from the OpenThread CLI on the Joiner device:

```
> ping 64:ff9b::808:808
16 bytes from 64:ff9b:0:0:0:0:808:808: icmp_seq=3 hlim=45 time=72ms
```

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
