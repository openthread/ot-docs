# Test UDP Functionality With OT CLI

OpenThread offers UDP commands for use with a Thread network to test peer-to-peer
communication between UDP sockets. The `cli udp` provides one sample socket with
which all `udp` commands interact.

The examples that follow demonstrate how you can open and bind sockets, how to
connect the socket, and how to send messages using UDP sockets.

## UDP commands

For a list of `udp` commands, type `help`:

> udp help
bind
close
connect
linksecurity
open
send
Done

### `open` command

Use the `udp open` command to open the socket to begin UDP communication.
You then have the option to bind the socket to a specific IP address and port.

### `bind` command

After you `open` the socket, you can run a `udp bind` command to assign an IPv6 address
and a port to the open socket. This binds the socket for communication. Assigning the
IPv6 address and port is also referred to as naming the socket. If you do not directly
`bind` the socket, connecting the socket (`udp connect`) or using it in
a `udp send` command binds the socket to an ephemeral port.

### `connect` command

A `udp connect` command can be used to connect the example socket to a peer socket address.
You can then issue a `udp send` command to send a message to the peer. If the socket
is not already bound, issuing the `udp connect` command also binds the socket. 

### `send` command

A `udp send` command sends a message using the example socket to a destination
whose IP address and UDP port can be specified with the command variables.
If the IP address and port are not specified in the
`udp send` command, the message gets sent using the example socket
to the destination that was specified in the `udp connect` command.
Issuing the `udp send` command binds the socket to an ephemeral port
if the socket has not already been bound.

### `close` command

It is recommended that you use the `udp close` command to close the socket when
the socket is no longer needed.

### `linksecurity` command

The `udp linksecurity` command can be used to enable or disable data-link layer security for messages. 

## Send a message between two nodes

1. On Node 1, open the UDP socket.

   ```
   > udp open
   Done
   ```

1. On Node 1, bind the socket.
   
   ```
   > udp bind :: 1234
   Done
   ```

   The use of `::` denotes that the `bind` should use the unspecified IPv6 address,
   thereby having the UDP/IPv6 stack assign the binding IPv6 address. For complete
   options with `udp bind`, such as binding to a network interface, 
   refer to [udp bind](https://openthread.io/reference/cli/commands#udp_bind).

1  On Node 2, open the UDP socket.

   ```
   > udp open
   Done
   ```

1. On Node 2, send a simple message to Node 1. 

   ```
   > udp send fdde:ad00:beef:0:bb1:ebd6:ad10:f33 1234 hello
   Done
   ```

   This command assumes that Node 2 has already discovered the address of Node 1.
   Additionally, in this example, the administrator of Node 2 has chosen to not
   bind the socket. This is because the Node 2 administrator wants to send
   a message to Node 1 without caring which of its IP addresses and ports are used
   as the Node 2 source. The socket chooses an IP address and port randomly in this scenario.

   For complete options with `udp send`, refer to
   [udp send](https://openthread.io/reference/cli/commands#udp_send).

1. Node 1 confirms receipt of the message from Node 2:

   ```
   > 5 bytes from fdde:ad00:beef:0:dac3:6792:e2e:90d8 49153 hello
   ```

## Connect the socket to the peer socket address, then send a message between two nodes

This example is similar to the previous one, but demonstrates some of the flexibility
you have in using UDP sockets. With this method, you first connect the socket to the
peer socket address, then you do not need to specify the peer IP address and port
each time you do a `udp send`.

1. On Node 1, open the UDP socket.

   ```
   > udp open
   Done
   ```

1. On Node 1, bind the socket.

   ```
   > udp bind :: 1234
   Done
   ```

1. On Node 2, open the UDP socket.

   ```
   > udp open
   Done
   ```

1. On Node 2, use the `udp connect` command to open communication to Node 1.

   ```
   > udp connect fdde:ad00:beef:0:bb1:ebd6:ad10:f33 1234
   Done
   ```

   For complete options with `udp connect`, refer to
   [udp connect](https://openthread.io/reference/cli/commands#udp_connect)

1. On Node 2, use the `udp send` command to send a message to Node 1, but do not
   specify `ip` and `port` in the `udp send` command syntax.

   ```
   > udp send hello
   Done
   ```

   By not specifying `ip` and `port`, the `udp send` command uses the `ip` and `port`
   that were specified in the `udp connect` command. 

## License

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
