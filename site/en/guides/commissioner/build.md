# OpenThread Commissioner Build and Configuration

This guide covers the basic build and configuration of OpenThread Commissioner
(OT Commissioner). Upon completion of this procedure, you'll have an OT
Commissioner CLI executable and a static Commissioner library.

## Step 1: Set up OT Commissioner

1.  Clone the OT Commissioner repository:
    ```
    $ git clone https://github.com/openthread/ot-commissioner
    ```

1.  Install dependencies:
    ```
    $ cd ot-commissioner
    $ ./script/bootstrap.sh
    ```

## Step 2: Build OT Commissioner

OT Commissioner installs to the `/usr/local` directory. If you'd like to change
your installation directory, set `-DCMAKE_INSTALL_PREFIX`.

> Note: If you change the default installation directory, you'll need to update
`$PATH` to run `commissioner-cli`. This guide uses an Environment variable to
work for both default and custom installations.

1.  Build OT Commissioner:
    ```
    $ mkdir build
    $ cd build
    $ cmake -DCMAKE_INSTALL_PREFIX={/usr/local} -GNinja ..
    $ ninja -j1
    ```

1.  Create an Environment variable to run `commissioner-cli` in the next step:
    ```
    $ export COMMISSIONER_CLI={/usr/local}/bin/commissioner-cli
    ```

1.  _Optional_. Run unit tests:
    ```
    $ ./tests/commissioner-test
    ```

## Step 3: Install OT Commissioner

OT Commissioner installs the following to your installation directory:

*   OT Commissioner library and header files
*   OT Commissioner CLI executable binary
*   Default configuration files and credentials
*   Scripts to run OT Commissioner CLI as daemon

```
$ sudo ninja install
```

Verify the installation by checking the help menu.

```
$ $COMMISSIONER_CLI -h
```

If you installed to the `/usr/local` directory, `commissioner-cli` is available
from the command line.

```
$ commissioner-cli -h
```

## Step 3: Configuration

The OT Commissioner CLI supports both Thread 1.2 Commercial Commissioning Mode
(CCM) and Thread 1.1 commissioning (Non-CCM). To connect to different Thread
networks, a JSON configuration file is needed to start OT Commissioner CLI:

*   `ccm-config.json` — The default configuration file for CCM Thread Network.
*   `non-ccm-config.json` — The default configuration file for Non-CCM Thread
    Network.

By default, these configuration files are installed in `/usr/local/etc/commissioner`. You can
also view sample files on the [ot-commissioner GitHub repository](https://github.com/openthread/ot-commissioner/tree/main/src/app/etc/commissioner).

### CCM configuration

To connect to a CCM Thread network, update these fields in `ccm-config.json`:

Field | Description
----|----
`DomainName` | Unique identifier within the Enterprise Domain.
`PrivateKeyFile` | The private key file in PEM format.
`CertificateFile` | The certificate file in PEM format.
`TrustAnchorFile` | The trust anchor file in PEM format.

These key and certificate files are used to establish secure sessions between
the Commissioner and Border Agent.

> Note: If you changed the default installation directory, you'll need to
update `"ThreadSMRoot" : "/usr/local/etc/commissioner"`.

### Non-CCM configuration

The Pre-Shared Key `PSKc` is used to establish a secure session between the
Commissioner and Border Agent. To connect to a Non-CCM Thread network, update
`PSKc` in `non-ccm-config.json`.

> Note: If you changed the default installation directory, you'll need to
update `"ThreadSMRoot" : "/usr/local/etc/commissioner"`.

## Commission a joiner

To use OT Commissioner to commission a joiner, refer to [External
Commissioning](../border-router/external-commissioning/index.md).

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
