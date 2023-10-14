# Send UDP Messages with OT CLI

Thread networks offer several `udp` commands for establishing peer-to-peer communication
using UDP sockets. Each peer needs its own socket. Two simple examples are shown below
to demonstrate different methods establishing communication over UDP.

## UDP Commands

For a list of `udp` commands, type `help`:

> udp help
bind
close
connect
linksecurity
open
send
Done

### `open` commmand

You use the `open` command to open a socket. You can then bind the socket to
a specific IP address and port if you wish. However, binding a socket is not
required to use if for communication. If you do not wish to bind a socket, the
socket will choose its own local IP address and portchoose its own local
IP address and port.

### `bind` command

After you `open` a socket, you can run a `udp bind` command to assign an IPv6 address
and a port to the open socket. This binds the socket for communication. Assigning the
IPv6 address and port is referred to as naming the socket.

### `connect' command

A `udp connect` command can be used to establish UDP communication with a peer node
by specifying the IP address and UDP port number of the peer. This command is typically
followed by a `udp send` command.

### `send' command

A `udp send` commands sends a message to the socket whose IP address and UDP port can
specified with the command variables. If the IP address and port are not specified in the
`udp send` command, the message gets sent to the socket that was specified in the `udp connect` command.

## Form a Network With Two Devices

1. On Node 1, open a UDP socket.

    ```
    > udp open
    Done
    ```

1. On Node 1, bind the socket.
   
   ...
   > udp bind :: 1234
   Done
   ...

The use of `::` denotes that the `bind` should use the unspecified IPv6 address,
therby having the UDP/IPv6 stack assign the binding IPv6 address. For complete
options with `udp bind`, refer to [`udp bind`](https://openthread.io/reference/cli/commands#bbr_state#udp_bind).

1.  On Node 2, open a UDP socket.

    ```
    > udp open
    Done
    ```

1. On Node 2, send a simple message to Node 1. 

   ...
   > udp send fdde:ad00:beef:0:bb1:ebd6:ad10:f33 1234 hello
   Done
   ...

This command assumes that Node 2 has already discovered the address of Node 1.
Additionally, in this example, the administrator of Node 2 has chosen to not
bind the socket. This is because the Node 2 administrator wants to send
a message to Node 1 without caring which of its IP addresses and ports are used
as the Node 2 source. The socket chooses an IP address and port randomly in this scenario.

For complete options with `udp send`, refer to
[`udp send`](https://openthread.io/reference/cli/commands#bbr_state#udp_send)

1. On Node 1, a display such as the following should indicate that the message from Node 2
has been received.

   ...
   5 bytes from fdde:ad00:beef:0:dac3:6792:e2e:90d8 49153 hello
   ...

## Incorporating Use of `Connect` in Forming Network With Two Devices

This example is similar to the previous one, but demonstrates some of the flexibilty
you have in using UDP sockets.

1. On Node 1, open a UDP socket.

    ```
    > udp open
    Done
    ```

1. On Node 1, bind the socket.

   ...
   > udp bind :: 1234
   Done
   ...

1.  On Node 2, open a UDP socket.

    ```
    > udp open
    Done
    ```

1.  On Node 2, use the `udp connect` command to open communication to Node 1.

    ...
    > udp connect fdde:ad00:beef:0:bb1:ebd6:ad10:f33 1234
    Done
    ....

For complete options with `udp connect`, refer to
[`udp send`](https://openthread.io/reference/cli/commands#bbr_state#udp_connect)

1. On Node 2, use the `udp send` command to send a message to Node 1, but do not
   specify `ip` and `port` in the `udp send` command syntax.

    ....
    >udp send fdde:ad00:beef:0:bb1:ebd6:ad10:f33 1234 hello
    Done
    ....

    By not specifying `ip` and `port`, the `udp send` command uses the `ip` and `port`
    that were specified in the `udp connect` command.

Copyright (c) 2023, The OpenThread Authors.
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
