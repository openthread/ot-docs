# Tools and Scripts

OpenThread Border Router (OTBR) includes a variety of tools and scripts that can
be used for testing purposes.

## PSKc generator

`pskc`, available in [ot-br-posix/tools](https://github.com/openthread/ot-br-posix/tree/main/tools),
generates a Pre-Shared Key for the Commissioner (PSKc). The
PSKc is used to authenticate an external Thread Commissioner to a Thread
network. Build and install OTBR to use this tool.

After building, `pskc` is located at
`ot-br-posix/build/otbr/tools`.

### Parameters

The PSKc is generated from the following parameters:

*   Commissioner Credential
*   Thread Network Extended PAN ID
*   Thread Network Name

> Note: The Commissioner Credential is a user-defined string between 6 and 255
characters, UTF-8 encoded.

### Usage

Syntax:

```
pskc {commissioner-credential} {extpanid} {network-name}
```

Example:

```
$ cd ~/ot-br-posix/build/otbr/tools
$ ./pskc J01NME 1234AAAA1234BBBB MyOTBRNetwork
ee4fb64e9341e13846bbe7e1c52b6785
```

To use this tool with `ot-ctl`, refer to [External
Commissioning](external-commissioning/index.md).

## Steering data generator

`steering-data`, available in [ot-br-posix/tools](https://github.com/openthread/ot-br-posix/tree/main/tools),
uses a Bloom filter to generate a hash of the set of Joiners
intended for commissioning. During commissioning, the Joiner only looks for
networks advertising steering data that includes the Joiner itself. Build and
install OTBR to use this tool.

After building, `steering-data` is located at
`ot-br-posix/build/otbr/tools`.

### Parameters

The steering data is generated from the following parameters:

*   Byte length of steering data (optional, default is 16)
*   Joiner ID (EUI-64)

### Usage

Syntax:

```
steering-data [{length}] {joiner-id}
```

Example:

```
$ cd ~/ot-br-posix/build/otbr/tools
$ ./steering-data 0000b57fffe15d68
00000000000000000020000000000100
```

Use multiple Joiner IDs to include them all in the steering data:

```
$ ./steering-data 0000b57fffe15d68 0000c57fffe15d68
00000000000080000020000000000500
```

Use the `length` parameter to change the byte length of the resulting steering
data:

```
$ ./steering-data 8 0000b57fffe15d68
0020000000000100
```

## OTBR Commissioner

By default, the Commissioner role is enabled on OTBR, similar to enabling the
Commissioner role on a device with the `-DOT_COMMISSIONER=ON` flag. On the
platform running OTBR, use `ot-ctl commissioner` to commission Joiners
from the command line.

### Parameters

Type `help` for a list of commands.

```
> sudo ot-ctl commissioner help
```

> Note: The set of Commissioner CLI commands varies based on the features
enabled in a particular build.

### Usage

Syntax:

```
sudo ot-ctl commissioner {parameters}
```

Example:

```
> sudo ot-ctl commissioner start
Done
> sudo ot-ctl commissioner joiner add 2f57d222545271f1 J01NME
Done
```

## MeshCop Script

OTBR provides a MeshCoP (Mesh Commissioning Protocol) test script that
uses [OT Commissioner](../commissioner/index.md) to test [External Commissioning](external-commissioning/index.md).
For usage information, refer to the [`meshcop` test script](https://github.com/openthread/ot-br-posix/tree/main/tests/scripts/meshcop)
on GitHub.

## standalone_ipv6 script

Use the `standalone_ipv6` script to test IPv6 functionality if your test or
development environment does not have a full IPv6 infrastructure available (for
example, if your network is not connected to an upstream IPv6 provider).

This script installs extra features on the platform running OTBR to enable the
device to serve IPv6 addresses.

This script is located at [`/ot-br-posix/script/standalone_ipv6`](https://github.com/openthread/ot-br-posix/tree/main/script/standalone_ipv6).

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