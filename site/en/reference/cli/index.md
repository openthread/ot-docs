# OpenThread CLI Overview

The OpenThread CLI exposes configuration and management APIs from a
command-line interface. Use OT CLI to create an OpenThread development
environment, or use it as a tool with additional application code. For example,
the OpenThread
[test scripts](https://github.com/openthread/openthread/tree/main/tests) use
the CLI to execute test cases.

## Get started

Complete our Simulation Codelab with Docker and review the CLI Command Reference.

<a class="button button-primary" style="width:285px"
   href="https://openthread.io/codelabs/openthread-simulation">Go to the Simulation Codelab</a>

<a class="button button-primary" style="width:285px"
   href="https://openthread.io/reference/cli/commands">Go to the CLI Command Reference</a>

## Use OT CLI

You can use OT CLI with OpenThread Border Router (OTBR) and Thread devices. CLI
commands will vary depending on your device type and build flags.

### OTBR

To use OT CLI with [OTBR](../../guides/border_router), enter the following
prefix before each command:

```
$ sudo ot-ctl
```

### Thread devices

To use CLI Commands on a Thread device, refer to the platform documentation,
codelab, or guide. For many examples, you can begin typing commands without a
prefix:

```
> state
router
Done
```

Here are a few resources to help you get started:

*   Review OpenThread [Platforms](https://openthread.io/platforms/)
*   [Nordic Hardware Codelab](https://openthread.io/codelabs/openthread-hardware/)
*   [Silicon Labs Hardware Codelab](https://openthread.io/codelabs/silabs-openthread-hardware/)
*   [Platform Examples on GitHub](https://github.com/openthread/openthread/tree/main/examples/platforms)

## Special characters

The whitespace character (`' '`) is used to delimit the command name and the
different arguments, together with the tab (`'\t'`) and new line characters
(`'\r'`, `'\n'`).

Some arguments might include spaces, for example a Thread network name. To
send arguments that include spaces, use the backslash character (`'\'`) to
escape separators or the backslash itself:

```
> networkname Test\ Network
Done
> networkname
Test Network
Done
> 
```

## Argument mappings

OT CLI uses predefined arguments that correspond to API configuration values. These
mappings can be passed with CLI commands, and might also return to the CLI
console for various Network Data commands, for example
[netdata show](https://openthread.io/reference/cli/concepts/netdata.md).

### otBorderRouterConfig

Some commands, for example `prefix add`, require
[otBorderRouterConfig](https://openthread.io/reference/struct/ot-border-router-config)
values. To set `otBorderRouterConfig` members from the command line, OT CLI
parses a mapped letter argument for each member. For example, the argument
combination `paros` sets the
[mPreferred](https://openthread.io/reference/struct/ot-border-router-config#mconfigure),
[mSlaac](https://openthread.io/reference/struct/ot-border-router-config#mSlaac),
[mDefaultRoute](https://openthread.io/reference/struct/ot-border-router-config#mDefaultRoute),
[mOnMesh](https://openthread.io/reference/struct/ot-border-router-config#mOnMesh),
and [mStable](https://openthread.io/reference/struct/ot-border-router-config#mStable)
members, consecutively.

#### Syntax

In the following example, `prefix` is required, and
[otBorderRouterConfig](https://openthread.io/reference/struct/ot-border-router-config)
arguments are optional, mapped as `p`, `a`, `d`, `c`, `r`, `o`, `s`, `n`, and
`D`:

<pre>prefix add <var>prefix</var> [<var>padcrosnD</var>]</pre>

#### Usage

To use argument mappings, do not enter spaces between the letters:

```
prefix add 2001:dead:beef:cafe::/64 paros
```

### otRoutePreference

To set the [otRoutePreference](https://openthread.io/reference/group/api-thread-general#otroutepreference),
use `high`, `med`, or `low` in OT CLI commands.

#### Syntax

<pre>prefix add <var>prefix</var> [<var>padcrosnD</var>] [<var>high</var>|<var>med</var>|<var>low</var>]</pre>

#### Usage

Here's an example of using mapped `otBorderRouterConfig` and `otRoutePreference`
parameters:

```
> prefix add 2001:dead:beef:cafe::/64 paros med
Done
```

### otExternalRouteConfig

For [otExternalRouteConfig](https://openthread.io/reference/struct/ot-external-route-config)
values, `s` maps to `mStable` and `n` maps to `mNat64`.

#### Syntax

<pre>publish route <var>prefix</var> [<var>sn</var>]</pre>

#### Usage

```
> route add 2001:dead:beef:cafe::/64 s
Done
```

## Return values

Most commands return the requested value, followed by `Done`:

```
> br onlinkprefix
fd41:2650:a6f5:0::/64
Done
```

Other commands that include Network Data might return argument mappings
for prefix, route, and service records. For more information, refer to
[Display and Manage Network Data with OT CLI](https://openthread.io/reference/cli/concepts/netdata.md).

## License

Copyright (c) 2021-2022, The OpenThread Authors.
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
