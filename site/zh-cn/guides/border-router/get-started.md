# 开始使用 OTBR

Translator: https://github.com/dsyx

使用以下平台通过物理或模拟的 RCP 运行 OTBR。

## Docker

开始使用 OTBR 的最快方法是尝试其 Docker 版本。使用物理或模拟的 RCP 在任何 Linux-based 系统或 Raspberry Pi 3B（及以上）的 Docker 容器中运行 OTBR。

有关更多信息，请参阅 [Docker 支持概述](docker/index.md)。

## Codelabs

如果希望在没有 Docker 的情况下使用 OTBR，可以参考以下 Border Router 相关的 Codelab，使用物理 RCP 和 Raspberry Pi 3B 或 4 运行 OTBR。

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-border-router/">Border Router
  Codelab</a>

<a class="button button-primary"
   href="https://openthread.io/codelabs/openthread-border-router-ipv6-multicast">Border Router Thread
  1.2 Multicast Codelab</a>

## 平台

OTBR 也可以在支持的平台上直接运行：

1. 选择一个平台：
   * [BeagleBone Black](beaglebone-black.md)
   * [Raspberry Pi 3B or newer](raspberry-pi.md)
2. [构建并配置 OTBR](build.md)
3. 了解 [OTBR 中包含的工具和脚本](tools.md)

## 获取代码

要直接访问源代码，请参阅 [OpenThread Border Router GitHub 仓库](https://github.com/openthread/ot-br-posix)。

您可以通过向 [问题跟踪器（Issue Tracker）](https://github.com/openthread/ot-br-posix/issues) 提交错误报告和特性请求来为 OpenThread 边界路由器的持续开发做出贡献。

## 社区项目

> Note: 此处列出的项目未得到 OpenThread 团队的正式支持。QEMU 指南是为 NCP 支持写的。有关使用 RCP 的好处，请参阅 [OpenThread Border Router 概述](index.md)。

### QEMU OTBR

OT 社区的一名成员 [使用 QEMU（一个开源机器模拟及虚拟器）启用了 OTBR 支持](https://github.com/ERNE196077/qemu_openthread_borderrouter)。该项目在 ARM 架构上模拟 Raspbian。

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
