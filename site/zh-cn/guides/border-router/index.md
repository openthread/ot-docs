# OpenThread Border Router

Contributor: https://github.com/dsyx

Thread 边界路由器（Border Router）用于将 Thread 网络连接到其他 IP-based 网络，例如 Wi-Fi 或 Ethernet（以太网）。Thread 网络至少需要一个边界路由器才能连接到其他网络。

<figure>
<img src="../images/otbr-arch-borderagent-white.png" border="0" alt="OTBR Border Agent Architecture" />
</figure>

Thread 边界路由器最低程度地支持以下功能：

* Thread 和 Wi-Fi/Ethernet 之间的双向 IP 连接。
* 通过 mDNS（在 Wi-Fi/Ethernet 链路上）和 SRP（在 Thread 网络上）的双向服务发现。
* 通过 IP-based 链路合并 Thread 分区的 Thread-over-infrastructure。
* 通过外部的 Thread Commissioning（例如手机）对 Thread 设备进行身份验证并将其加入到 Thread 网络。

<figure class="attempt-right">
  <a href="https://www.threadgroup.org/What-is-Thread#certifiedproducts">
    <img src="../images/ot-thread-certified.png"
         srcset="/images/ot-thread-certified.png 1x, /images/ot-thread-certified_2x.png 2x"
         border="0" alt="Thread Certified" /></a></figure>

OpenThread 的边界路由器实现称为 OTBR（OpenThread Border Router），支持 [无线协处理器（RCP，Radio Co-Processor）](https://openthread.io/platforms/co-processor) 设计。选择平台时，请考虑使用 RCP 的以下好处：

* 更多资源：OpenThread 可以利用主机处理器的资源，这通常比 802.15.4 SoC 提供的资源多得多。
* 更高的性价比：最大限度地降低对 802.15.4 SoC 的资源需求，从而获得更具成本效益的解决方案。
* 更容易调试：由于大部分处理发生在主机处理器上，您可以在主机处理器上使用功能更强大的调试工具。
* 更稳定的 802.15.4 SoC 固件：RCP 只实现 sub-MAC 和 PHY，降低了 802.15.4 SoC 的固件更新频率。
* 更容易与主机的 IPv6 网络栈集成：在主机上运行 OpenThread 可以更直接地与主机的 IPv6 栈集成。

> Note: 使用 [Raspberry Pi 3B](raspberry-pi.md) 和 [Nordic nRF52840](https://openthread.io/vendors/nordic-semiconductor) NCP 构建的 OTBR 也是一个 Thread 认证组件（Thread Certified Component）。

## 特性和服务

OTBR 包括许多特性，包括：

* 用于配置和管理的 [Web GUI](web-gui.md)
* 支持 [外部 commissioning](external-commissioning/index.md) 的 Thread 边界代理（Thread Border Agent）
* Thread 网络用于获取 IPv6 前缀的 DHCPv6 前缀代理（DHCPv6 Prefix Delegation）
* 用于连接到 IPv4 网络的 NAT64
* 允许 Thread 设备根据名称发起与 IPv4-only 服务器通信的 DNS64
* 使用 OpenThread 内置特性的 Thread 接口驱动程序
* [Docker 支持](docker/index.md)

### 边界路由器服务

OTBR 提供以下服务：

* [mDNS 发布器（mDNS Publisher）](mdns-discovery.md) —— 允许外部 Commissioner 发现 OTBR 及其关联的 Thread 网络
* [PSKc 生成器（PSKc Generator）](tools.md) —— 用于生成 PSKc 密钥
* Web 服务 —— 用于管理 Thread 网络的 [Web UI](web-gui.md)

边界路由器服务（Border Router Services）使用到的第三方组件包括 Simple Web Server 和用于 Web UI 框架的 Material Design Lite。

### OTBR 防火墙

OTBR 使用 `iptables` 和 `ipset` 来实现以下入口过滤规则：

* 阻止使用 On-Link 地址源发起的入站分组，例如 OMR（Off-Mesh Routable）和基于 Mesh-Local 前缀的地址。
* 阻止目的地址不是 OMR 地址或 DUA（Domain Unicast Address）的入站单播分组。
* 阻止源地址或目的地址为 Link-Local 的入站单播分组。请注意，此规则由内核处理，并未明确设置。

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
