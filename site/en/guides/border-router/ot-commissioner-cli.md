#  OT Commissioner CLI

External commissioning is supported by the OT Commissioner CLI, available
on the [ot-commissioner GitHub repository](https://github.com/openthread/ot-commissioner). To
build and install OT Commissioner, refer to [OT Commissioner Build and
Configuration](https://openthread.io/guides/commissioner/build) for instructions.

**Note:** OT Commissioner must be running on the same host machine as OTBR.

## Connect to the Border Router

1.  Open the Non-CCM configuration file located at
    `/usr/local/etc/commissioner/non-ccm-config.json` and change the `PSKc` to
    `198886f519a8fd7c981fee95d72f4ba7`:
    
    `"PSKc" : "198886f519a8fd7c981fee95d72f4ba7"`   
    
1.  Start the OT Commissioner CLI with the Non-CCM configuration:

    ```
    $ commissioner-cli /usr/local/etc/commissioner/non-ccm-config.json
    >
    ```
    
1.  Connect to OTBR:

    ```
    > start :: 49191
    [done]
    >
    ```
    
1.  Verify that the Commissioner is active:

    ```
    >active
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
    
## Join the network

Next, on the Joiner device, [join the Thread network](joiner-start-thread.md) and test network connectivity.

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
