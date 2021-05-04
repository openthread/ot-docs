# Overview

OpenThread Border Router (OTBR) provides Docker support, and can be run in a
Docker container rather than directly on your local machine.

This guide focuses on running OTBR Docker on the Raspberry Pi (RPi) or any
Linux-based machine, and has only been tested on those platforms.

> Caution: Only the commissioner included with OTBR is supported with Docker. The 
Thread Commissioning App is not supported.

## Raspberry Pi setup

Install the [**Raspberry Pi OS with
Desktop**](https://www.raspberrypi.org/downloads/raspberry-pi-os/)
OS on the RPi. Follow the instructions in the [Download and Install the
OS](../../../guides/border-router/raspberry-pi#download-and-install-the-os) step from
the Raspberry Pi Overview, but make sure to use **Raspberry Pi OS with
Desktop** as the OS. You cannot use the Lite version, as you need to access the
OTBR Web GUI in a web browser.

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
    command, modify your user settings:
    ```
    $ sudo usermod -aG docker $USER
    ```

1.  Start Docker if it is not already running:
    ```
    $ sudo dockerd
    ```

1.  Install git:
    ```
    $ sudo apt install git
    ```

## Linux setup

Use the same instructions as the RPi:

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
    command, modify your user settings:
    ```
    $ sudo usermod -aG docker $USER
    ```

1.  Start Docker if it is not already running:
    ```
    $ sudo dockerd
    ```

1.  Install git:
    ```
    $ sudo apt install git
    ```

## Mac or Windows

To use OTBR Docker on Mac or Windows, install [Docker
Toolbox](https://docs.docker.com/toolbox/). This is required as running OTBR
Docker involves mounting virtual serial ports, which is only supported by Docker
Toolbox on those systems.

> Note: This guide does not provide complete instructions for running OTBR Docker
on Mac or Windows. See [Contribute](/resources#contribute) if you are interested
in contributing this content to openthread.io.

## Get the OTBR Docker image

> Going forward, all steps apply to a supported platform, either the RPi or a Linux machine.

Get the OTBR Docker image by pulling it directly from the [OpenThread Docker
Hub](https://hub.docker.com/u/openthread/), or by cloning the OTBR repository
and building the included Dockerfile locally.

We recommend pulling the image from Docker Hub, as it has been tested and
verified by the OpenThread team.

### Pull the image from Docker Hub

This image is as of Commit ID `e80def4`.

1.  Pull the image:
    ```
    $ docker pull openthread/otbr:latest
    ```

1.  It should now appear in your list of Docker images:
    ```
    $ docker images
    REPOSITORY           TAG          IMAGE ID           CREATED           SIZE
    openthread/otbr      latest       98416559dcbd       2 weeks ago       1.15GB
    ```

### Build the Dockerfile

To create the image yourself, clone the OpenThread Border Router repository and
build the included Dockerfile.

> Note: Building the Dockerfile locally might take up to one hour, depending on
your system. For example, a Raspberry Pi with a fresh OS image takes longer
to install all dependencies than a Linux machine that might have many of them
already.

1.  Clone the OTBR repository:
    ```
    $ cd ~
    $ git clone https://github.com/openthread/ot-br-posix
    $ cd ot-br-posix
    ```

1.  Build the Dockerfile:
    ```
    $ docker build --no-cache -t openthread/otbr -f etc/docker/Dockerfile .
    ```