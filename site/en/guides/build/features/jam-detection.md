# Jam Detection

To provide a configurable mechanism for signal jamming detection on any
OpenThread node, enable the Jam Detection feature.

This feature is useful for device certifications that require the ability to
detect signal jamming on a specific channel. It can be configured to meet the
requirements of each type of certification.

## How it works

Jam Detection monitors the RSSI (received signal strength indicator) of a node
during specified windows of time to determine whether the channel has been
jammed.

When Jam Detection is enabled:

1.  The Jam Detection State is set to `false`.
1.  The node samples the RSSI multiple times over each one second interval.
1.  If the RSSI over that entire one second interval remains above the
    configured [RSSI Threshold](#threshold) for every sample, that one second
    interval is considered jammed.
1.  If an aggregate number of jammed one second intervals is _more than or_
    _equal to_ the aggregate number of configured [Busy Period](#busy-period)
    seconds within the preceding configured [Detection Window](#window) seconds
    at any point in time, the Jam Detection State at that point in time is set
    to `true`.
1.  If an aggregate number of jammed one second intervals is _less than_ the
    aggregate number of configured [Busy Period](#busy-period) seconds within
    the preceding configured [Detection Window](#window) seconds at any point in
    time, the Jam Detection State at that point in time is set to `false`.

### History Bitmap

In the [OpenThread API](#openthread) and [`wpantund` properties](#wpantund), a
bitmap of the preceding 63 seconds is available for retrieval. This bitmap
indicates whether the RSSI crossed the configured RSSI Threshold at each of the
preceding 63 seconds.

For example, you might retrieve the following bitmap:

```
0xC248068C416E7FF0
```

Converting to binary produces every instance the RSSI went above the configured
RSSI Threshold during the preceding 63 seconds:

```
11000010 01001000 00000110 10001100 01000001 01101110 01111111 11110000
```

If the Detection Window is set to 16 seconds, and the Busy Period is set to 8
seconds, the Jam Detection State becomes `true` at 51 seconds, as that is the
first instance where the RSSI Threshold was exceeded at least 8 entire seconds
in the preceding 16 seconds. In this example, the Jam Detection State remains
`true` for the next 13 seconds.

```
11000010 01001000 00000110 10001100 01000001 01101110 011{11111 11110000}
                                      [00001 01101110 011] = 8 in 16
```

This bitmap might be represented by the following graph, if -45 dBm was the
configured RSSI Threshold:

<figure>
<a href="/guides/images/ot-jam-detection-graph_2x.png"><img src="/guides/images/ot-jam-detection-graph.png" srcset="/guides/images/ot-jam-detection-graph.png 1x, /guides/images/ot-jam-detection-graph_2x.png 2x" border="0" alt="OT Jam Detection" /></a>
</figure>

## How to enable

This feature is disabled by default.

### By define

To enable Jam Detection, define
`OPENTHREAD_CONFIG_JAM_DETECTION_ENABLE` as `1` in the
[`/src/core/config/openthread-core-default-config.h`](https://github.com/openthread/openthread/tree/main/src/core/config/openthread-core-default-config.h)
file, prior to [building OpenThread](/guides/build):

```
#ifndef OPENTHREAD_CONFIG_JAM_DETECTION_ENABLE
#define OPENTHREAD_CONFIG_JAM_DETECTION_ENABLE 1
#endif
```

### By switch

Alternatively, use the `JAM_DETECTION=1` build switch when [building
OpenThread](/guides/build):

```
$ make -f examples/Makefile-{platform} JAM_DETECTION=1
```

## Parameters

Jam Detection parameters can only be configured through the OpenThread API, the
Spinel protocol, or `wpanctl`, the `wpantund` command line tool for Network
Co-Processor (NCP) management. Default values are applied if the feature is
enabled without subsequent configuration.

Customize this feature using the following parameters:

<table class="details responsive">
  <thead>
    <th colspan="2">Parameters</th>
  </thead>
  <tbody>
    <tr>
      <td id="threshold"><i>RSSI Threshold</i></td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td><b>Default value</b></td>
              <td>
                <div>0 dBm</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the threshold RSSI level in dBm above which to consider the channel jammed.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="window"><i>Detection Window</i></td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Default value</b>
              </td>
              <td>
                <div>63 seconds</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the window in seconds in which to check for signal jamming. Range: 1-63.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="busy-period"><i>Busy Period</i></td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Default value</b>
              </td>
              <td>
                <div>63 seconds</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the number of aggregate seconds within the Detection Window in which the RSSI must be above the RSSI Threshold to trigger Jam Detection. Must be smaller than Detection Window. Range: 1-63.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
  </tbody>
</table>

## API

### OpenThread

Use the [Jam Detection API](/reference/group/api-jam-detection) to
manage the Jam Detection feature directly in your OpenThread application. The
OpenThread API provides the following functionality:

*   Start and stop the feature
*   View the Jam Detection State
*   Manage all parameters
*   Retrieve the current Jam Detection history bitmap
*   Register a callback function for when a jam is detected

### Spinel

The Spinel protocol enables a host device to communicate directly with an NCP.
This protocol exposes Jam Detection properties in
[`/src/lib/spinel/spinel.h`](https://github.com/openthread/openthread/tree/main/src/lib/spinel/spinel.h)
that provide the following functionality:

*   Start and stop the feature
*   View the Jam Detection State
*   Manage all parameters
*   Retrieve the current Jam Detection history bitmap

## CLI

### OpenThread

There are no OpenThread CLI commands related to this feature.

### wpantund

Use the `wpanctl` CLI to manage the Jam Detection feature for an OpenThread NCP
configuration. `wpantund` retains all Jam Detection configuration upon NCP
reset.

`wpanctl` provides access to the following `wpantund` properties:

<table class="details responsive">
  <thead>
    <th colspan="2">Properties</th>
  </thead>
  <tbody>
    <tr>
      <td id="wpan-status"><code>JamDetection:Status</code></td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Format</b>
              </td>
              <td>
                <div>boolean</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Read only. Jam Detection State. Indicates if a signal jam is currently detected.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="wpan-enable"><code>JamDetection:Enable</code></td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Format</b>
              </td>
              <td>
                <div>boolean</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Enable or disable the Jam Detection feature.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="wpan-threshold"><code>JamDetection:RssiThreshold</code></td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Format</b>
              </td>
              <td>
                <div>dBm</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the threshold RSSI level in dBm above which to consider the channel blocked.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="wpan-window"><code>JamDetection:Window</code></td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Format</b>
              </td>
              <td>
                <div>seconds</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the window in seconds in which to check for signal jamming. Range: 1-63.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="wpan-busy-period"><code>JamDetection:BusyPeriod</code></td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Format</b>
              </td>
              <td>
                <div>seconds</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Specifies the number of aggregate seconds within <code>JamDetection:Window</code> in which the RSSI must be above <code>JamDetection:RssiThreshold</code> to trigger Jam Detection. Must be smaller than <code>JamDetection:Window</code>. Range: 1-63.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td id="wpan-bitmap"><code>JamDetection:Debug:HistoryBitmap</code></td>
      <td>
        <table class="function param responsive">
          <tbody>
            <tr>
              <td>
                <b>Format</b>
              </td>
              <td>
                <div>64-bit value</div>
              </td>
            </tr>
            <tr>
              <td>
                <b>Description</b>
              </td>
              <td>
                <div>Provides information about the Jam Detection State history for monitoring and debugging purposes.</div>
              </td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
  </tbody>
</table>

For example, to get the Jam Detection State for an NCP:

```
$ sudo wpanctl getprop JamDetection:Status
JamDetection:Status = false
```

To set the Jam Detection RSSI Threshold to -45 dBm on an NCP:

```
$ sudo wpanctl setprop JamDetection:RssiThreshold -45
$ sudo wpanctl getprop JamDetection:RssiThreshold
JamDetection:RssiThreshold = -45
```

For more information on `wpantund` properties, see the [`wpantund` GitHub
repository](https://github.com/openthread/wpantund/tree/master/src/wpantund/wpan-properties.h).

> Note: To learn more about managing an NCP using `wpantund` and `wpanctl`, see the [Build a Thread Network Codelab](https://openthread.io/codelabs/openthread-hardware/).

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
