# Update OT CLI Commands

CLI commands provide command-line access to common API methods and functions.
Because most commands share the same descriptions, parameters, and return
values, they can be documented in the same API header files. This offers the
following benefits:

*   One documentation set for similar information
*   Keeps documentation current with API updates
*   Provides additional context for API methods and functions
*   Quickly find CLI commands from the Reference menu

## Get started

To document a CLI command, complete the following steps. It's important that
you follow these steps in the order provided.

1.  Find the associated API from the `openthread/include/openthread`
    directory. For example, `ba state` maps to `otBorderAgentGetState`:

    [otBorderAgentGetState](https://github.com/openthread/openthread/blob/main/include/openthread/border_agent.h#L73)

1.  Generalize the `@brief` description to work for both the API and the
    CLI command. Brief and detailed descriptions display on API and CLI pages.

    ```none
    /**
     * Gets the #otBorderAgentState of the Thread Border Agent role.
     * 
    ```

1.  Use the Doxygen ALIAS `@cli` to indicate the CLI command name.

    ```none
     * @cli ba state
    ```

1.  Next, immediately after `@cli`, add examples using `@code`. Include at
    least one example. Do not include the `>` prompt, and make sure to
    document the full return output, including the standard `Done` response.

    ```none
     * @code
     * ba state
     * Started
     * Done
     * @endcode
     * 
    ```

These are the minimum steps required to document an OT CLI command, resulting in
the following Doxygen comment:

```none
/**
 * Gets the #otBorderAgentState of the Thread Border Agent role.
 *
 * @cli ba state
 * @code
 * ba state
 * Started
 * Done
 * @endcode
 *
 * @param[in]  aInstance  A pointer to an OpenThread instance.
 *
 * @returns The current #otBorderAgentState of the Border Agent.
 *
 */
otBorderAgentState otBorderAgentGetState(otInstance *aInstance);
```

To review the HTML output, refer to
[ba state](https://openthread.io/reference/cli/commands#ba_state).
For advanced examples and additional options, refer to the following sections.

## Map API methods and functions

CLI commands must only be documented in the `openthread/include/openthread`
directory.

Use the `openthread/src/cli` source files to locate the corresponding API
methods or functions.

*   If the CLI command doesn't have a direct match, use your best judgment.
    You can link to other API methods or functions with `#otFunctionName` or
    `@sa`.

*   If a CLI command doesn't have a corresponding method or function,
    document it at the beginning of a relevant header file. For example,
    `netdata help` in `netdata.h`:

    ```none
    /**
     * @addtogroup api-thread-general
     *
     * @{
     *
     * @cli netdata help
     * @code
     * netdata help
     * help
     * publish
     * register
     * show
     * steeringdata
     * unpublish
     * Done
     * @endcode
     * @par
     * Gets a list of `netdata` CLI commands.
     * @sa @netdata
     * 
     */
    ```

### CLI template

CLI commands are grouped by one continuous comment block, beginning with the
ALIAS tag `@cli`. At the minimum, use `@cli` and `@code` to document a
command. Multiple `@code` examples are supported.

```none
 *
 * @cli command name (overload1,overload2)
 * @code
 * command name arg1
 * Done
 * @endcode
 * @cparam command name @ca{arg1} [@ca{opt-arg}] [@ca{choose-1}|@ca{choose-2}]
 * Optional parameter description; displays below the Parameter heading.
 * *   Markdown and lists are supported.
 * @par
 * Optional CLI specific paragraph. Markdown and lists are supported.
 * @csa{other command name}
 * 
```

## Command names

Any text after `@cli` becomes the command name header. This header is used in
dynamic linking, for example the
[netdata publish prefix](https://openthread.io/reference/cli/commands#netdata_publish_prefix)
command:

```none
 * @cli netdata publish prefix
```

> Important: Do not use arguments or special characters in command names.

Other commands might be more complex. For example,
`netdata publish dnssrp unicast` provides two options:

1.  Publish by address and port number
1.  Publish by port number and a device's Mesh-Local EID

If a command is overloaded, use parentheses to uniquely identify the command.

```none
 * @cli netdata publish dnssrp unicast (mle)
 * @cli netdata publish dnssrp unicast (addr,port)
```

If two similar commands share the same API method or function, use parentheses
to indicate each command option. Do not use spaces inside the
parentheses. For example, `joiner discerner set` and
`joiner discerner clear` both map to the API method `otJoinerSetDiscerner`:

```none
 * @cli joiner discerner (set,clear)
```

## Parameters

Define command parameters using `@cparam` and `@ca`.

*   Put brackets `[ ]` around optional arguments.
*   Use vertical bars `|` (pipes) between argument choices.
*   To display parameter details, you can enter sentences and markdown lists
    below the `@cparam` tag.

### Parameters with details

```none
 * @cparam netdata publish prefix @ca{prefix} [@ca{padcrosnD}] [@ca{high}|@ca{med}|@ca{low}]
 * OT CLI uses mapped arguments to configure #otBorderRouterConfig values. @moreinfo{the @overview}.
```

To review the HTML output, refer to [netdata publish prefix](https://openthread.io/reference/cli/commands#netdata_publish_prefix).

### Parameters with markdown list

```none
 * @cparam netdata show [@ca{local}] [@ca{-x}]
 * *   The optional `-x` argument gets Network Data as hex-encoded TLVs.
 * *   The optional `local` argument gets local Network Data to sync with Leader.
```

To review the HTML output, refer to [netdata show](https://openthread.io/reference/cli/commands#netdata_show).

## CLI specific descriptions

Use `@par` to add more information to the `@brief` and `@details` section.
These paragraphs are CLI specific, and won't display on the associated API
Reference page.

Only provide a title when necessary. To create the paragraph without a title,
make sure to drop down to the next line:

```none
 * @par
 * CLI description here; add a few things that do not apply to the API method.
 * @par
 * Start a new paragraph.
```

### Description example

```none
 * @par
 * OT CLI checks for `OPENTHREAD_CONFIG_BORDER_ROUTER_ENABLE`. If OTBR is enabled, it
 * registers local Network Data with the Leader. Otherwise, it calls the CLI function `otServerRegister`.
 * @moreinfo{@netdata}.
```

To review the HTML output, refer to [netdata register](https://openthread.io/reference/cli/commands#netdata_register).

## References and links

Use `@csa` to link to other commands. If a command is overloaded, include the
parentheses:

```none
 * @csa{joiner discerner (get)}
```

Use `@sa` to link to other API functions. If `@sa` is part of the `@cli`
block, the link only displays on the CLI Command Reference page; not the API
Reference.

### Link example

```none
 * @csa{br omrprefix}
 * @csa{br onlinkprefix}
 * @sa [NetworkData::ProcessShow
 * function](https://github.com/openthread/openthread/blob/main/src/cli/cli_network_data.cpp#L401)
 * @sa #otBorderRouterGetNetData
```

To review the HTML output, refer to [netdata show](https://openthread.io/reference/cli/commands#netdata_show).

## Doxygen special commands

CLI commands support the following Doxygen ALIASES and special commands:

| ALIAS | Example | Description |
| ---- | ---- | ---- |
| @cli | @cli ba port | Command name. Starts a CLI comment block. |
| @code | @code<br/>ba port<br/>41953<br/>Done<br/>@endcode | Command example. |
| @ca | [@ca{prefix}] | Command argument. Use brackets `[ ]` for optional arguments. |
| @cparam | @cparam joiner discerner @ca{discerner}<br/>Parameter details. | Command parameters. |
| @par | @par<br/>Optional CLI description. | CLI specific paragraphs. |
| @csa | @csa{prefix add} | Command See Also. Links to another command. |
| @sa | @sa otBorderRouterConfig | See Also. Creates a link to an API Reference. |
| @overview | @overview | Creates a link to the [OpenThread CLI Overview](https://openthread.io/reference/cli). |
| @netdata | @netdata | Creates a link to [Display and Manage Network Data with OT CLI](https://openthread.io/reference/cli/concepts/netdata). |
| @moreinfo | @moreinfo{@netdata} | Creates a referral link. |
| @note | @note Important callout. | Creates a note callout box. |

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
