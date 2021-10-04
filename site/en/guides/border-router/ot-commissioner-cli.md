#  OT Commissioner CLI

{{intro paragraph}}

## Form the Thread network

### Web GUI

The recommended way to form a Thread network is via the [OTBR Web
GUI](https://openthread.io/guides/border-router/web-gui). When doing so, change
all the default values on the **Form** menu option, except for the On-Mesh
Prefix.

Make note of the **Passphrase** used. This passphrase is the Commissioner
Credential and is used (along with the Extended PAN ID and Network Name) to
generate the Pre-Shared Key for the Commissioner (PSKc). The PSKc is needed to
authenticate the Thread Commissioner (the external device) to the network.

**Note:** The Commissioner Credential is a user-defined string between 6
and 255 characters, UTF-8 encoded.

### Manual

The Thread network can also be formed manually on the command line of
OpenThread POSIX, using `ot-ctl`.

1.  Initialize a new operational dataset:

    ``` 
    $ sudo ot-ctl dataset init new
    Done
    ```
   
1.  Set the network credentials:

    ```  
    $ sudo ot-ctl dataset panid 0xdead
    Done
    ```
        
    ```
    $ sudo ot-ctl dataset extpanid dead1111dead2222
    Done
    ```
        
    ```
    $ sudo ot-ctl dataset networkname OpenThreadGuide
    Done
    ```
    
    ```       
    $ sudo ot-ctl dataset networkkey 11112233445566778899DEAD1111DEAD
    Done
    ```

1.  Generate a hex-encoded PSKc by using a Passphrase (Commissioner Credential),
    the Extended PAN ID, and the Network Name with the PSKc Generator tool on
    the OTBR. Make sure to use the same Extended PAN ID and Network Name that
    was used in the operational dataset:
    
    ```
    $ cd ~/ot-br-posix/build/otbr/tools
    $ ./pskc J01NME DEAD1111DEAD2222 OpenThreadGuide
    198886f519a8fd7c981fee95d72f4ba7
    ```
    
1.  Set the PSKc:

    ```
    $ sudo ot-ctl dataset pskc 198886f519a8fd7c981fee95d72f4ba7</code>
    Done
    ```
    
1.  Commit the active dataset, set the on-mesh prefix, and form the Thread
    network:

    ```
    $ sudo ot-ctl dataset commit active
    Done
    ```

    ```
    $ sudo ot-ctl prefix add fd11:22::/64 pasor
    Done
    ```
    
    ```
    $ sudo ot-ctl ifconfig up
    Done 
    ```

    ```
    $ sudo ot-ctl thread start
    Done
    ```
    
    ```
    $ sudo ot-ctl netdata register
    Done
    ```
    
1.  Confirm the network configuration:

    ```
    $ sudo ot-ctl state
    leader
    Done
    ```
    
    ```
    $ sudo ot-ctl pskc
    198886f519a8fd7c981fee95d72f4ba7
    Done
    ```

## Prepare the Joiner device

Build and flash a device with OpenThread, to function as the Joiner. For an
overview of building OpenThread, see the [Building Guide](../build/index.md).

Use the `-DOT_JOINER=ON` build option to enable the Joiner role.

For example, to build the CC2538 example platform for use as a Joiner:

```
$ ./script/build -DOT_JOINER=ON
```

Specific instructions on building supported platforms with GNU Autotools can be
found in each example's
[platform folder](https://github.com/openthread/openthread/tree/master/examples/platforms).

Once the Joiner device is ready, obtain its factory-assigned IEEE EUI-64. Use
the `eui64` command in the OpenThread CLI:

```
> eui64
0000b57fffe15d68
Done
```

## Build and install OT Commissioner</h2>

Build and install OT Commissioner. See [OT Commissioner Build and
Configuration](https://openthread.io/guides/commissioner/build) for instructions.

## Connect to the Border Router

**Note:** OT Commissioner must be running on the same host machine as OTBR.

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

<h2 class="numbered">Commission the Joiner</h2>

Once connected to the Border Router, OT Commissioner can commission the Joiner
device.

1.  In OT Commissioner, enable Thread 1.1 MeshCoP joiner for all Joiners with a
    password of `J01NU5`:
<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">joiner enableall meshcop J01NU5</code>
[done]
<code class="devsite-terminal" data-terminal-prefix="&gt; "></code></pre>
1.  On the Joiner device, start the Joiner role with the password configured in
    OT Commissioner:
<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">ifconfig up
Done</code>
<code class="devsite-terminal" data-terminal-prefix="&gt; ">joiner start J01NU5
Done</code></pre>
1.  Wait a minute for the DTLS handshake to complete between the Commissioner
    and Joiner:<br>
<div class="ot-inline"><pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">
Join success!</code></pre></div>

{% dynamic else %}

<h2 class="numbered">Download the app</h2>

External commissioning is supported by the
[{% dynamic print setvar.app_name %}]({% dynamic print setvar.app_link %}){:target="_blank"},
available for download at the {% dynamic print setvar.download %}.

Note: The {% dynamic print setvar.app_name %} is only available for Android.

<h2 class="numbered">Connect to the Border Router</h2>

<figure class="attempt-right">
<a href="../images/{% dynamic print setvar.img_br %}.png"><img src="../images/{% dynamic print setvar.img_br %}.png" width="200" border="0" class="screenshot" alt="App Border Router" /></a>
</figure>

1.  With both devices on the same network, connect the device with the
    {% dynamic print setvar.app_name %} to the Border Router.
1.  Open the {% dynamic print setvar.app_name %} and select the desired Border
    Router from the available list. The name is the same as the Thread network
    created by the OTBR Web GUI.
1.  Enter the Passphrase (Commissioner Credential) set in the OTBR Web GUI (and
    used to generate the PSKc) when prompted for a password.

<h2 class="numbered" style="clear:right">Commission the Joiner</h2>

<style type='text/css'>
.ot-inline { overflow: hidden !important; }
</style>

Once connected to the Border Router, the app provides the option to scan a
Connect QR Code or enter a Join Passphrase manually. The Join Passphrase is also
called the Joiner Credential, and is used (along with the Extended PAN ID and
Network Name) to generate the Pre-Shared Key for the Device (PSKd). The PSKd is
then used to authenticate a device during Thread Commissioning. The Joiner
Credential should be unique to each device.

Thread Connect QR Codes are created with the following text string format:

<pre>v=1&&eui=0000b57fffe15d68&&cc=J01NU5</pre>

Where `eui` is the Joiner device's EUI64 value and `cc` is the Joiner
Credential. Use this text string with an online QR Code generator to create a QR
Code for scanning.

{{ joiner_cred }}

<figure class="attempt-right">
<a href="../images/{% dynamic print setvar.img_comm %}.png"><img src="../images/{% dynamic print setvar.img_comm %}.png" width="200" border="0" class="screenshot" alt="App Commissioning" /></a>
</figure>

1.  In the {% dynamic print setvar.app_name %}, scan the Connect QR Code of the
    Joiner device, or enter the EUI64 and Joiner Credential manually. This
    generates the PSKd, propagates the steering data through the Thread network,
    and establishes a DTLS session.
1.  While the app is waiting, enter the OpenThread CLI on the Joiner device and
    start the Joiner role with that same Joiner Credential:
<div class="ot-inline"><pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">ifconfig up
Done</code>
<code class="devsite-terminal" data-terminal-prefix="&gt; ">joiner start J01NU5
Done</code></pre></div>
1.  Wait a minute for the DTLS handshake to complete between the Commissioner
    and Joiner:<br>
<div class="ot-inline"><pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">
Join success!</code></pre></div>
1.  The {% dynamic print setvar.app_name %} also updates with an
    "{% dynamic print setvar.confirm %}" confirmation message.

The Joiner has obtained the Thread network credentials, and can now join the
network.

{% dynamic endif %}

<h2 class="numbered">Join the network</h2>

On the Joiner device, start the Thread protocol to automatically join the
network.

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">thread start
Done</code>
</pre>

Check the state after a few moments to confirm. It may initially start as a
child, but within two minutes it should upgrade to a router.

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">state
router
Done</code>
</pre>

Also check the device's IPv6 addresses. It should have a Global address using
the On-Mesh Prefix specified during formation of the Thread network through the
OTBR Web GUI.

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">ipaddr
fdde:ad11:11de:0:0:ff:fe00:9400
<b>fd11:22:0:0:3a15:3211:2723:dbe1</b>
fe80:0:0:0:6006:41ca:c822:c337
fdde:ad11:11de:0:ed8c:1681:24c4:3562</code>
</pre>

<h2 class="numbered">Ping the external internet</h2>

Test the connectivity between the Joiner device in the Thread network and the
external internet by pinging a public IPv4 address.

For example, the Well-Known NAT64 prefix of `64:ff9b::/96` and an IPv4 address
of `8.8.8.8` combine to form an IPv6 address of `64:ff9b::808:808`.

Add an external route for the NAT64 Prefix:

<pre class="devsite-click-to-copy">
<code class="devsite-terminal">sudo ot-ctl route add 64:ff9b::/96 s med</code>
Done
<code class="devsite-terminal">sudo ot-ctl netdata register</code>
Done
</pre>

Ping the synthesized IPv6 address `64:ff9b::808:808` from the OpenThread CLI on the Joiner device:

<pre class="devsite-click-to-copy"><code class="devsite-terminal" data-terminal-prefix="&gt; ">ping 64:ff9b::808:808</code>
16 bytes from 64:ff9b:0:0:0:0:808:808: icmp_seq=3 hlim=45 time=72ms
</pre>

{% dynamic if request.query_string.comm == "android-app" %}

## Thread Commissioning App troubleshooting

You may encounter issues with the Thread Commissioning App, due to changed or
stale network information. The app retains OTBR network information locally and
does not always prompt for updates.

To resolve these issues, delete all local application data, restart the app, and
try the commissioning process again.

To delete local application data:

1.  On the Android device, open the Settings app
1.  Go to **Apps & notifications** > **App info** > **Thread** > **Storage**
1.  Select **CLEAR DATA**
1.  Go back to **App info** and select **FORCE STOP**
1.  Close the Settings app and restart the Thread app

{% dynamic endif %}

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
