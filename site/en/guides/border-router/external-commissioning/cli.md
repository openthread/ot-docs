#  OT Commissioner CLI

External commissioning is supported by the OT Commissioner CLI, available
on the [ot-commissioner GitHub repository](https://github.com/openthread/ot-commissioner).

In this guide, you'll build and install OT Commissioner and commission a Joiner.

## Set up OT Commissioner

To use OT Commissioner CLI, first [Build OT Commissioner](https://openthread.io/guides/commissioner/build).

> Note: OT Commissioner must be running on the same host machine as OTBR.

## Discover your network

OTBR registers a `_meshcop._udp` service named `OpenThread_BorderRouter`. This
service is configured in the [OTBR CMakeLists.txt file](https://github.com/openthread/ot-br-posix/blob/main/CMakeLists.txt#L36).

To start OT Commissioner, you'll need to find the port number of your border
agent service. There are several ways to do this, for example:

*   Use `dns-sd` to browse for a service on `raspberrypi.local.:49155`:

    ```
    $ dns-sd -L "OpenThread_BorderRouter" _meshcop._udp local
    ```

    <pre>
    Lookup OpenThread_BorderRouter._meshcop._udp.local
    DATE: ---Tue 16 Nov 2021---
    13:31:03.197  ...STARTING...
    13:31:03.350  OpenThread_BorderRouter._meshcop._udp.local. can be reached at raspberrypi.local.:49155 (interface 3)
    </pre>

*   Use `avahi-utils`:

    ```
    $ avahi-browse -r _meshcop._udp
    ```

> Note: To support multiple Thread interfaces on a single host, the border agent
service uses an ephemeral port by default. This means that your port number
might change when you restart `otbr-agent` or your OTBR platform.

## Connect to the Border Router

1.  Open the Non-CCM configuration file in your installation directory:

    ```
    $ cd /usr/local/etc/commissioner/non-ccm-config.json
    ```

1.  Change the `PSKc` to `198886f519a8fd7c981fee95d72f4ba7`:

    ```
    "PSKc" : "198886f519a8fd7c981fee95d72f4ba7"
    ```

    _Optional_. Configure logging. When you run `pi@raspberrypi: commissioner-cli`
    from the command line, OT Commissioner creates a `commissioner.log` file in
    the current working directory, for example `/home/pi/commissioner.log`. In
    the `non-ccm-config.json` file, you can configure your `LogFile` path,
    logging level, and other log settings.

1.  Start the OT Commissioner CLI with the Non-CCM configuration:

    ```
    $ commissioner-cli /usr/local/etc/commissioner/non-ccm-config.json
    > 
    ```

1.  Connect to OTBR, providing your OTBR port:

    ```
    > start :: 49155
    [done]
    >
    ```

1.  Verify that the Commissioner is active:

    ```
    > active
    true
    [done]
    > 
    ```

## Commission the Joiner

Once connected to the Border Router, OT Commissioner can commission the Joiner
device.

1.  In OT Commissioner, enable Thread MeshCoP joiner for all Joiners with a
    password of `J01NU5`:
    ```
    > joiner enableall meshcop J01NU5
    [done]
    > 
    ```

1.  On the Joiner device, start the Joiner role with the password configured in
    OT Commissioner:
    ```
    > ifconfig up
    Done
    > joiner start J01NU5
    Done
    ```

1.  Wait a minute for the DTLS handshake to complete between the Commissioner
    and Joiner:
    ```
    > 
    Join success!
    ```

## Join the Thread network

Next, on the Joiner device, [join the Thread network](join.md) and test network
connectivity.

## Troubleshooting

If you're having issues with OT Commissioner, make sure you use `stop`, then
`exit` to properly end a session:

```
> stop
[done]
> exit
```

**IO_ERROR: NET - Reading information from the socket failed**

The socket APIs return this error message when a call to bind or connect to OTBR
fails. If you're receiving this error message, try the following:

*   Make sure that you're passing the correct port number when you start OT
    Commissioner. OTBR may use a different port after it's restarted or you
    reboot your platform.
*   Make sure that OTBR is running and that your Thread network is properly
    configured, including your PSKc. Your Passphrase/Commissioner Credential
    must be a string between 6 and 255 characters.
*   Check your global IP addresses, for example `ifconfig eth0`. You may be
    using the wrong IP address to start OT Commissioner.

**SECURITY: SSL - A fatal alert message was received from our peer**

OT Commissioner establishes a secure DTLS session with the border agent service.
A fatal SSL error typically indicates that the secure DTLS session fails. If you
receive this message, try the following:

*   Check the `commissioner.log`, if available.
*   Make sure that OT Commissioner is properly configured when you pass your
    Non-CCM or CCM Thread network JSON file.
*   If OT Commissioner wasn't shut down properly or you disconnected from an SSH
    session, you may need to restart the app or your platform.

## Resources

For additional `commissioner-cli` commands, refer to [OT Commissioner CLI](https://github.com/openthread/ot-commissioner/blob/main/src/app/cli/README.md).

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
