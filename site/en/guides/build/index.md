# Build OpenThread

## How to build OpenThread

The steps to build OpenThread vary depending on toolchain, user machine, and
target platform.

The most common workflow is:

1.  Set up the build environment and install the desired toolchain:
    1.  **To build directly on a machine,** see the [Simulation Codelab](https://openthread.io/codelabs/openthread-simulation-posix/index.html?index=..%2F..index#1) for detailed instructions
    1.  **To use a Docker container with a pre-configured environment,**
        download and run the OpenThread `environment` image:

            $ docker pull openthread/environment:latest
            $ docker run -it --rm openthread/environment bash

1.  Within your chosen environment, clone the platform-specific OpenThread Git repository. Taking CC2538 as an example:

           $ git clone https://github.com/openthread/ot-cc2538.git --recursive

1.  From the cloned repository's root directory:
    1.  Install the toolchain:

            $ ./script/bootstrap

    1.  Build the configuration:

            $ ./script/build {platform-specific-args} {cmake-options}

1.  Flash the desired binary to the target platform. All generated binaries are
    located in `./build/bin`.

> Note: Between builds in the same repository, run `rm -r build/` to ensure a clean build each time.

### Configuration

You can configure OpenThread for different functionality and behavior during the
build process. Available configuration options are detailed at the following
locations:

Type | Location
---- | ----
Compile-time constants | Listed in all the header files in [`/src/core/config`](https://github.com/openthread/openthread/tree/main/src/core/config)
cmake build options | Listed in [`openthread/examples/README.md`](https://github.com/openthread/openthread/blob/main/examples/README.md)

> Note: Each platform repository specifies some, but not all, of the constants and flags that the platform supports. Modify the platform's `openthread-core-{platform}-config.h` file to enable or disable compile-time constants prior to building.

### Build examples

Use cmake build options to enable functionality for the platform. For example, to
build the binary for the CC2538 platform with Commissioner and Joiner support enabled:

```
$ ./script/build -DOT_COMMISSIONER=ON -DOT_JOINER=ON
```

Or, to build the nRF52840 platform with the [Jam Detection
feature](../../guides/build/features/jam-detection.md) enabled in its repo:

```
$ ./script/build nrf52840 UART_trans -DOT_JAM_DETECTION=ON
```

### Binaries

The following binaries are generated in `./build/bin` from the build process. To determine which binaries are generated, use flags with the `./script/build` command. For example, to build OpenThread and generate only the FTD CLI binary:

```
$ ./script/build -DOT_APP_CLI=ON -DOT_FTD=ON -DOT_MTD=OFF -DOT_APP_NCP=OFF -DOT_APP_RCP=OFF -DOT_RCP=OFF
```

Binary | Description | Options
---- | ---- | ----
`ot-cli-ftd` | Full Thread device for SoC designs | `-DOT_APP_CLI=ON`<br/> `-DOT_FTD=ON`
`ot-cli-mtd` | Minimal Thread device for SoC designs | `-DOT_APP_CLI=ON`<br/> `-DOT_MTD=ON`
`ot-ncp-ftd` | Full Thread device for Network Co-Processor (NCP) designs | `-DOT_APP_NCP=ON`<br/> `-DOT_FTD=ON`
`ot-ncp-mtd` | Minimal Thread device for NCP designs | `-DOT_APP_NCP=ON`<br/> `-DOT_MTD=ON`
`ot-rcp` | Radio Co-Processor (RCP) design | `-DOT_APP_RCP=ON`<br/> `-DOT_RCP=ON`

By default, all above flags are enabled. If you explicitly disable all flags, applications are not
built but OpenThread library files are still generated in `./build/lib` for use in a project.

Check the example Makefiles for each platform to see which flags each platform
supports. For more information on FTDs and MTDs, see the
[Thread Primer](../../guides/thread-primer/node-roles-and-types.md#device_types). For
more information on SoC and NCP designs, see [Platforms](https://openthread.io/platforms/).

The process to flash these binaries varies across example platforms. See the
READMEs in each platform's
[example folder](https://github.com/openthread/openthread/tree/main/examples/platforms) for detailed instructions.

### OpenThread Daemon

OpenThread Daemon (OT Daemon) is an OpenThread POSIX build mode that runs
OpenThread as a service and is used with the RCP design. For more information on
how to build and use it, see [OpenThread Daemon](https://openthread.io/platforms/co-processor/ot-daemon).

## Build Support Packages

Build Support Packages (BSPs)  are found in
[`/third_party`](https://github.com/openthread/openthread/tree/main/third_party). BSPs are additional third-party code used by OpenThread on each respective platform, generally included when [porting OpenThread](../../guides/porting/index.md) to a new hardware platform.

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
