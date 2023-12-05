# Setting Up Service Registration Protocol (SRP) Server-Client Connectivity With OT CLI

OpenThread offers SRP server and client commands for use with a Thread network
to perform such functions as registering and leasing services, and managing
server and client hosts.

This guide provides some basic tasks that use some of the more common `srp server`
and `srp client` commands.

## SRP server commands

For a list of `srp server` commands, type `help`:

> srp server help
addrmode
auto
disable
domain
enable
help
host
lease
seqnum
service
state
Done

## SRP Client Commands

For a list of `srp client` commands, type `help`:

> srp client help
autostart
callback
help
host
keyleaseinterval
leaseinterval
service
start
state
stop
ttl
Done

## CLI Command Reference

For descriptions and syntax of all commands, refer to the CLI Command Reference.
The SRP server commands begin alphabetically with
[srp server (enable,disable)](https://openthread.io/reference/cli/commands#srp_server_enabledisable).
The SRP client commands begin alphabetically with
[srp client autostart (get)](https://openthread.io/reference/cli/commands#srp_client_autostart_get).

## Thread Border Router codelab reference

The [OpenThread Border Router codelab](https://openthread.io/codelabs/openthread-border-router#1)
includes information about how to set up the SRP client end device.

## Example

INTRO TEXT

### Starting the SRP Server

1. Start the SRP server node:
  
   ```
   > ./output/simulation/bin/ot-cli-ftd 1
   ```
1. Set up a Thread network and start the SRP server:

   ```
   > dataset init new
   Done
   > dataset
   Active Timestamp: 1
   Channel: 22
   Channel Mask: 0x07fff800
   Ext PAN ID: 8d6ed7a05a28fb3b
   Mesh Local Prefix: fded:5114:8263:1fe1::/64
   Network Key: 7fcbae4153cc2955c28440c15d4d4219
   Network Name: OpenThread-f7af
   PAN ID: 0xf7af
   PSKc: b658e40f174e3a11be149b302ef07a0f
   Security Policy: 672, onrc
   Done
   > dataset commit active
   Done
   > ifconfig up
   Done
   > thread start
   Done
   > state
   leader
   Done
   > ipaddr
   fded:5114:8263:1fe1:0:ff:fe00:fc00
   fded:5114:8263:1fe1:0:ff:fe00:c000
   fded:5114:8263:1fe1:68bc:ec03:c1ad:9325
   fe80:0:0:0:a8cd:6e23:df3d:4193
   Done
   > srp server enable
   Done
   ```

### Starting the SRP client

1. Start the SRP Client node:

   ```
   > ./output/simulation/bin/ot-cli-ftd 2
   ```

1. Join the Thread network, and register a `_ipps._tcp` service:
   
   ```
   > dataset networkkey 7fcbae4153cc2955c28440c15d4d4219
   Done
   > dataset commit active
   Done
   > ifconfig up
   Done
   > thread start
   Done
   > state
   child
   Done
   > ipaddr
   fded:5114:8263:1fe1:0:ff:fe00:c001
   fded:5114:8263:1fe1:44f9:cc06:4a2d:534
   fe80:0:0:0:38dd:fdf7:5fd:24e
   Done
   > srp client host name my-host
   Done
   > srp client host address fded:5114:8263:1fe1:44f9:cc06:4a2d:534
   Done
   > srp client service add my-service _ipps._tcp 12345
   Done
   > srp client autostart enable
   Done
   ```
Use the `srp client autostart enable` command to enable auto-start mode on the client.
The client monitors network data to discover available SRP servers within
the Thread network, then the client automatically starts itself. Alternatively,
start the client manually by issuing the `srp client start` command, and be sure
to include the SRP address and port in the command.

The SRP Server listening UDP port, which is `c002(49154)` in the example below,
is included in the server data that is returned by running the `netdata show` command.

```
> netdata show
Prefixes:
Routes:
Services:
44970 5d c002 s 8400
Done
srp client start fded:5114:8263:1fe1:68bc:ec03:c1ad:9325 49154
Done
```

### Verify the service status

1. Check if the host and service has been successfully registered on the client node:

   ```
   > srp client host
   name:"my-host", state:Registered, addrs:[fded:5114:8263:1fe1:44f9:cc06:4a2d:534]
   Done
   > srp client service
   instance:"my-service", name:"_ipps._tcp", state:Registered, port:12345, priority:0, weight:0
   Done
   ```
1. Make sure it shows `state:Registered` for both host and service commands
   ```
1. Check the host and service on the server node:
   ```
   > srp server host
   my-host.default.service.arpa.
   deleted: false 
   addresses: [fded:5114:8263:1fe1:44f9:cc06:4a2d:534]
   Done
   > srp server service
   my-service._ipps._tcp.default.service.arpa.
   deleted: false
   port: 12345
   priority: 0
   weight: 0
   ttl: 7200
   lease: 7200
   key-lease: 1209600
   TXT: []
   host: my-host.default.service.arpa.
   addresses: [fded:5114:8263:1fe1:44f9:cc06:4a2d:534]
   Done
   ```
   Make sure the output shows `deleted: false` for both the `srp server host`
   and `srp service service` commands.


### 


### `XXXX` command

Use the `tcp init` command to initialize the TCP module to begin TCP communication.
The TCP module can then perform many functions, such as listening for incoming
connections using the example TCP listener provided by the `tcp` CLI.
To deinitialize the example TCP listener and the example TCP endpoint,
issue the `tcp deinit` command.

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
