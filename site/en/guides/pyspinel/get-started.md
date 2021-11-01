# Get started

The quickest way to get started with Pyspinel is to try out the CLI:

1.  First, [Install Pyspinel and dependencies without extcap](install-pyspinel.md).
1.  Directly on your machine, clone and build a simiulated OpenThread NCP as
    described in [How to build OpenThread](../build/index.md#how_to_build_openthread). After cloning and
    bootstraping, build the sim example:

    ```
    $ make -f examples/Makefile-simulation
    ```

1.  Run the Pyspinel CLI, using the path to your simulated build:

    ```
    $ cd {path-to-pyspinel}
    $ spinel-cli.py -p {path-to-openthread}/output/simulation/bin/ot-ncp-ftd -n 1
    spinel-cli >
    ```

1.  Verify the OpenThread version:

    ```
    spinel-cli > version
    OPENTHREAD/20180926-01310-g9fdcef20; SIMULATION; Feb 11 2020 14:09:56
    ```

1.  Start Thread on the simulated NCP and verify that it has become the leader
    in a Thread network:

    ```
    spinel-cli > panid 1234
    Done
    spinel-cli > ifconfig up
    Done
    spinel-cli > thread start
    Done
    spinel-cli > state
    leader
    Done
    ```

1.  View the help menu to see what commands are available:

    ```
    spinel-cli > help
    ```

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
