# Test Connectivity

Once you have started OTBR Docker, form a Thread network and test its
connectivity to the internet.

## Step 1: Form the Thread Network

Start an `ot-ctl` session.

```
$ docker exec -it otbr ot-ctl
```

Generate and view new network configuration.

```
> dataset init new
Done
> dataset
Active Timestamp: 1
Channel: 15
Wake-up Channel: 16
Channel Mask: 0x07fff800
Ext PAN ID: 39758ec8144b07fb
Mesh Local Prefix: fdf1:f1ad:d079:7dc0::/64
Network Key: f366cec7a446bab978d90d27abe38f23
Network Name: OpenThread-5938
PAN ID: 0x5938
PSKc: 3ca67c969efb0d0c74a4d8ee923b576c
Security Policy: 672 onrc 0
Done
```

Commit new dataset to the Active Operational Dataset in non-volatile storage.

```
> dataset commit active
Done
```

Enable Thread interface.

```
> ifconfig up
Done
> thread start
Done
```

Exit the `ot-ctl` session:

```
> exit
```

Use `ifconfig` to view the new Thread network interface:

```
> ifconfig wpan0
wpan0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1280
        inet6 fe80::3c98:89e8:ddec:bda7  prefixlen 64  scopeid 0x20<link>
        inet6 fd4d:b3e5:9738:3193:0:ff:fe00:fc00  prefixlen 64  scopeid 0x0<global>
        inet6 fd4d:b3e5:9738:3193:0:ff:fe00:f800  prefixlen 64  scopeid 0x0<global>
        inet6 fd4d:b3e5:9738:3193:39c4:ee02:ca9e:2b1d  prefixlen 64  scopeid 0x0<global>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 500  (UNSPEC)
        RX packets 16  bytes 1947 (1.9 KiB)
        RX errors 0  dropped 3  overruns 0  frame 0
        TX packets 7  bytes 1152 (1.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

## Step 2: Bring up a second Thread node

With OTBR Docker up and running, add a standalone Thread node to the Thread
network and test that it has connectivity to the internet.

Build and flash a standalone Thread node on the [supported platform](https://openthread.io/platforms)
of your choice. This node does not have to be built with any specific build
switches.

See [Build OpenThread](../../../guides/build.md) for basic building instructions.

See the [Build a Thread network with nRF52840 boards and OpenThread
Codelab](https://openthread.io/codelabs/openthread-hardware) for
detailed instructions on building and flashing the Nordic nRF52840 platform.

1.  After building and flashing, use `screen` in a new terminal window
    to access the CLI. For example, if the device is mounted on port
    `/dev/ttyACM1`:
    ```
    $ screen /dev/ttyACM1 115200
    ```

1.  Press the **Enter** key to bring up the `>` OpenThread CLI prompt.

## Step 3: Join the second node to the Thread network

Using the OpenThread CLI for your second Thread node, join the node to
the Thread network created by OTBR Docker.

1.  Update the Thread network credentials for the node, using the minimum
    required values from OTBR Docker:
    ```
    > dataset networkkey f366cec7a446bab978d90d27abe38f23
    Done
    > dataset commit active
    Done
    ```

1. Bring up the Thread interface and start Thread:
    ```
    > ifconfig up
    Done
    > thread start
    Done
    ```

1.  The node should join the OTBR Thread network automatically. Within two
    minutes its state should be `router`:
    ```
    > state
    router
    ```

## Step 4: Ping a public address

You should be able to a ping a public IPv4 address from the standalone Thread
node at this point. Since Thread only uses IPv6, the public IPv4 address
will be automatically translated to IPv6 by combining with the NAT64 prefix in
the Thread network.

1.  To view the NAT64 prefix in the Thread network:
    ```
    > netdata show
    Prefixes:
    fd3e:d39b:d91:1::/64 paros low 1800
    Routes:
    ::/0 s med 1800
    fd3e:d39b:d91:2:0:0::/96 sn low 1800
    Services:
    Contexts:
    fd3e:d39b:d91:1::/64 1 c
    Commissioning:
    12156 - - -
    ```
    Here `fd3e:d39b:d91:2:0:0::/96` is the NAT64 prefix in the Thread network.

1.  Ping an IPv4 address from the CLI of the standalone Thread node to
    test its internet connectivity: 
    ```
    > ping 8.8.8.8
    Pinging synthesized IPv6 address: fd3e:d39b:d91:2:0:0:808:808
    16 bytes from fd3e:d39b:d91:2:0:0:808:808: icmp_seq=1 hlim=113 time=73ms
    1 packets transmitted, 1 packets received. Packet loss = 0.0%. Round-trip min/avg/max = 73/73.0/73 ms.
    Done
    ```

Success! The second Thread node can now communicate with the internet, through
OTBR Docker.

> Note: This configuration only illustrates public internet connectivity using
IPv4 and NAT64. Direct public connectivity to IPv6 addresses requires
additional configuration of a public IPv6 address or prefix for the OTBR Docker
container, which is out of scope for this guide.

## License

Copyright (c) 2021-2025, The OpenThread Authors.
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
