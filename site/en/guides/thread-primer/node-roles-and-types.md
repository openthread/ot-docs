# Node Roles and Types

## Forwarding roles

<figure class="attempt-right">
<a href="../images/ot-primer-roles_2x.png"><img src="../images/ot-primer-roles.png" srcset="../images/ot-primer-roles.png 1x, ../images/ot-primer-roles_2x.png 2x" border="0" alt="OT Node Roles" /></a>
</figure>

In a Thread network, nodes are split into two forwarding roles:

### Router

A Router is a node that:

*   forwards packets for network devices
*   provides secure commissioning services for devices trying to join the network
*   keeps its transceiver enabled at all times

### End Device

An End Device (ED) is a node that:

*   communicates primarily with a single Router
*   does not forward packets for other network devices
*   can disable its transceiver to reduce power

Key Point: The relationship between Router and End Device is a Parent-Child
relationship. An End Device attaches to exactly one Router. The Router is always
the Parent, the End Device the Child.

## Device types

Furthermore, nodes comprise a number of types.

<figure class="attempt-right">
<a href="../images/ot-primer-taxonomy.png"><img src="../images/ot-primer-taxonomy.png" border="0" alt="OT Device Taxonomy" /></a>
</figure>

### Full Thread Device

A Full Thread Device (FTD) always has its radio on, subscribes to the
all-routers multicast address, and maintains IPv6 address mappings. There are
three types of FTDs:

*   Router
*   Router Eligible End Device (REED) — can be promoted to a Router
*   Full End Device (FED) — cannot be promoted to a Router

An FTD can operate as a Router (Parent) or an End Device (Child).

### Minimal Thread Device

A Minimal Thread Device does not subscribe to the all-routers
multicast address and forwards all messages to its Parent. There are
two types of MTDs:

*   Minimal End Device (MED) — transceiver always on, does not need to poll for
    messages from its parent
*   Sleepy End Device (SED) — normally disabled, wakes on occasion to poll for
    messages from its parent

An MTD can only operate as an End Device (Child).

### Upgrading and downgrading

When a REED is the only node in reach of a new End Device wishing to join the
Thread network, it can upgrade itself and operate as a Router:

<figure>
<a href="../images/ot-primer-router-upgrade_2x.png"><img src="../images/ot-primer-router-upgrade.png" srcset="../images/ot-primer-router-upgrade.png 1x, ../images/ot-primer-router-upgrade_2x.png 2x" border="0" width="400" alt="OT End Device to Router" /></a>
</figure>

Conversely, when a Router has no children, it can downgrade itself and operate
as an End Device:

<figure>
<a href="../images/ot-primer-router-downgrade_2x.png"><img src="../images/ot-primer-router-downgrade.png" srcset="../images/ot-primer-router-downgrade.png 1x, ../images/ot-primer-router-downgrade_2x.png 2x" border="0" width="400" alt="OT Router to End Device" /></a>
</figure>

## Other roles and types

### Thread Leader

<figure class="attempt-right">
<a href="../images/ot-primer-leader_2x.png"><img src="../images/ot-primer-leader.png" srcset="../images/ot-primer-leader.png 1x, ../images/ot-primer-leader_2x.png 2x" border="0" alt="OT Leader and Border Router" /></a>
</figure>

The Thread Leader is a Router that is responsible for managing the set of
Routers in a Thread network. It is dynamically self-elected for fault tolerance,
and aggregates and distributes network-wide configuration information.

Note: There is always a single Leader in each Thread network
[partition](#partitions).

### Border Router

A Border Router is a device that can forward information between a Thread
network and a non-Thread network (for example, Wi-Fi). It also configures a
Thread network for external connectivity.

Any device may serve as a Border Router.

Note: There can be multiple Border Routers in a Thread network.

## Partitions

<figure class="attempt-right">
<a href="../images/ot-primer-partitions_2x.png"><img src="../images/ot-primer-partitions.png" srcset="../images/ot-primer-partitions.png 1x, ../images/ot-primer-partitions_2x.png 2x" border="0" alt="OT Partitions" /></a>
</figure>

A Thread network might be composed of partitions. This occurs when a group of
Thread devices can no longer communicate with another group of Thread devices.
Each partition logically operates as a distinct Thread network with its own
Leader, Router ID assignments, and network data, while retaining the same
security credentials for all devices across all partitions.

Partitions in a Thread network do not have wireless connectivity between each
other, and if partitions regain connectivity, they automatically merge into a
single partition.

Key Point: Security credentials define the Thread network. Physical radio
connectivity defines partitions within that Thread network.

Note that the use of "Thread network" in this primer assumes a single partition.
Where necessary, key concepts and examples are clarified with the term "partition."
Partitions are covered in-depth later in this primer.

## Device limits

There are limits to the number of device types a single Thread network supports.

Role | Limit
----|----
Leader | 1
Router | 32
End Device | 511 per Router

Thread tries to keep the number of Routers between 16 and 23. If a REED attaches
as an End Device and the number of Routers in the network is below 16, it
automatically promotes itself to a Router.

## Recap

What you learned:

*   A Thread device is either a Router (Parent) or an End Device (Child)
*   A Thread device is either a Full Thread Device (maintains IPv6 address
    mappings) or a Minimal Thread Device (forwards all messages to its Parent)
*   A Router Eligible End Device can promote itself to a Router, and vice versa
*   Every Thread network partition has a Leader to manage Routers
*   A Border Router is used to connect Thread and non-Thread networks
*   A Thread network might be composed of multiple partitions

## Check Your Understanding





<div>
  <devsite-multiple-choice>
    <div>A Thread device fulfills one of which two roles?</div>
    <div>
      <div>Child Node</div>
      <div>Incorrect.</div>
    </div>
    <div correct>
      <div>Router</div>
      <div>Correct.</div>
    </div>
    <div correct>
      <div>End Device</div>
      <div>Correct.</div>
    </div>
    <div>
      <div>Gateway</div>
      <div>Incorrect.</div>
    </div>
  </devsite-multiple-choice>
</div>






<div>
  <devsite-multiple-choice>
    <div>What are the two classifications of Thread device?</div>
    <div correct>
      <div>Minimal Thread Device (MTD)</div>
      <div>Correct.</div>
    </div>
    <div correct>
      <div>Full Thread Device (FTD)</div>
      <div>Correct.</div>
    </div>
    <div>
      <div>Semi-End Device (SED)</div>
      <div>Incorrect.</div>
    </div>
    <div>
      <div>Miniscule Thread Device (MTD)</div>
      <div>Incorrect.</div>
    </div>
    <div>
      <div>Sleepy End Device (SED)</div>
      <div>Incorrect.</div>
    </div>
  </devsite-multiple-choice>
</div>





<div>
  <devsite-multiple-choice>
    <div>What are the three subtypes of FTD?</div>
    <div correct>
      <div>Router</div>
      <div>Correct.</div>
    </div>
    <div>
      <div>Thread Gateway Device (TGD)</div>
      <div>Incorrect.</div>
    </div>
    <div correct>
      <div>Router Eligible End Device (REED)</div>
      <div>Correct.</div>
    </div>
    <div correct>
      <div>Full End Device (FED)</div>
      <div>Correct.</div>
    </div>
    <div>
      <div>Half Thread Device (HTD)</div>
      <div>Incorrect.</div>
    </div>
  </devsite-multiple-choice>
</div>

<div>
  <devsite-multiple-choice>
    <div>What are the two subtypes of Minimal Thread Device (MTD)?</div>
    <div>
      <div>Semi-End Device (SED)
      </div>
      <div>Incorrect.</div>
    </div>
    <div>
      <div>Half Thread Device (HTD)</div>
      <div>Incorrect.</div>
    </div>
    <div correct>
      <div>Minimal End Device (MED)</div>
      <div>Correct.</div>
    </div>
    <div>
      <div>Miniscule Thread Device (MTD)</div>
      <div>Incorrect.</div>
    </div>
    <div correct>
      <div>Sleepy End Device (SED)</div>
      <div>Correct.</div>
    </div>
</devsite-multiple-choice>
</div>



<div>
  <devsite-multiple-choice>
    <div>A Router is a node that (select all that apply)</div>
    <div>
      <div>can disable its transceiver to reduce power</div>
      <div>Devices that are functioning as Routers do not disable their transceiver while fulfilling the Router role.</div>
    </div>
    <div>
      <div>communicates primarily with a single Router </div>
      <div>That would be an End Device (ED).</div>
    </div>
    <div>
      <div>does not forward packets for other network devices </div>
      <div>By definition, Routers forward packets for other network devices.</div>
    </div>
    <div correct>
      <div>forwards packets for network devices</div>
      <div>Correct.</div>
    </div>
    <div correct>
      <div>keeps its transceiver enabled at all times</div>
      <div>Correct. In order to function properly as a Router, a device must keep its transceiver online at all times.</div>
    </div>
    <div correct>
      <div>provides secure commissioning services for devices trying to join the network</div>
      <div>Correct. This is an important function of a Thread Router.</div>
    </div>
  </devsite-multiple-choice>
</div>


<div>
  <devsite-multiple-choice>
    <div>
      <div>An End Device (ED) is a node that  (select all that apply)</div>
    </div>
    <div correct>
      <div>can disable its transceiver to reduce its power consumption</div>
      <div>An End Device can do this, but a Router cannot.</div>
    </div>
    <div correct>
      <div>communicates primarily with a single Router</div>
      <div>End Devices communicate primarily with one Router.</div>
    </div>
    <div correct>
      <div>does not forward packets for other network devices</div>
      <div>This is a function of a Router, not an End Device.</div>
    </div>
    <div>
      <div>forwards packets for network devices</div>
      <div>This is a function of a Router, not an End Device.</div>
    </div>
    <div>
      <div>keeps its transceiver enabled at all times</div>
      <div>Incorrect. End Devices do not need to keep their transceivers enabled all the time, and doing so would waste power.</div>
    </div>
    <div>
      <div>provides secure commissioning services for devices trying to join the network</div>
      <div>Incorrect. This is a function of a Router, not an End Device.</div>
    <div>
  </devsite-multiple-choice>
</div>


<div>
  <devsite-multiple-choice>
    <div>A REED is a</div>
    <div correct>
      <div>Router Eligible End Device</div>
      <div>Correct.</div>
    </div>
    <div>
      <div>Regional Elliptic End Device</div>
      <div>There is no such thing.</div>
    </div>
    <div>
      <div>Routing Electronic Exfiltration Device</div>
      <div>There is no such thing.</div>
    </div>
    <div>
      <div>Router Eligible Edge Device</div>
      <div>There is no such thing.</div>
    </div>
  </devsite-multiple-choice>
</div>



<div>
  <devsite-multiple-choice>
    <div>When can a device upgrade itself?</div>
    <div correct>
      <div>when it is a REED and it is the only node in reach of a new End Device seeking to join the Thread network</div>
      <div>That's right. Under these circumstances, a REED can promote itself to a Router.</div>
    </div>
    <div>
      <div>when it is an End Device seeking to join the Thread network</div>
      <div>Incorrect.</div>
    </div>
    <div>
      <div>when it is a REED and the Thread network has merged with a larger network</div>
      <div>Incorrect.</div>
    </div>
  </devsite-multiple-choice>
</div>

<div>
  <devsite-multiple-choice>
    <div>When can a device downgrade itself?</div>
    <div correct>
      <div>when it is a Router and has no children</div>
      <div>That's correct. A Router with no children is allowed to revert to an End Device on its own.</div>
    </div>
    <div>
      <div>when it is a Router and a new End Device is seeking to join the Thread network</div>
      <div>A Router cannot revert to an End Device in this scenario.</div>
    </div>
    <div>
      <div>when it is an End Device and is seeking to leave the Thread network</div>
      <div>A Router cannot revert to an End Device in this scenario.</div>
    </div>
  </devsite-multiple-choice>
</div>

<div>
  <devsite-multiple-choice>
    <div>What is a accurate description of a Thread network partition?</div>
    <div>
      <div>A group of nodes within a Thread network that is walled off from the rest of the network</div>
      <div>That is not right.</div>
    </div>
    <div correct>
      <div>A distinct subsection of a network, with its own Leader, Router ID assignments, and network data, using the same security credentials as the rest of the larger network.</div>
      <div>That's right. Partitions are formed based on ability to intercommunicate. If one or more groups of nodes within a Thread network do not have radio connectivity with one another, you can be sure that there is more than one partition in that network.</div>
    </div>
    <div>
      <div>A group of nodes within a Thread network whose members all share the same Group Name</div>
      <div>Incorrect. Partitioning is not based on naming.</div>
    </div>
  </devsite-multiple-choice>
</div>

<div>
  <devsite-multiple-choice>
    <div>Every Thread network partition has &lowbar;&lowbar;&lowbar;&lowbar;&lowbar;&lowbar;&lowbar;&lowbar;&lowbar; to manage Routers</div>
    <div>
      <div>a Gateway</div>
      <div>Incorrect. Thread networks do not have Gateways.</div>
    </div>
    <div>
      <div>a Parent</div>
      <div>Incorrect. A Parent is a term that is applied to Thread Routers, and a Thread network can have more than one Router.</div>
    </div>
    <div correct>
      <div>a Leader</div>
      <div>This is the correct term. A Thread Leader manages Routers on a network, and gathers and disseminates network-wide configuration information.</div>
    </div>
  </devsite-multiple-choice>
</div>

<div>
  <devsite-multiple-choice>
    <div>What device is used to connect Thread and non-Thread networks?</div>
    <div>
      <div>a Gateway</div>
      <div>Although the term 'Gateway', in traditional networking, refers to a device that connects two networks, this is the wrong term for a Thread device that serves this role.</div>
    </div>
    <div correct>
      <div>a Border Router</div>
      <div>Correct. A Border Router is used to connect Thread and non-Thread networks.</div>
    </div>
    <div>
      <div>a Firewall</div>
      <div>That is not right.</div>
    </div>
    <div>
      <div>a Bridge</div>
      <div>This term refers to a similar concept in traditional networking, but it's not correct in the context of Thread networks. A bridge connects two LANs that use the same network protocol.</div>
    </div>
  </devsite-multiple-choice>
</div>

<div>
  <devsite-multiple-choice>
    <div>A network may have &lowbar;&lowbar;&lowbar;&lowbar;&lowbar; Leader</div>
    <div>
      <div>either none or exactly one</div>
      <div>This is wrong.</div>
    </div>
    <div correct>
      <div>one and only one</div>
      <div>Correct.</div>
    </div>
    <div>
      <div>more than one</div>
      <div>Wrong; a Thread network cannot have multiple Leaders.</div>
    </div>
  </devsite-multiple-choice>
</div>

<div>
  <devsite-multiple-choice>
    <div>The optimal number of routers on a Thread network is &lowbar;&lowbar;&lowbar;&lowbar;</div>
    <div>
      <div>exactly one</div>
      <div>Incorrect.</div>
    </div>
    <div>
      <div>between 8 and 16</div>
      <div>Incorrect.</div>
    </div>
    <div correct>
      <div>between 16 and 23</div>
      <div>That's right!</div>
    </div>
    <div>
      <div>between 24 and 32</div>
      <div>Incorrect.</div>
    </div>
    <div>
      <div>between 96 and 128</div>
      <div>Incorrect.</div>
    </div>
  </devsite-multiple-choice>
</div>






## License

Copyright (c) 2022, The OpenThread Authors.
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
