# Cleanup and Docker Basics

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
REPOSITORY                 TAG       IMAGE ID       CREATED       SIZE
openthread/border-router   latest    08666d77013d   2 hours ago   171MB
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
