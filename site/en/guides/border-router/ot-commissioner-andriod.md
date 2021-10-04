# OT Commissioner Android App

Use the OT Commissioner CLI to {{intro}}.

## Download the OT Commissioner Android App

You can download and build the OT Commissioner Android App from OpenThread's [GitHub
repository](https://github.com/openthread/ot-commissioner/tree/master/android).

**Note:** The OT Commissioner App is only available for Android.

## Connect to the Border Router

1. With both devices on the same network, connect the device with the OT
Commissioner Android App to the Border Router.
1. Open the OT Commissioner Android App and select the desired Border
Router from the available list. The name is the same as the Thread network
created by the OTBR Web GUI.
1. Enter the Passphrase (Commissioner Credential) set in the OTBR Web GUI (and
used to generate the PSKc) when prompted for a password.

<figure class="attempt-right">
<a href="../images/comm-app-border-router.png"><img src="../images/comm-app-border-router.png" width="200" border="0" class="screenshot" alt="App Border Router" /></a>
</figure>

## Commission the Joiner

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

**Note:** The Joiner Credential is a device-specific string of all uppercase
alphanumeric characters (0-9 and A-Y, excluding I, O, Q, and Z for readability),
with a length between 6 and 32 characters.

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
