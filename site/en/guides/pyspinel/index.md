# Pyspinel

Pyspinel, available on the [pyspinel GitHub repository](https://github.com/openthread/pyspinel), is a Python CLI
for the [Spinel](https://openthread.io/platforms/co-processor#spinel_protocol) protocol, used to
configure and manage [OpenThread NCPs or RCPs](https://openthread.io/platforms/co-processor). This CLI is primarily
targeted to CI tests, but can be used manually to experiment with and test
OpenThread Co-Processor instances.

Pyspinel is used to:

*   Add simulated Co-Processor testing to continuous integration.
*   Automate testing of testbeds running Co-Processor firmware on hardware.
*   Debug Co-Processor builds of OpenThread.
*   Convert an OpenThread Co-Processor into a packet sniffer.

> Note: Pyspinel is only supported for NCP and RCP builds of OpenThread on Linux
or MacOS.

## Contribute

You can contribute to the ongoing development of Pyspinel by submitting bug
reports and feature requests to the [Issue Tracker](https://github.com/openthread/pyspinel/issues).

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
