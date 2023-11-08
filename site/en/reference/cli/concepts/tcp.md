# Test TCP Functionality With OT CLI

Thread networks offer several `cli tcp` commands for testing peer-to-peer communication
using TCP endpoints. The `cli tcp` provides one sample TCP endpoint and one sample
TCP listener with which all `tcp`commands interact.

Command overviews and the example that follows provide information on initializing
the example TCP endpoint and TCP listener, how to connect to a peer TCP endpoint,
and how to send a message to the peer.

## TCP commands

For a list of `tcp` commands, type `help`:

> tcp help
abort
benchmark
bind
connect
deinit
init
listen
send
sendend
stoplistening
Done

For descriptions and syntax of all commands, refer to the CLI Command Reference.
The TCP commands begin alphabetically with
[tcp abort](https://openthread.io/reference/cli/commands#tcp_abort).

### `init` command

Use the `tcp init` command to initialize the TCP module to begin TCP communication.
The TCP module can then perform many functions, such as listening for incoming
connections using the example TCP listener provided by the `tcp` CLI.
If you want to deinitialize the example TCP listener and the example TCP endpoint,
you can issue the `tcp deinit` command.

### `bind` command

After you initialize the example TCP endpoint, you can run a `tcp bind`
command to assign an IPv6 address and a port to the TCP endpoint. This binds
the endpoint for communication. Assigning the IPv6 address and port is also
referred to as "naming the endpoint."

### `listen` command

After you initialize the TCP module, you can run a `tcp listen` command to
use the example TCP listener to listen for incoming connections on the
TCP endpoint specified with its IPv6 address and port number.

If you want the example listener to stop listening for incoming TCP connections,
you can issue the `tcp stoplistening` command. 

### `connect` command

A `tcp connect` command can be used to connect the example TCP endpoint to a
peer TCP endpoint address. You can then issue a `tcp send` command to send a
message to the peer.

### `send` command

A `tcp send` command sends a message using the example TCP endpoint to the
destination endpoint that was speciifed in a preceding `tcp connect` command.

### `benchmark` commands

Once a TCP connections is established between two nodes, you can use the
`benchmark` commands to send large amounts of data between the nodes to test
network bandwidth and performance. The number of transmitted bytes in milliseconds
as well as the TCP Goodput will be provided in the `benchmark` results.

### `abort` command

To immediately and unceremoniously end a TCP connection, run the `tcp abort`
command on either node to transition the TCP endpoint to a closed state.

### `sendend` command

When one node is done sending data to the other node, the first node can
issue a `tcp sendend` command to alert the second node to no longer expect
data. The second node can also send a `tcp sendend` to the first node.
Once each node receives a `TCP: Disconnected` message, the TCP connection
between the two nodes is torn down. It is recommended but not required to
issue this command when data transfer is complete.

## Send a message between two nodes

1. On node 1, initialize the TCP CLI module, then listen for incoming connections
   using the example TCP listener.

   ```
   > tcp init
   > tcp listen :: 30000
   ```

   The use of `::` denotes that the `listen` should use the unspecified IPv6 address,
   thereby having the TCP/IPv6 stack assign the IPv6 address. The port is 30000.  

1. On Node 2, initialize the TCP CLI module, connect to node 1, and then send a
   simple message. 

   ```
   > tcp init
   > tcp connect fe80:0:0:0:a8df:580a:860:ffa4 30000
   > tcp send hello
   ```
### Verification

Based on the example steps shown above, the following output would be expected:

1. After Node 2 runs the `tcp connect` command, Node 2 should receive:
   the message `TCP: Connection established'.
1. Node 1 should then receive the messages (with example IPv6 address and port):
    * `Accepted connection from [fe80:0:0:0:8f3:f602:bf9b:52f2]:49152'
    * `TCP: Connection established'
1. After Node 2 runs the `tcp send` command, Node 1 should receive:
   the message: `TCP: Received 5 bytes: hello' 
    
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
