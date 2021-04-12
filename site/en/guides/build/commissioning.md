# Thread Commissioning

<figure class="attempt-right">
<a href="/guides/images/ot-primer-joiner_2x.png"><img src="/guides/images/ot-primer-joiner.png" srcset="/guides/images/ot-primer-joiner.png 1x, /guides/images/ot-primer-joiner_2x.png 2x" border="0" alt="Commissioner and Joiner" /></a>
</figure>

Commissioning requires one device with the Commissioner role, and one device
with the Joiner role. The Commissioner is either a Thread device in an
existing Thread network, or a device external to the Thread network (such as a
mobile phone) that performs the Commissioner role. The Joiner is the device
wishing to join the Thread network.

A Thread Commissioner is used to authenticate a device onto the network. It does
not transfer or have possession of Thread network credentials such as the master
key.

This guide covers basic, on-mesh commissioning without an external Commissioner
or Border Router. To learn how to use an external Commissioner, see [External
Thread Commissioning](/guides/border-router/external-commissioning).

For an example of commissioning using virtual devices, see the
[OpenThread Simulation Codelab](https://openthread.io/codelabs/openthread-simulation/#3).

## Step 1: Enable roles

To enable the Commissioner and Joiner roles, use the following build switches:

Switch | Description
---- | ----
`COMMISSIONER=1` | Enables the Commissioner role
`JOINER=1` | Enables the Joiner role

For example, to build the CC2538 example platform for use as a Joiner only:

```
$ make -f examples/Makefile-cc2538 JOINER=1
```

Flash each binary to the desired device. One device serves as the Commissioner,
the other as the Joiner.

Specific instructions on building and flashing supported platforms can be found
in each example's [platform folder](https://github.com/openthread/openthread/tree/main/examples/platforms).

## Step 2: Create a network

Create a network on the device acting as the Commissioner:

```
> dataset init new
Done
> dataset
Active Timestamp: 1
Channel: 13
Channel Mask: 07fff800
Ext PAN ID: d63e8e3e495ebbc3
Mesh Local Prefix: fd3d:b50b:f96d:722d/64
Master Key: dfd34f0f05cad978ec4e32b0413038ff
Network Name: OpenThread-8f28
PAN ID: 0x8f28
PSKc: c23a76e98f1a6483639b1ac1271e2e27
Security Policy: 0, onrcb
Done
> dataset commit active
Done
> ifconfig up
Done
> thread start
Done
```

Wait a few seconds and verify that the device has become a Thread Leader:

```
> state
leader
Done
```

## Step 3: Start the Commissioner role

On that same device, start the Commissioner role:

```
> commissioner start
Done
```

Use the * wildcard to allow any Joiner with the specified Joiner Credential to
commission onto the network. The Joiner Credential is used (along with the
Extended PAN ID and Network Name) to generate the Pre-Shared Key for the Device
(PSKd). The PSKd is then used to authenticate a device during Thread
Commissioning. The Joiner Credential should be unique to each device.

```
> commissioner joiner add * J01NME
Done
```

> Note: The Joiner Credential is a device-specific string of all uppercase alphanumeric characters (0-9 and A-Y, excluding I, O, Q and Z for readability), with a length between 6 and 32 characters.

### Restrict to a specific Joiner

To restrict commissioning to a specific Joiner device, use the `eui64`
parameter, which is the device's factory-assigned IEEE EUI-64.

On the device serving as the Joiner, get the EUI-64:

```
> eui64
2f57d222545271f1
Done
```

Use that value instead of the * wildcard in the `commissioner joiner` command on
the Commissioner device:

```
> commissioner joiner add 2f57d222545271f1 J01NME
Done
```

## Step 4: Start the Joiner role

On the device serving as the Joiner, perform a factory reset, then enable the
Joiner role with the same Joiner Credential specified on the Commissioner:

```
> factoryreset
> ifconfig up
Done
> joiner start J01NME
Done
```

Wait a few seconds for confirmation:

```
> Join success!
```

The Joiner device has successfully authenticated itself with the Commissioner
and received Thread Network credentials.

Now start Thread on the Joiner device:

```
> thread start
Done
```

## Step 5: Validate authentication

Check the state on the Joiner device, to validate that it has joined the
network. Within two minutes, the state transitions from child to router:

```
> state
child
Done
...
> state
router
Done
```

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
