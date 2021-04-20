# Periodic Parent Search

To allow end devices (EDs) in a Thread network to switch to a better parent
router than their current one&mdash;while still attached to the
network&mdash;enable the Periodic Parent Search feature.

To determine whether a router is a better parent for the ED, this feature checks
a variety of router attributes, including:

*   RSSI (received signal strength indicator)
*   Link Quality
*   Connectedness of the router to other routers
*   Number of existing children for the router

This ensures that EDs connect to the best possible router rather than remaining
attached to a router with poor link quality or connectedness. This feature is
particularly useful when a new router is added to an existing Thread network.

## How it works

1.  The ED checks the average RSSI value for its current parent router,
    according to the configured check interval
    ([`OPENTHREAD_CONFIG_PARENT_SEARCH_CHECK_INTERVAL`](#check-interval)).
1.  If the average RSSI value for the ED's current parent router is below the
    configured threshold
    ([`OPENTHREAD_CONFIG_PARENT_SEARCH_RSS_THRESHOLD`](#rss-threshold)),
    a parent search is initiated:
    1.  If the parent search discovers a better parent router, the ED dissolves
        its current Child-Parent link and initiates the [MLE
        Attach](/guides/thread-primer/network-discovery#join_an_existing_network)
        process with the new router.
    1.  If the parent search does not discover a better parent router, the
        existing Child-Parent link remains.
1.  After the parent search attempt, the ED waits to check the average RSSI
    value for its current parent router according to the configured backoff
    interval
    ([`OPENTHREAD_CONFIG_PARENT_SEARCH_BACKOFF_INTERVAL`](#backoff-interval)).
    This backoff occurs regardless of the outcome of the parent search.

Note: The parent search process may be power consuming, as the child must remain
in RX mode to receive parent responses. Use the backoff interval to limit the
impact of periodic parent searches on battery-powered devices.

We recommend enabling the [Inform Previous Parent on
Reattach](/guides/build/features/inform-previous-parent-on-reattach) feature
in conjunction with this feature.

## How to enable

This feature is disabled by default.

To enable Periodic Parent Search, define
`OPENTHREAD_CONFIG_PARENT_SEARCH_ENABLE` as `1` in the
[`/src/core/config/parent_search.h`](https://github.com/openthread/openthread/tree/main//src/core/config/parent_search.h)
file, prior to [building OpenThread](/guides/build):

```
#ifndef OPENTHREAD_CONFIG_PARENT_SEARCH_ENABLE
#define OPENTHREAD_CONFIG_PARENT_SEARCH_ENABLE 1
#endif
```
## Parameters

Use the following parameters in
[`/src/core/config/parent_search.h`](https://github.com/openthread/openthread/tree/main//src/core/config/parent_search.h)
to customize this feature:

<table class="details responsive">
  <thead>
    <th colspan="2">Parameters</th>
  </thead>
  <tbody>
    <tr>
      <td id="check-interval">OPENTHREAD_CONFIG_PARENT_SEARCH_CHECK_INTERVAL</td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td><b>Default value</b></td>
              <td>
                <div>540 seconds (9 minutes)</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the interval in seconds for a child to check the trigger condition to
perform a parent search.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="backoff-interval">OPENTHREAD_CONFIG_PARENT_SEARCH_BACKOFF_INTERVAL</td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Default value</b>
              </td>
              <td>
                <div>36000 seconds (10 hours)</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the backoff interval in seconds for a child to not perform a parent
search after triggering one.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="rss-threshold">OPENTHREAD_CONFIG_PARENT_SEARCH_RSS_THRESHOLD</td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Default value</b>
              </td>
              <td>
                <div>-65</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the RSSI threshold used to trigger a parent search.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
  </tbody>
</table>

## API

There is no public API for this feature.

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
