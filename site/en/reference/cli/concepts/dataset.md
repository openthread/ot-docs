# Display and Manage Datasets with OT CLI

Thread network configuration parameters are managed using Active and Pending
Operational Dataset objects. OT CLI includes commands to get and set Active
and Pending datasets.

> Key Point: Thread network management can also be done with the [Thread Network SDK for Android](https://developers.home.google.com/thread) and [iOS Thread APIs](https://developer.apple.com/documentation/threadnetwork/).

### WARNING - Restrictions for production use!

The CLI commands to write/change the Active and Pending Operational Datasets may allow setting invalid parameters, or 
invalid combinations of parameters, for testing purposes. These CLI commands can only be used:

- To configure network parameters for the first device in a newly created Thread network.
- For testing (not applicable to production devices).

In production Thread networks, the correct method to write/change Operational Datasets is via a [Commissioner](/src/cli/README_COMMISSIONER.md) 
that performs [commissioning](src/cli/README_COMMISSIONING.md). Production devices that are not an active Commissioner and are part 
of a Thread network MUST NOT modify the Operational Datasets in any way.

## Active Operational Dataset

The Active Operational Dataset includes parameters that are currently in use
across an entire Thread network. The Active Operational Dataset contains:

*   Active Timestamp
*   Channel
*   Channel Mask
*   Extended PAN ID
*   Mesh-Local Prefix
*   Network Name
*   PAN ID
*   PSKc
*   Security Policy

To easily configure a device so that it's a full member of the Thread network, you
can use the [dataset active -x](/reference/cli/commands#dataset_active) command to
get a hex-encoded TLV, and the
[dataset set active](/reference/cli/commands#dataset_set_activepending) command to
set the dataset on a new device.

On an existing device, get the hex-encoded TLV:

```
> dataset active -x
0e080000000000010000000300001035060004001fffe00208e227ac6a7f24052f0708fdb753eb517cb4d3051062b2442a928d9ea3b947a1618fc4085a030f4f70656e5468726561642d393837330102987304105330d857354330133c05e1fd7ae81a910c0402a0f7f8
Done
```

On a new device, set the active dataset:

```
> dataset set active 0e080000000000010000000300001035060004001fffe00208e227ac6a7f24052f0708fdb753eb517cb4d3051062b2442a928d9ea3b947a1618fc4085a030f4f70656e5468726561642d393837330102987304105330d857354330133c05e1fd7ae81a910c0402a0f7f8
Done
```

## Pending Operational Dataset

The Pending Operational Dataset is used to communicate changes to the Active
Operational Dataset before they take effect. The Pending Operational Dataset
contains all the parameters from the Active Operational Dataset, with the
addition of:

- Delay Timer
- Pending Timestamp

## Get started

To manage datasets from the command line, complete our Simulation Codelab with
Docker and review the CLI Command Reference.

<a class="button button-primary" style="width:285px"
   href="/codelabs/openthread-simulation">Go to the Simulation Codelab</a>

<a class="button button-primary" style="width:285px"
   href="/reference/cli/commands">Go to the CLI Command Reference</a>

For a list of `dataset` commands, type `help`:

```
> dataset help
help
active
activetimestamp
channel
channelmask
clear
commit
delay
extpanid
init
meshlocalprefix
mgmtgetcommand
mgmtsetcommand
networkkey
networkname
panid
pending
pendingtimestamp
pskc
securitypolicy
Done
```

## Argument mappings

### Security Policy

Security Policy commands use argument mappings to get and set
[otSecurityPolicy](https://openthread.io/reference/struct/ot-security-policy)
members. For example, `dataset active`:

```
> dataset active
Active Timestamp: 1
Channel: 13
Channel Mask: 0x07fff800
Ext PAN ID: d63e8e3e495ebbc3
Mesh Local Prefix: fd3d:b50b:f96d:722d::/64
Network Key: dfd34f0f05cad978ec4e32b0413038ff
Network Name: OpenThread-8f28
PAN ID: 0x8f28
PSKc: c23a76e98f1a6483639b1ac1271e2e27
Security Policy: 0, onrcb
Done
```

In this example, `Security Policy: 0` indicates [mRotationTime](https://openthread.io/reference/struct/ot-security-policy#mrotationtime).

Here's a list of all of the Security Policy CLI arguments and
the corresponding `otSecurityPolicy` member for each argument:

*   `o`: [mObtainNetworkKeyEnabled](https://openthread.io/reference/struct/ot-security-policy#mobtainnetworkkeyenabled)
*   `n`: [mNativeCommissioningEnabled](https://openthread.io/reference/struct/ot-security-policy#mnativecommissioningenabled)
*   `r`: [mRoutersEnabled](https://openthread.io/reference/struct/ot-security-policy#mroutersenabled)
*   `c`: [mExternalCommissioningEnabled](https://openthread.io/reference/struct/ot-security-policy#mexternalcommissioningenabled)
*   `b`: [mBeaconsEnabled](https://openthread.io/reference/struct/ot-security-policy#mbeaconsenabled)
*   `C`: [mCommercialCommissioningEnabled](https://openthread.io/reference/struct/ot-security-policy#mcommercialcommissioningenabled)
*   `e`: [mAutonomousEnrollmentEnabled](https://openthread.io/reference/struct/ot-security-policy#mautonomousenrollmentenabled)
*   `p`: [mNetworkKeyProvisioningEnabled](https://openthread.io/reference/struct/ot-security-policy#mnetworkkeyprovisioningenabled)
*   `R`: [mNonCcmRoutersEnabled](https://openthread.io/reference/struct/ot-security-policy#mnonccmroutersenabled)

The `dataset securitypolicy` get and set commands also use the same argument
mappings, for example setting the `securitypolicy` and passing `o`, `n`, `r`,
and `c`:

```
> dataset securitypolicy 672 onrc
Done
```

### Dataset components and `mgmt` commands

Along with other parameters, the `mgmtgetcommand` and `mgmtsetcommand`
for Active and Pending Datasets allow you to get and set any combination
of [otOperationalDatasetComponents](https://openthread.io/reference/struct/ot-operational-dataset-components):

*   `activetimestamp`
*   `pendingtimestamp`
*   `networkkey`
*   `networkname`
*   `extpanid`
*   `localprefix`
*   `delaytimer`
*   `panid`
*   `channel`
*   `securitypolicy`

> Note: These commands are primarily used for testing only.

For the `mgmtgetcommand`, you can specify these components in any order to get
the corresponding values. Optionally, you can also pass `-x` to use a hex
string that's treated as a byte sequence representation of TLVs. This can be vendor
specific TLVs that you might want to add in addition to other parameters.

`mgmtgetcommand` also allows you to optionally specify the IPv6 address of
the leader. Otherwise, the leader ALOC is used.

<pre>dataset mgmtgetcommand {active|pending} [address <var>leader-address</var>] [<var>dataset-components</var>] [-x <var>tlv-list</var>]</pre>

For example, to get `activetimestamp` and `securitypolicy`, use the following
arguments:

```
> dataset mgmtgetcommand active address fdde:ad00:beef:0:558:f56b:d688:799 activetimestamp securitypolicy
Done
```

> Note: `mgmtgetcommand` will not output a response to the CLI. Instead, you can
observe the output response from a packet sniffer.

To set components, you can also supply the dataset components in any order,
followed by the component value.

<pre>dataset mgmtgetcommand {active|pending} [<var>dataset-components</var>] [-x <var>tlv-list</var>]</pre>

To set the `activetimestamp` and `securitypolicy`, use the following
arguments:

```
> dataset mgmtsetcommand active activetimestamp 123 securitypolicy 1 onrc
Done
```

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
