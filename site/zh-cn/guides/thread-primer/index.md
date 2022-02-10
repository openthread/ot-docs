# 什么是 Thread？

<figure class="attempt-right">
<img src="../images/ot-logo-thread.png" srcset="../images/ot-logo-thread.png 1x, ../images/ot-logo-thread_2x.png 2x" border="0" alt="Thread" />
</figure>

<a href="http://threadgroup.org/">Thread<sup>®</sup></a> 是一个为低功耗物联网（IEEE 802.15.4-2006 WPAN）设备设计的基于 IPv6 的网络协议。Thread 是一个新的网状网络协议，它并不依赖其它的 802.15 网状网络协议（如 ZigBee、Z-Wave 和 Bluetooth LE）。

Thread 的主要特性包括：

* 易于部署和维护 — 安装、启动和操作相对简单
* 通信安全 — Thread 网络中的设备都必须通过身份验证，并且所有的通信都经过了加密
* 稳定可靠 — 具有自愈能力的网状网络，无单点故障，并且采用扩频技术以提高抗干扰能力
* 低功耗 — Thread 低功耗设备可以进入休眠并使用电池供电，通常使用一块电池便能工作数年
* 规模可扩展 — Thread 网络的规模可以扩展达数百个设备

如果你不熟悉 Thread，那么了解基本的 Thread 知识对于你在应用中使用 OpenThread 是至关重要的。本入门教程的目的是解释 Thread 的基本概念和工作原理，并为你提供了 OpenThread 开发的起点。

本教程假定读者已具备如下的基本知识：

* IEEE 802.15.4
* 网络及路由概念
* IPv6

本入门教程基于 Thread Specification V1.2。Thread Specification 可以在 [threadgroup.org](https://www.threadgroup.org/support#specifications) 中获取。

## License

Copyright (c) 2021-2022, The OpenThread Authors.
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