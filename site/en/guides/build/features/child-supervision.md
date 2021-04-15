# Child Supervision

To provide an energy-efficient mechanism for a sleepy end device (SED) to
verify that it is still connected to its parent router (listed in the parent
router's child table), enable the Child Supervision feature.

The Thread Specification requires an SED to periodically perform an MLE Child
Update Request and Response exchange with its parent router to verify
connectivity. Child Supervision is an alternate solution for verification of
SED-router connectivity that places the burden of message transmission on the
parent router instead of on the energy-constrained SED.

Note: Most 802.15.4 radios generate an AUTO ACK in response to a received MAC
frame. An SED cannot solely rely on the 802.15.4 ACKs it receives from a parent
router as an indication that it is still connected to that router and listed in
its child table.

## How it works

This feature works in two ways, depending on the node type and which
[parameters](#parameters) are configured:

### On the parent
If a parent router does not transmit to its child SED within the
<a href="#interval">`OPENTHREAD_CONFIG_CHILD_SUPERVISION_INTERVAL`</a>,
the parent router enqueues and sends a Child Supervision message to the child
SED. The Child Supervision message is a MAC frame containing the following
information:

*   The [RLOC16](/guides/thread-primer/ipv6-addressing#how_a_routing_locator_is_generated)
    of the SED as the destination in the MAC header.
*   An empty payload.

By default, a MAC header contains an 802.15.4 ACK request. To disable this
request in the Child Supervision message, set the 
[`OPENTHREAD_CONFIG_CHILD_SUPERVISION_MSG_NO_ACK_REQUEST`](#msg-no-ack-request) parameter to 1.

### On the child

If an SED does not hear from its parent router within the
[`OPENTHREAD_CONFIG_CHILD_SUPERVISION_CHECK_TIMEOUT`](#check-timeout),
it assumes that it has lost its connection to the parent router and initiates
the [MLE
Attach](/guides/thread-primer/network-discovery#join_an_existing_network)
process to reattach to the parent router.

## How to enable

This feature is disabled by default.

Note: This feature is specific to SEDs. It should not be enabled for any other
type of end device.

### By define

To enable Child Supervision, define
`OPENTHREAD_CONFIG_CHILD_SUPERVISION_ENABLE` as `1` in the
[`/src/core/config/child_supervision.h`](https://github.com/openthread/openthread/tree/main/src/core/config/child_supervision.h)
file, prior to [building OpenThread](/guides/build):

```
$ make -f examples/Makefile-<var>&lt;platform&gt; CHILD_SUPERVISION=1
```

## Parameters

Use the following parameters in
[`/src/core/config/child_supervision.h`](https://github.com/openthread/openthread/tree/main/src/core/config/child_supervision.h)
to customize this feature:

<table class="details responsive">
  <thead>
    <th colspan="2">Parameters</th>
  </thead>
  <tbody>
    <tr>
      <td id="interval">OPENTHREAD_CONFIG_CHILD_SUPERVISION_INTERVAL</td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td><b>Default value</b></td>
              <td>
                <div>129 seconds</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the supervision interval in seconds used by parent. Set to 0 to disable the supervision process on the parent.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="check-timeout">OPENTHREAD_CONFIG_SUPERVISION_CHECK_TIMEOUT</td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Default value</b>
              </td>
              <td>
                <div>190 seconds</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the supervision check timeout interval in seconds used by a device in child state. Set to 0 to disable the supervision check process on the child.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="msg-no-ack-request">OPENTHREAD_CONFIG_SUPERVISION_MSG_NO_ACK_REQUEST</td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Default value</b>
              </td>
              <td>
                <div>0 (ACK request enabled)</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Set to 1 to clear/disable the 802.15.4 ACK request in the MAC header of a supervision message.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
  </tbody>
</table>

## API

Use the [Child Supervision API](/reference/group/api-child-supervision) to
manage the supervision and check timeout intervals directly in your OpenThread
application.

## CLI

There are no CLI commands related to this feature.

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
