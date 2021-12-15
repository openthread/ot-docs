# Run OTBR Docker

OpenThread Border Router (OTBR) requires a Thread RCP node in order to join a
Thread network. OTBR Docker provides support for both a physical RCP (OpenThread
dongle) or a simulated RCP.

If you want to connect OTBR Docker to other physical Thread devices, use a
physical RCP. If you want to test border routing with a simulated Thread
network, use a simulated RCP.

## Physical RCP

Use any [supported OpenThread platform](https://openthread.io/platforms) for the 
physical RCP. See the [Build and flash RCP](../../../guides/border-router/build.md#build-and-flash-rcp)
step from the OpenThread Border Router Build and Configuration guide for more
information.

### Attach the RCP

1.  After building and flashing, attach the RCP device to the machine running
    OTBR Docker via USB.
1.  Determine the serial port name for the RCP device by checking `/dev`:
    ```
    $ ls /dev/tty*
    /dev/ttyACMO
    ```

### Start the OTBR Docker container

> Note: Linux users, if you haven't done so already, make sure to run
`sudo modprobe ip6table_filter` for OTBR firewall support.
This allows OTBR scripts to create rules inside the Docker container
before `otbr-agent` starts.

In a new terminal window, start OTBR Docker, referencing the RCP's serial port.
For example, if the RCP is mounted at `/dev/ttyACM0`:

```
$ docker run --sysctl "net.ipv6.conf.all.disable_ipv6=0 net.ipv4.conf.all.forwarding=1 net.ipv6.conf.all.forwarding=1" -p 8080:80 --dns=127.0.0.1 -it --volume /dev/ttyACM0:/dev/ttyACM0 --privileged openthread/otbr --radio-url spinel+hdlc+uart:///dev/ttyACM0
```

Upon success, you should have output similar to this:

```
WARNING: Localhost DNS setting (--dns=127.0.0.1) may fail in containers.
RADIO_URL: spinel+hdlc+uart:///dev/ttyACM0
TUN_INTERFACE_NAME: wpan0
NAT64_PREFIX: 64:ff9b::/96
AUTO_PREFIX_ROUTE: true
AUTO_PREFIX_SLAAC: true
Current platform is ubuntu
* Applying /etc/sysctl.d/10-console-messages.conf ...
kernel.printk = 4 4 1 7
* Applying /etc/sysctl.d/10-ipv6-privacy.conf ...
net.ipv6.conf.all.use_tempaddr = 2
net.ipv6.conf.default.use_tempaddr = 2
* Applying /etc/sysctl.d/10-kernel-hardening.conf ...
kernel.kptr_restrict = 1
* Applying /etc/sysctl.d/10-link-restrictions.conf ...
fs.protected_hardlinks = 1
fs.protected_symlinks = 1
* Applying /etc/sysctl.d/10-magic-sysrq.conf ...
kernel.sysrq = 176
* Applying /etc/sysctl.d/10-network-security.conf ...
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.tcp_syncookies = 1
* Applying /etc/sysctl.d/10-ptrace.conf ...
kernel.yama.ptrace_scope = 1
* Applying /etc/sysctl.d/10-zeropage.conf ...
vm.mmap_min_addr = 65536
* Applying /etc/sysctl.d/60-otbr-ip-forward.conf ...
net.ipv6.conf.all.forwarding = 1
net.ipv4.ip_forward = 1
* Applying /etc/sysctl.conf ...
 * Starting userspace NAT64 tayga             [ OK ]
/usr/sbin/service
 * Starting domain name service... bind9      [ OK ]
/usr/sbin/service
 * dbus is not running
 * Starting system message bus dbus           [ OK ]
   ...fail!
otWeb[155]: border router web started on wpan0
otbr-agent[224]: Thread interface wpan0
otbr-agent[224]: Thread is down
otbr-agent[224]: Check if Thread is up: OK
otbr-agent[224]: Stop publishing service
otbr-agent[224]: PSKc is not initialized
otbr-agent[224]: Check if PSKc is initialized: OK
otbr-agent[224]: Initialize OpenThread Border Router Agent: OK
otbr-agent[224]: Border router agent started.
```

OTBR Docker is now running. Leave this terminal window open and running in the
background. If you quit the process or close the window, OTBR Docker will go
down.

Go to [Test Connectivity](../../../guides/border-router/docker/test-connectivity.md) to
continue with the OTBR Docker setup.

## Simulated RCP

Use a simulated OpenThread RCP build for the simulated RCP. This is useful if
you want to test border routing with a simulated Thread network on a single
machine.

### Build the simulated RCP application

1.  Clone the OpenThread repository:
    ```
    $ cd ~
    $ git clone https://github.com/openthread/openthread
    ```

1.  Bootstrap and build the simulated application:
    ```
    $ cd openthread
    $ ./bootstrap
    $ make -f examples/Makefile-simulation
    ```

### Set up a bidirectional data stream

Use the `socat` command line utility to establish a bidirectional data stream
to transfer data between the simulated RCP and OTBR Docker.

1.  Open a new terminal window to run this process, as it must be left running
    while OTBR Docker is running.

1.  Install `socat`:
    ```
    $ sudo apt-get install socat
    ```
    
1.  Start `socat`:
    ```
    $ socat -d -d pty,raw,echo=0 pty,raw,echo=0
    2018/09/06 09:58:29 socat[242994] N PTY is /dev/pts/2
    2018/09/06 09:58:29 socat[242994] N PTY is /dev/pts/7
    2018/09/06 09:58:29 socat[242994] N starting data transfer loop with FDs [5,5] and [7,7]
    ```

Make a note of the two serial ports bolded in the output. Use the first one for
the simulated RCP and the second one for OTBR Docker. In the example output
above:

*   `/dev/pts/2` = Simulated RCP port
*   `/dev/pts/7` = OTBR Docker

> Note: The serial ports returned by `socat` might differ on your system. Always use the values 
in your local `socat` output.

Leave this terminal window open and running in the background.

### Start the simulated RCP

1.  Open a new terminal window to run the simulated RCP, as it must be left
    running while OTBR Docker is running.

1.  Using the first serial port in the `socat` output, start the simulated RCP
    application. For example, if using `/dev/pts/2` from the `socat` output:
    ```
    $ ~/openthread/output/simulation/bin/ot-rcp 1 > /dev/pts/2 < /dev/pts/2
    ```

There is no output from this command. Leave this terminal window open and
running in the background.

### Start the OTBR Docker container

In a new terminal window, start OTBR Docker, using the second serial port in the
`socat` output. For example, if using `/dev/pts/7` from the `socat` output:

```
$ docker run --sysctl "net.ipv6.conf.all.disable_ipv6=0 net.ipv4.conf.all.forwarding=1 net.ipv6.conf.all.forwarding=1" -p 8080:80 --dns=127.0.0.1 -it --volume /dev/pts/7:/dev/ttyUSB0 --privileged openthread/otbr
```

Note that the command is also using the `/dev/ttyUSB0` port. This is the default
mount point within the Docker container.

Upon success, you should have output similar to this:

```
WARNING: Localhost DNS setting (--dns=127.0.0.1) may fail in containers.
RADIO_URL: spinel+hdlc+uart:///dev/ttyUSB0
TUN_INTERFACE_NAME: wpan0
NAT64_PREFIX: 64:ff9b::/96
AUTO_PREFIX_ROUTE: true
AUTO_PREFIX_SLAAC: true
Current platform is ubuntu
* Applying /etc/sysctl.d/10-console-messages.conf ...
kernel.printk = 4 4 1 7
* Applying /etc/sysctl.d/10-ipv6-privacy.conf ...
net.ipv6.conf.all.use_tempaddr = 2
net.ipv6.conf.default.use_tempaddr = 2
* Applying /etc/sysctl.d/10-kernel-hardening.conf ...
kernel.kptr_restrict = 1
* Applying /etc/sysctl.d/10-link-restrictions.conf ...
fs.protected_hardlinks = 1
fs.protected_symlinks = 1
* Applying /etc/sysctl.d/10-magic-sysrq.conf ...
kernel.sysrq = 176
* Applying /etc/sysctl.d/10-network-security.conf ...
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.tcp_syncookies = 1
* Applying /etc/sysctl.d/10-ptrace.conf ...
kernel.yama.ptrace_scope = 1
* Applying /etc/sysctl.d/10-zeropage.conf ...
vm.mmap_min_addr = 65536
* Applying /etc/sysctl.d/60-otbr-ip-forward.conf ...
net.ipv6.conf.all.forwarding = 1
net.ipv4.ip_forward = 1
* Applying /etc/sysctl.conf ...
 * Starting userspace NAT64 tayga             [ OK ]
/usr/sbin/service
 * Starting domain name service... bind9      [ OK ]
/usr/sbin/service
 * dbus is not running
 * Starting system message bus dbus           [ OK ]
   ...fail!
otWeb[155]: border router web started on wpan0
otbr-agent[224]: Thread interface wpan0
otbr-agent[224]: Thread is down
otbr-agent[224]: Check if Thread is up: OK
otbr-agent[224]: Stop publishing service
otbr-agent[224]: PSKc is not initialized
otbr-agent[224]: Check if PSKc is initialized: OK
otbr-agent[224]: Initialize OpenThread Border Router Agent: OK
otbr-agent[224]: Border router agent started.
```

OTBR Docker is now running. Leave this terminal window open and running in the
background. If you quit the process or close the window, OTBR Docker will go
down.

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
