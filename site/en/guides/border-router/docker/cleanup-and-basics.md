# Cleanup and Docker Basics

## Stop OTBR Docker

Use `Ctrl+C` in the terminal window running OTBR Docker to stop the process
gracefully.

If you are running a simulated RCP, also use `Ctrl+C` to stop the processes for
`socat` and the RCP node itself.

## Restart OTBR Docker

Follow the same complete procedure in [Run OTBR
Docker](../../../guides/border-router/docker/run.md) and [Test
Connectivity](../../../guides/border-router/docker/test-connectivity.md) to restart OTBR
Docker.

Upon restart, even though OTBR Docker reforms a Thread network using the network
credentials you already provided, it does not automatically push the SLAAC
addresses needed for internet connectivity and border routing. **You must reform
the Thread network through the Web GUI to ensure border routing functions are
enabled.**

If after joining a Thread node to the network it doesn't receive an on-mesh
IPv6 address, factory reset it with the `factoryreset` CLI command and
reconfigure it as detailed in [Join the second node to the Thread
network](../../../guides/border-router/docker/test-connectivity.md#join_the_second_node_to_the_thread_network).

## Docker maintenance

If you are having problems with OTBR Docker, you might have multiple containers
running. Before running OTBR Docker, we recommend cleaning up any extraneous
Docker containers, both running and stopped.

Note that there is a difference between Docker images and containers. Images are
the source, while containers are instances of the source image. You can have
multiple container instances running from the same source Docker image.

To view all stopped and running Docker containers on your machine:

```
$ docker ps -a
CONTAINER ID IMAGE        COMMAND       CREATED      STATUS  PORTS   NAMES
d09847ad66bf 43e7a898e524 "/app/bord.." 26 hours ago Exited          john.smith
```

To stop and remove a specific Docker container, use the Container ID from the
previous command:

```
$ docker stop d09847ad66bf
$ docker rm d09847ad66bf
```

To stop and remove all Docker containers at once:

```
$ docker stop $(docker ps -a -q)
$ docker rm $(docker ps -a -q)
```

To view all Docker images:
```
$ docker images
REPOSITORY           TAG          IMAGE ID           CREATED           SIZE
openthread/otbr      latest       98416559dcbd       2 weeks ago       1.15GB
```

To remove a Docker image, use the Image ID from the previous command. Note that
any stopped or running containers based on the image must be removed prior to
removing the Docker image.

```
$ docker image rm 98416559dcbd
```

> Caution: If you remove a Docker image, you'll have to rebuild it from the
Dockerfile or repull it from Docker Hub, as detailed in the [OTBR Docker
Overview](../../../guides/border-router/docker/index.md).

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
