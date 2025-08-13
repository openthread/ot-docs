# OpenThread Logs

OpenThread logs are controlled by numerous compile-time configuration constants.
Unless otherwise noted, these constants are defined in the following file:

[`openthread/src/core/config/logging.h`](https://github.com/openthread/openthread/tree/main/src/core/config/logging.h)

Note: Platform-level configuration constants override the default core ones. If
a constant is defined at both the core and the platform level, changing the
core default will have no effect.

## Output methods

OpenThread supports different output logging methods, defined as the
compile-time configuration constant of `OPENTHREAD_CONFIG_LOG_OUTPUT`. The
logging method options are listed in the following file:

[`openthread/src/core/config/logging.h`](https://github.com/openthread/openthread/tree/main/src/core/config/logging.h)

The default log output configuration is
`OPENTHREAD_CONFIG_LOG_OUTPUT_PLATFORM_DEFINED`.

The output method is an example of where you may need to update the
platform-level configuration constant instead of the core one. For example, to
change the output method in the simulation example app, edit
`openthread/examples/platforms/simulation/openthread-core-simulation-config.h` instead of
`openthread/src/core/config/logging.h`.

## Log levels

Logs may output varying levels of information, defined as the compile-time
configuration constant of `OPENTHREAD_CONFIG_LOG_LEVEL`. The level options are
listed in the following file:

[`openthread/include/openthread/platform/logging.h`](https://github.com/openthread/openthread/tree/main/include/openthread/platform/logging.h)

The list of log levels are also available in the [Platform
Logging Macros](https://openthread.io/reference/group/plat-logging#macros) API reference.

The default log level is `OT_LOG_LEVEL_CRIT` which only outputs the most
critical logs. Change the level to see more logs as desired. To see all
OpenThread logs, use `OT_LOG_LEVEL_DEBG`.

> Note: Compiled code size increases the more logs that are enabled.

## Log regions

The log regions determine what areas of the OpenThread code are enabled for
logging. The region enumeration is defined in the following file:

[`openthread/include/openthread/platform/logging.h`](https://github.com/openthread/openthread/tree/main/include/openthread/platform/logging.h)

The list of log regions are also available in the [Platform
Logging Enumerations](https://openthread.io/reference/group/plat-logging#otlogregion) API reference.

Log regions are commonly used as parameters in log functions. All regions are
enabled by default.

## Default logging function

The default function for logging within OpenThread is `otPlatLog`, defined as
the compile-time configuration constant of
`OPENTHREAD_CONFIG_PLAT_LOG_FUNCTION`.

See the [Platform Logging](https://openthread.io/reference/group/plat-logging#otplatlog) API
reference for more information on this function.

To use this function directly in the OpenThread example apps, use the `OT_REFERENCE_DEVICE`
cmake option. For example, to use it within the CLI app for the CC2538 example:

```
$ ./script/build -DOT_REFERENCE_DEVICE=ON
```

Alternatively, update the [`openthread/etc/cmake/options.cmake`](https://github.com/openthread/openthread/blob/main/etc/cmake/options.cmake) file to enable it by default when building.

## How to enable logs

Before enabling logs, make sure your environment is configured for building
OpenThread. See [Build OpenThread](../../guides/build/index.md) for more information.

### Enable all logs

To quickly enable all log levels and regions, use the `OT_FULL_LOGS` cmake option:

```
$ ./script/build -DOT_FULL_LOGS=ON
```

This switch sets the log level to `OT_LOG_LEVEL_DEBG` and turns on all region
flags.

### Enable a specific level of logs

To enable a specific level of logs, edit `openthread/src/core/config/logging.h` and update
`OPENTHREAD_CONFIG_LOG_LEVEL` to the desired level, then build OpenThread. For
example, to enable logs up to `OT_LOG_LEVEL_INFO`:

```
#define OPENTHREAD_CONFIG_LOG_LEVEL OT_LOG_LEVEL_INFO
```

```
$ ./script/build
```

### View logs in syslog

Logs are sent to the `syslog` by default. On Linux, this is `/var/log/syslog.`

1.  Build the simulation example with all logs enabled:

        $ cd openthread
        $ ./script/cmake-build simulation -DOT_FULL_LOGS=ON

1.  Start a simulated node:

        $ ./build/simulation/examples/apps/cli/ot-cli-ftd 1

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
Network Key: dfd34f0f05cad978ec4e32b0413038ff
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
    `openthread/examples/platforms/simulation/openthread-core-simulation-config.h`:

        #define OPENTHREAD_CONFIG_LOG_OUTPUT OPENTHREAD_CONFIG_LOG_OUTPUT_APP

1.  Build the simulation example with the desired level of logs. To enable all
    logs:

        $ ./script/cmake-build simulation -DOT_FULL_LOGS=ON

1.  Start a simulated node:

        $ ./build/simulation/examples/apps/cli/ot-cli-ftd 1

1.  You should see log output in the same window as the OpenThread CLI as
    commands are processed.

If you have added custom logging and enabled all logs, the CLI line buffer or the UART transmit 
buffer may not be large enough to handle the additional custom logs. If some logs are not 
appearing when they should, try increasing the size of the CLI line
buffer, defined as `OPENTHREAD_CONFIG_CLI_MAX_LINE_LENGTH` in
`/openthread/src/cli/cli_config.h` or increasing the size of the UART transmit buffer, defined as
`OPENTHREAD_CONFIG_CLI_UART_TX_BUFFER_SIZE` in the platform's configuration file, such as
`/src/nrf52840/openthread-core-nrf52840-config.h`.

### Change the log level at run time

Log levels may be changed at run time if dynamic log level control is enabled.

1.  Build the app with option `-DOT_LOG_LEVEL_DYNAMIC=ON`. For example,

        $ ./script/build nrf52840 UART_trans -DOT_JOINER=ON -DOT_FULL_LOGS=ON -DOT_LOG_LEVEL_DYNAMIC=ON

1.  Use the [Logging API](https://openthread.io/reference/group/api-logging) within your
    OpenThread application.

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
