# Set Up Service Registration Protocol (SRP) Server-Client Connectivity With OT CLI

OpenThread offers both SRP server and client functionality, enabling devices
to register DNS-based services using the standard DNS Update sent as unicast
packets. This functionality enables DNS-Based Service Discovery.


This guide provides basic tasks that use some of the more common `srp server`
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

## SRP client commands

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

## Thread Border Router codelab

The [OpenThread Border Router codelab](https://openthread.io/codelabs/openthread-border-router#1)
includes information about how to set up the SRP client end device.

## Overviews of some basic SRP commands

SRP server and client commands can be used in sequence to perform typical SRP tasks:

1. [Start the SRP server](#start-the-srp-server).
   
   `srp server enable` enables the SRP server once you have created the Thread network.

1. [Start the SRP client](#start-the-srp-client).

   * `srp client host name` sets the host name to be used by the client. 

   * `srp client host address (set)` either enables auto host client address mode or
     explicitly sets the list of host client addresses.

   * `srp client service add` adds  a service with a given instance name, service
      name, and port number.

   * `srp client autostart enable` enables auto-start mode. You can also manually
     start the client by running `srp client start`. 

1. [Verify the service status](#verify-the-service-status).

   * `srp client host` and `srp client service` provide status about whether
      the client host and service have been successfully registered on the client node.

   * `srp server host` and `srp server service` provide host and service status
     on the server node.

1. [Remove the service](#remove-the-service).

   `srp client service remove` removes a service but retains the service name.

1. [Remove the host and service names](#remove_the_host_and_service_names).

   `srp client host remove` removes the host and all registered services.

## Examples of SRP server and client command usage

These examples use basic CLI commands to set up a Thread network, start
the SRP server and client, verify server status, and remove a service. Sample data
is used for illustrative purposes.

### Start the SRP server

1. Start the SRP server node:
  
   ```
   $ ./output/simulation/bin/ot-cli-ftd 1
   ```

1. Set up a Thread network, then enable the SRP server by running the `srp server enable` command:

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

### Start the SRP client

1. Start the SRP Client node:

   ```
   $ ./output/simulation/bin/ot-cli-ftd 2
   ```

1. Join the Thread network, set the client host name and address, and
   register a service.
   
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

   As shown above, use the `srp client autostart enable` command to enable
   auto-start mode on the client. The client monitors network data to discover
   available SRP servers within the Thread network, then the client
   automatically starts itself.

1. If manually starting the client, run the following, and include
   the SRP address and port:
  
   ```
   > srp client start fded:5114:8263:1fe1:68bc:ec03:c1ad:9325 49154
   Done
   ```
   The SRP server listening UDP port is `c002(49154)` in the example above.

### Verify the service status

1. Check if the host and service have been successfully registered on the client node:

   ```
   > srp client host
   name:"my-host", state:Registered, addrs:[fded:5114:8263:1fe1:44f9:cc06:4a2d:534]
   Done
   > srp client service
   instance:"my-service", name:"_ipps._tcp", state:Registered, port:12345, priority:0, weight:0
   Done
   ```

   Make sure the output shows `state:Registered` for both host and service commands,
   as in the above example.

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
   and `srp server service` commands, as in the example above. 

### Remove the service

1. Remove the service via the client node:

   ```
   > srp client service remove my-service _ipps._tcp
   Done
   ```

1. Confirm via the server node that the service has been removed:

   ```
   > srp server service
   my-service._ipps._tcp.default.service.arpa.
   deleted: true
   Done
   ```

   The service entry is listed in the output because the name of service is
   not removed.

### Remove the host and service names

1. Remove the host and all its registered services:

   ```
   > srp client host remove 1
   Done
   ```

1. Confirm on the server node that no host or service entries are listed:

   ```
   > srp server host
   Done
   > srp server service
   Done
   >
   ```

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
