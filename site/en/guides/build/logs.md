# OpenThread Logs

OpenThread logs are controlled by numerous compile-time configuration constants.
Unless otherwise noted, these constants are defined in the following file:

[`/src/core/config/logging.h`](https://github.com/openthread/openthread/tree/main/src/core/config/logging.h)

Note: Platform-level configuration constants override the default core ones. If
a constant is defined at both the core and the platform level, changing the
core default will have no effect.

## Output methods

OpenThread supports different output logging methods, defined as the
compile-time configuration constant of `OPENTHREAD_CONFIG_LOG_OUTPUT`. The
logging method options are listed in the following file:

[`/src/core/config/logging.h`](https://github.com/openthread/openthread/tree/main/src/core/config/logging.h)

The default log output configuration is
`OPENTHREAD_CONFIG_LOG_OUTPUT_PLATFORM_DEFINED`.

The output method is an example of where you may need to update the
platform-level configuration constant instead of the core one. For example, to
change the output method in the sim example app, edit
`/examples/platforms/sim/openthread-core-sim-config.h` instead of
`/src/core/config/logging.h`.

## Log levels

Logs may output varying levels of information, defined as the compile-time
configuration constant of `OPENTHREAD_CONFIG_LOG_LEVEL`. The level options are
listed in the following file:

[`/include/openthread/platform/logging.h`](https://github.com/openthread/openthread/tree/main/include/openthread/platform/logging.h)

The list of log levels are also available in the [Platform
Logging Macros](/reference/group/plat-logging#macros) API reference.

The default log level is `OT_LOG_LEVEL_CRIT` which only outputs the most
critical logs. Change the level to see more logs as desired. To see all
OpenThread logs, use `OT_LOG_LEVEL_DEBG`.

> Note: Compiled code size increases the more logs that are enabled.

## Log regions

The log regions determine what areas of the OpenThread code are enabled for
logging. The region enumeration is defined in the following file:

[`/include/openthread/platform/logging.h`](https://github.com/openthread/openthread/tree/main/include/openthread/platform/logging.h)

The list of log regions are also available in the [Platform
Logging Enumerations](/reference/group/plat-logging#otlogregion) API reference.

Log regions are commonly used as parameters in log functions. All regions are
enabled by default.

## Default logging function

The default function for logging within OpenThread is `otPlatLog`, defined as
the compile-time configuration constant of
`OPENTHREAD_CONFIG_PLAT_LOG_FUNCTION`.

See the [Platform Logging](/reference/group/plat-logging#otplatlog) API
reference for more information on this function.

To use this function directly in the OpenThread example apps, use the `REFERENCE_DEVICE`
build switch. For example, to use it within the CLI app
(`examples/apps/cli/main.c`) for the nRF52840 example:

```
$ make -f examples/Makefile-nrf52840 REFERENCE_DEVICE=1
```

Alternatively, modify the `COMMONCFLAGS` variable in an [example
`Makefile`](https://github.com/openthread/openthread/tree/main/examples) to enable it by default when building.

> Note: In the simulation example app, `REFERENCE_DEVICE` is enabled by default.

## How to enable logs

Before enabling logs, make sure your environment is configured for building
OpenThread. See [Build OpenThread](/guides/build) for more information.

### Enable all logs

To quickly enable all log levels and regions, use the `FULL_LOGS` build switch:

```
$ make -f examples/Makefile-simulation FULL_LOGS=1
```

This switch sets the log level to `OT_LOG_LEVEL_DEBG` and turns on all region
flags.

### Enable a specific level of logs

To enable a specific level of logs, edit `/src/core/config/logging.h` and update
`OPENTHREAD_CONFIG_LOG_LEVEL` to the desired level, then build OpenThread. For
example, to enable logs up to `OT_LOG_LEVEL_INFO`:

```
#define OPENTHREAD_CONFIG_LOG_LEVEL OT_LOG_LEVEL_INFO
```

```
$ make -f examples/Makefile-simulation
```

### View logs in syslog

Logs are sent to the `syslog` by default. On Linux, this is `/var/log/syslog.`

1.  Build the simulation example with all logs enabled:

        $ make -f examples/Makefile-simulation FULL_LOGS=1

1.  Start a simulated node:

        $ ./output/simulation/bin/ot-cli-ftd 1

1.  In a new terminal window, set up a real-time output of the OT logs:

        $ tail -F /var/log/syslog | grep "ot-cli-ftd"

1.  On the simulated node, bring up Thread:

```
> dataset init new
Done
> dataset
Active Timestamp: 1
Channel: 13
Channel Mask: 07fff800
Ext PAN ID: d63e8e3e495ebbc3
Mesh Local Prefix: fd3d:b50b:f96d:722d/64
Master Key: dfd34f0f05cad978ec4e32b0413038ff
Network Name: OpenThread-8f28
PAN ID: 0x8f28
PSKc: c23a76e98f1a6483639b1ac1271e2e27
Security Policy: 0, onrcb
Done
> dataset commit active
Done
> ifconfig up
Done
> thread start
Done
```

Switch back to the terminal window running the `tail` command. Logs should display in real time for 
the simulated node. Note the log tags in the output: `[INFO]`, `[DEBG]`, `[NOTE]`. These all correspond 
to the [log levels](#log-levels). For example, if you change the log level to `OT_LOG_LEVEL_INFO`, 
the `DEBG` logs disappear from the output.

```
ot-cli-ftd[30055]: [1] [DEBG]-MAC-----: SrcAddrMatch - Cleared all entries
ot-cli-ftd[30055]: [1] [INFO]-CORE----: Non-volatile: Read NetworkInfo {rloc:0x9c00, extaddr:1a4aaf5e97c852de, role:Leader, mode:0x0f, keyseq:0x0, ...
ot-cli-ftd[30055]: [1] [INFO]-CORE----: Non-volatile: ... pid:0x8581bc9, mlecntr:0x3eb, maccntr:0x3e8, mliid:05e4b515e33746c8}
ot-cli-ftd[30055]: [1] [INFO]-CORE----: Notifier: StateChanged (0x7f133b) [Ip6+ Ip6- LLAddr MLAddr Rloc+ KeySeqCntr NetData Ip6Mult+ Channel PanId NetName ExtPanId MstrKey PSKc SecPolicy]
ot-cli-ftd[30055]: [1] [INFO]-CLI-----: execute command: dataset panid
ot-cli-ftd[30055]: [1] [INFO]-CLI-----: execute command: dataset commit active
ot-cli-ftd[30055]: [1] [INFO]-MESH-CP-: Active dataset set
ot-cli-ftd[30055]: [1] [DEBG]-MAC-----: Idle mode: Radio sleeping
ot-cli-ftd[30055]: [1] [DEBG]-MAC-----: RadioPanId: 0x8f28
ot-cli-ftd[30055]: [1] [INFO]-CORE----: Notifier: StateChanged (0x007f0100) [KeySeqCntr Channel PanId NetName ExtPanId MstrKey PSKc SecPolicy]
ot-cli-ftd[30055]: [1] [INFO]-CLI-----: execute command: ifconfig up
ot-cli-ftd[30055]: [1] [DEBG]-MAC-----: Idle mode: Radio receiving on channel 11
ot-cli-ftd[30055]: [1] [INFO]-CLI-----: execute command: thread start
ot-cli-ftd[30055]: [1] [NOTE]-MLE-----: Role Disabled -> Detached
ot-cli-ftd[30055]: [1] [INFO]-MLE-----: Attempt to become router
ot-cli-ftd[30055]: [1] [INFO]-CORE----: Non-volatile: Read NetworkInfo {rloc:0x9c00, extaddr:1a4aaf5e97c852de, role:Leader, mode:0x0f, keyseq:0x0, ...
ot-cli-ftd[30055]: [1] [INFO]-CORE----: Non-volatile: ... pid:0x8581bc9, mlecntr:0x3eb, maccntr:0x3e8, mliid:05e4b515e33746c8}
ot-cli-ftd[30055]: [1] [INFO]-CORE----: Non-volatile: Saved NetworkInfo {rloc:0x9c00, extaddr:1a4aaf5e97c852de, role:Leader, mode:0x0f, keyseq:0x0, ...
ot-cli-ftd[30055]: [1] [INFO]-CORE----: Non-volatile: ... pid:0x8581bc9, mlecntr:0x7d4, maccntr:0x7d0, mliid:05e4b515e33746c8}
ot-cli-ftd[30055]: [1] [DEBG]-MLE-----: Store Network Information
ot-cli-ftd[30055]: [1] [INFO]-MLE-----: Send Link Request (ff02:0:0:0:0:0:0:2)
```

### View logs in the CLI app

Logs may be viewed directly in the OpenThread CLI example app.

1.  Edit the configuration file for the example platform and change the log
    output to the app. For the simulation example, this is
    `/examples/platforms/sim/openthread-core-sim-config.h`:

        #define OPENTHREAD_CONFIG_LOG_OUTPUT OPENTHREAD_CONFIG_LOG_OUTPUT_APP

1.  Build the simulation example with the desired level of logs. To enable all
    logs:

        $ make -f examples/Makefile-simulation FULL_LOGS=1

1.  Start a simulated node:

        $ ./output/simulation/bin/ot-cli-ftd 1

1.  You should see log output in the same window as the OpenThread CLI as
    commands are processed.

If you have added custom logging and enabled all logs, the UART transmit buffer
may not be large enough to handle the additional custom logs. If some logs are
not appearing when they should, try increasing the size of the UART transmit
buffer, defined as `OPENTHREAD_CONFIG_CLI_UART_TX_BUFFER_SIZE` in
`/src/cli/cli_config.h` or the platform's configuration file, such as
`/examples/platforms/nrf528xx/nrf52840/openthread-core-nrf52840-config.h`.

### View logs for an NCP

Logs for an NCP may be sent through `wpantund` to the `syslog` of a host. For a
Linux host, this is `/var/log/syslog.`

Use an `OPENTHREAD_CONFIG_LOG_OUTPUT` value of
`OPENTHREAD_CONFIG_LOG_OUTPUT_NCP_SPINEL` to enable NCP logging. Change this in
the platform's configuration file.

For example, to enable this for an nrf52840 connected to a Linux host:

1.  Edit the configuration file for the platform and change the log output to
    NCP Spinel. For nrf52840, this is
    `/examples/platforms/nrf528xx/nrf52840/openthread-core-nrf52840-config.h`:

        #define OPENTHREAD_CONFIG_LOG_OUTPUT OPENTHREAD_CONFIG_LOG_OUTPUT_NCP_SPINEL

1.  Build the nrf52840 example with the desired level of logs and other
    NCP-specific flags. To build a joiner with all logs enabled:

        $ make -f examples/Makefile-nrf52840 JOINER=1 USB=1 FULL_LOGS=1

1.  Flash the NCP, connect it to the Linux host, and start `wpantund` as
    detailed in the [OpenThread Hardware Codelab](https://openthread.io/codelabs/openthread-hardware/#3).

1.  Once the NCP is running, check the `syslog` on the Linux machine:

        $ tail -F /var/log/syslog | grep "ot-ncp-ftd"

1.  You should see OpenThread logs display in real time for the NCP. You may
    also see them in the `wpantund` output.

### Change the log level at run time

Log levels may be changed at run time if dynamic log level control is enabled.

1.  Edit `/src/core/config/logging.h` and set
    `OPENTHREAD_CONFIG_ENABLE_DYNAMIC_LOG_LEVEL` to `1`.

1.  Change the log level depending on your implementation:
    1.  For a [system-on-chip (SoC)](/platforms#single-chip-thread-only-soc),
        use the [Logging API](/reference/group/api-logging) within your
        OpenThread application.
    1.  For an NCP, use `wpanctl` on the command line. See [`wpan-properties.h`](https://github.com/openthread/wpantund/tree/master/src/wpantund/wpan-properties.h) in the `wpantund` repository for all the properties exposed to `wpanctl` and the  [Spinel API](https://github.com/openthread/openthread/blob/da12dca4d9e82cda08aba272567affa188bf8b12/src/ncp/spinel.h#L308) for its log level definitions.

            $ wpanctl set OpenThread:LogLevel 5

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
