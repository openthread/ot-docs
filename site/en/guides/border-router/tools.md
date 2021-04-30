# Tools and Scripts

OpenThread Border Router (OTBR) includes a variety of tools and scripts that can
be used for testing purposes.

## PSKc generator

`pskc` generates a Pre-Shared Key for the Commissioner (PSKc). The
PSKc is used to authenticate an external Thread Commissioner to a Thread
network. Build and install OTBR to use this tool.

After building, `pskc` is located at
[`/ot-br-posix/tools`](https://github.com/openthread/ot-br-posix/tree/master/tools/README.md).

### Parameters

The PSKc is generated from the following parameters:

*   Commissioner Credential
*   Thread Network Extended PAN ID
*   Thread Network Name

> Note: The Commissioner Credential is a user-defined string between 6 and 255 characters, UTF-8 encoded.
  
### Usage

Syntax:

```
pskc {<commissioner-credential> <extpanid> <network-name>}
```

Example:

```
$ ./pskc J01NME 1234AAAA1234BBBB MyOTBRNetwork
ee4fb64e9341e13846bbe7e1c52b6785
```

See [External Thread
Commissioning](/guides/border-router/external-commissioning#manual) for how to
use this tool with `ot-ctl`.

## Steering data generator

`steering-data` uses a Bloom filter to generate a hash of the set of Joiners
intended for commissioning. During commissioning, the Joiner only looks for
networks advertising steering data that includes the Joiner itself. Build and
install OTBR to use this tool.

After building, `steering-data` is located at
[`/ot-br-posix/tools`](https://github.com/openthread/ot-br-posix/tree/master/tools/README.md).

### Parameters

The steering data is generated from the following parameters:

*   Joiner ID (EUI-64)
*   Byte length of steering data (optional, default is 16)

### Usage

Syntax:

```
steering-data {[length] <joiner-id>}
```

Example:

```
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

## OTBR commissioner

Use `otbr-commissioner` to commission a Thread device from the command line.
This tool is used in MeshCop (Mesh Commissioning Protocol) tests during
continuous integration. Build and install OTBR to use this tool.

After building, `otbr-commissioner` is located at
[`/src/commissioner`](https://github.com/openthread/ot-br-posix/tree/master/src/commissioner).

### Parameters

To successfully commissioner a Thread device with `otbr-commissioner`, we
suggest using the following parameters at a minimum:

<table class="details responsive">
  <thead>
    <th colspan="2">Parameters</th>
  </thead>
        <tbody>
          <tr>
            <td>Network details</td>
            <td>
              <table class="function param responsive">
                <tbody>
                  <tr>
                    <td>
                      <code>--network-name</code>
                    </td>
                    <td>
                      <div>Thread network name.</div>
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <code>--network-password</code>
                    </td>
                    <td>
                      <div>Commissioner credential.</div>
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <code>--xpanid</code>
                    </td>
                    <td>
                      <div>Thread extended PAN ID.</div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </td>
          </tr>
          <tr>
            <td>Joiner details</td>
            <td>
              <table class="function param responsive">
                <tbody>
                  <tr>
                    <td>
                      <code>--joiner-eui64</code>
                    </td>
                    <td>
                      <div>Factory-assigned IEEE EUI-64 of the joiner device.</div>
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <code>--joiner-pskd</code>
                    </td>
                    <td>
                      <div>Joiner credential.</div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </td>
          </tr>
          <tr>
            <td>Agent details</td>
            <td>
              <table class="function param responsive">
                <tbody>
                  <tr>
                    <td>
                      <code>--agent-host</code>
                    </td>
                    <td>
                      <div>Agent IP address from mDNS broadcasts.</div>
                    </td>
                  </tr>
                  <tr>
                    <td>
                      <code>--agent-port</code>
                    </td>
                    <td>
                      <div>Agent port from mDNS broadcasts.</div>
                    </td>
                  </tr>
                </tbody>
              </table>
            </td>
          </tr>
        </tbody>
</table>

Use `--help` for a full list of parameters.

### Usage
```
$ ./otbr-commissioner --network-name MyOTBRNetwork --network-password J01NME \
      --xpanid 1234AAAA1234BBBB --joiner-eui64 0000b57fffe15d68 \
      --joiner-pskd J01NU5 --agent-host 192.168.1.2 --agent-port 49191
```

For a shell script example, see the
[`meshcop` test script](https://github.com/openthread/ot-br-posix/tree/master/tests/scripts/meshcop).

## standalone_ipv6 script

Use the `standalone_ipv6` script to test IPv6 functionality if your test or
development environment does not have a full IPv6 infrastructure available (for
example, if your network is not connected to an upstream IPv6 provider).

This script installs extra features on the platform running OTBR to enable the
device to serve IPv6 addresses.

This script is located at [`/ot-br-posix/script/standalone_ipv6`](https://github.com/openthread/ot-br-posix/tree/master/script/standalone_ipv6).

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
