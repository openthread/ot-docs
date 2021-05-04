# Test Connectivity

Once you have started OTBR Docker, form a Thread network and test its
connectivity to the internet.

## Step 1: Form the Thread Network

<figure class="attempt-right">
<img src="/guides/images/otbr-gui-home-full.png" srcset="/guides/images/otbr-gui-home-full.png 1x, /guides/images/otbr-gui-home-full_2x.png 2x" border="0" alt="OTBR Web GUI Home" />
</figure>
On the machine running OTBR Docker:

1.  Open a browser window and navigate to 127.0.0.1:8080. If OTBR Docker is
    running correctly, the OTBR Web GUI loads.
    
1.  Select the "Form" menu option and change some of the default credentials. We
    recommend leaving the default Channel and On-Mesh Prefix values. Whatever
    you use, make a note of them so you can test a separate Thread node later.
<table>
  <thead>
    <th>Parameter</th><th>Sample Value</th>
  </thead>
  <tbody>
    <tr>
      <td>Network Name</td>
      <td>OTBR4444</td>
    </tr>
    <tr>
      <td>PAN ID</td>
      <td>0x4444</td>
    </tr>
    <tr>
      <td>Network Key</td>
      <td>33334444333344443333444433334444</td>
    </tr>
    <tr>
      <td>Extended PAN ID</td>
      <td>3333333344444444</td>
    </tr>
    <tr>
      <td>Passphrase</td>
      <td>444444</td>
    </tr>
    <tr>
      <td>Channel</td>
      <td>15</td>
    </tr>
    <tr>
      <td>On-Mesh Prefix</td>
      <td>fd11:22::</td>
    </tr>
  </tbody>
</table>

1.  Select **FORM** to form the Thread network. Check the output in the terminal
    window running OTBR Docker. You should see `otbr-agent` log output for the
    addition of the on-mesh prefix and a SLAAC address:

    ```
    otbr-agent[224]: [INFO]-CLI-----: execute command: prefix add fd11:22::/64 pasor
    ```

    This output is required for internet connectivty for the Thread network.

## Step 2: Bring up a second Thread node

With OTBR Docker up and running, add a standalone Thread node to the Thread
network and test that it has connectivity to the internet.

If using a physical RCP with OTBR Docker, use a second physical Thread node to
test. If using a simulated RCP with OTBR Docker, use a second simulated node to
test.

### Physical Thread node

Build and flash a standalone Thread node on the [supported platform](/platforms)
of your choice. This node does not have to be built with any specific build
switches.

See [Build OpenThread](/guides/build) for basic building instructions.

See the [Build a Thread network with nRF52840 boards and OpenThread
Codelab](https://codelabs.developers.google.com/codelabs/openthread-hardware/#0) for 
detailed instructions on building and flashing the Nordic nRF52840 platform.

1.  After building and flashing, attach the Thread device to the machine running
    OTBR Docker via USB. Use `screen` in a new terminal window to access the
    CLI. For example, if the device is mounted on port `/dev/ttyACM1`:
    ```
    $ screen /dev/ttyACM1 115200
    ```

1.  Press the **Enter** key to bring up the `>` OpenThread CLI prompt.

### Simulated Thread node

1.  Open a new terminal window on the machine running OTBR Docker.

1.  Start the CLI application to bring up a simulated node:
    ```
    $ cd ~/openthread
    $ ./output/simulation/bin/ot-cli-ftd 2
    ```

1.  Press the **Enter** key to bring up the `>` OpenThread CLI prompt.

## Step 3: Join the second node to the Thread network

Using the OpenThread CLI for your physical or simulated Thread node, join the
node to the Thread network created by OTBR Docker.

> Caution: Only the commissioner included with OTBR is supported with Docker. 
The Thread Commissioning App is not supported.

1.  Update the Thread network credentials for the node, using the minimum
    required values from OTBR Docker:
    ```
    > dataset masterkey 33334444333344443333444433334444
    Done
    > dataset commit active
    Done
    ```
    
1. Bring up the Thread interface and start Thread:
    ```
    > ifconfig up
    Done
    <code class="devsite-terminal" data-terminal-prefix="&gt; ">
    > thread start
    Done
    ```

1.  The node should join the OTBR Thread network automatically. Within two
    minutes its state should be `router`:
    ```
    > state
    router
    ```
    
1.  Check the node's IP addresses to ensure it has an IPv6 address with the
    on-mesh prefix of `fd11:22::/64` as specified during Thread network
    formation:
    ```
    > ipaddr
    fd11:22:0:0:614e:4588:57a1:a473
    fd33:3333:3344:0:0:ff:fe00:f801
    fd33:3333:3344:0:1b5f:db5:ecac:a9e
    fe80:0:0:0:e0c4:5304:5404:5f70:98cd
    ```
    
## Step 4: Ping a public address

You should be able to a ping a public IPv4 address from the standalone Thread
node at this point. Since Thread only uses IPv6, to ping a public IPv4 address
you must translate it to IPv6 and combine it with the well-known prefix of
`64:ff9b::/64` used by Network Address Translation (NAT) in OTBR.

For more information on how NAT is configured in OTBR, see [Configure
NAT](/guides/border-router/access-point#configure-nat).

1.  To get a translated IPv4 address, use a website like
    [findipv6.com](https://findipv6.com/ipv4-toipv6/).

1.  Translate the IPv4 address you wish to test. For example, `172.217.164.110`
    translated to IPv6 is `::ffff:acd9:a46e`.

1.  Using only the last 4 bytes of the resulting IPv6 address, combine it with
    the well-known prefix of `64:ff9b::/64` to get a new IPv6 address:
    ```64:ff9b::acd9:a46e```

1.  Ping this new IPv6 address from the CLI of the standalone Thread node to
    test it's internet connectivity. Pinging this address is akin to pinging the
    original IPv4 address:
    ```
    > ping 64:ff9b::acd9:a46e
    16 bytes from 64:ff9b:0:0:0:0:acd9:a46e: icmp_seq=1 hlim=118 time=45ms
    ```

Success! The second Thread node can now communicate with the internet, through
OTBR Docker.

> Note: This configuration only illustrates public internet connectivity using
IPv4 and NAT64. Direct public connectivity to IPv6 addresses requires
additional configuration of a public IPv6 address or prefix for the OTBR Docker
container, which is out of scope for this guide.