# Inform Previous Parent on Reattach

To allow end devices (EDs) in a Thread network to inform their previous parent
router that they have attached to a new parent router, enable the Inform
Previous Parent on Reattach feature.

This updates the previous parent's child table quicker than the configured
[child timeout interval](/reference/group/api-thread-general#otthreadgetchildtimeout) and
prevents it from queueing traffic for an ED that it thinks is asleep, but in
actuality has a new parent.

## How it works

After an ED attaches to a new parent router, it sends a single unicast IPv6
message containing the following information to its previous parent router:

*   The [Mesh-Local EID](/guides/thread-primer/ipv6-addressing#mesh-local-eid-ml-eid) of the ED
    as the source address.
*   The Mesh-Local EID of the previous parent router as the destination address.
*   An empty payload.

This type of IPv6 message prompts the old parent router to immediately remove
all registered IPv6 addresses for that ED from its child table.

## How to enable

This feature is disabled by default.

To enable Inform Previous Parent on Reattach, define
`OPENTHREAD_CONFIG_MLE_INFORM_PREVIOUS_PARENT_ON_REATTACH` as `1` in the
[`/src/core/config/mle.h`](https://github.com/openthread/openthread/tree/main/src/core/config/mle.h) file, prior to [building OpenThread](/guides/build):

```
#ifndef OPENTHREAD_CONFIG_MLE_INFORM_PREVIOUS_PARENT_ON_REATTACH
#define OPENTHREAD_CONFIG_MLE_INFORM_PREVIOUS_PARENT_ON_REATTACH 1
#endif
```

## Parameters

There are no configurable parameters for this feature.

## API

There is no public API for this feature.

## CLI

There are no CLI commands related to this feature.

### License

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
