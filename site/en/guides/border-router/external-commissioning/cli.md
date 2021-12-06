# OT Commissioner CLI

External commissioning is supported by the OT Commissioner CLI, available
on the [ot-commissioner GitHub repository](https://github.com/openthread/ot-commissioner).

In this guide, you'll build and install OT Commissioner and commission a Joiner.

## Set up OT Commissioner

To use OT Commissioner CLI, first [Build OT Commissioner](../../commissioner/build.md).

## Discover your network

To start OT Commissioner, you'll need to find the IP address and port number of
your border agent service. For help on locating this information, refer to
[mDNS Discovery](../mdns-discovery.md).

> Note: To support multiple Thread interfaces on a single host, the border agent
service uses an ephemeral port by default. This means that your port number
might change when you restart `otbr-agent` or your OTBR platform.

## Connect to the Border Router

1.  Start the OT Commissioner CLI:

    ```
    $ commissioner-cli
    > 
    ```

1.  Set your PSKc:

    ```
    > config set pskc 198886f519a8fd7c981fee95d72f4ba7
    [done]
    ```

1.  Connect to OTBR, providing your mDNS IP address and port:

    ```
    > start FD00::74D0:6FC9:6BE6:3582 49155
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

If you're having issues with OT Commissioner, check the `commissioner.log`,
if available. To configure logging, refer to [Build OT Commissioner](../../commissioner/build.md).

**IO_ERROR: connect socket to peer addr**

Try using a different IP address to start OT Commissioner.

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
A fatal SSL error typically indicates that the secure DTLS session fails.

If you receive this message, check your PSKc.

From OTBR:

```
$ sudo ot-ctl pskc
198886f519a8fd7c981fee95d72f4ba7
Done
```

From OT Commissioner:

```
> config get pskc
198886f519a8fd7c981fee95d72f4ba7
[done]
```

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
