# Overview

OpenThread Border Router (OTBR) provides Docker support, and can be run in a
Docker container rather than directly on your local machine.

This guide focuses on running OTBR Docker on the Raspberry Pi (RPi).

## Raspberry Pi setup

Install the [**Raspberry Pi OS with
Desktop**](https://www.raspberrypi.org/downloads/raspberry-pi-os/)
OS on the RPi. Both Desktop and Lite versions will work.

Once installed, boot up the RPi and open a terminal window.

1.  Update the system:
    ```
    $ sudo apt-get update
    $ sudo apt-get upgrade
    ```

1.  Install Docker:

    ```
    $ curl -sSL https://get.docker.com | sh
    ```

1.  If you want to use Docker as non-root, without requiring `sudo` before each
    command, modify your user settings. Sign out for the changes to take effect:
    ```
    $ sudo usermod -aG docker $USER
    ```

1.  Start Docker if it is not already running:
    ```
    $ sudo dockerd
    ```

1.  Enable IP forwarding.

    Linux typically disables IP forwarding by default. Run the
    [`setup-host`](https://raw.githubusercontent.com/openthread/ot-br-posix/refs/heads/main/etc/docker/border-router/setup-host)
    script to enable IP forwarding on the host system.
    ```
    $ curl -sSL https://raw.githubusercontent.com/openthread/ot-br-posix/refs/heads/main/etc/docker/border-router/setup-host | bash
    ```

## Get the OTBR Docker image

Get the OTBR Docker image by pulling it directly from the [OpenThread Docker
Hub](https://hub.docker.com/u/openthread/), or by cloning the OTBR repository
and building the included Dockerfile locally.

We recommend pulling the image from Docker Hub, as it has been tested and
verified by the OpenThread team.

### Pull the image from Docker Hub

1.  Pull the image:
    ```
    $ docker pull openthread/border-router:latest
    ```

1.  It should now appear in your list of Docker images:
    ```
    $ docker images
    REPOSITORY                 TAG       IMAGE ID       CREATED       SIZE
    openthread/border-router   latest    08666d77013d   2 hours ago   171MB
    ```

### Build the Dockerfile

To create the image yourself, clone the OpenThread Border Router repository and
build the included Dockerfile.

1.  Install git:
    ```
    $ sudo apt install git
    ```

1.  Clone the OTBR repository:
    ```
    $ cd ~
    $ git clone https://github.com/openthread/ot-br-posix
    $ cd ot-br-posix
    ```

1.  Build the Dockerfile:
    ```
    $ docker build --no-cache -t openthread/border-router -f etc/docker/border-router/Dockerfile .
    ```

## License

Copyright (c) 2021-2025, The OpenThread Authors.
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

