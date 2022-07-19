# Update OT CLI Commands

We generate [CLI Commands](https://openthread.io/reference/cli/commands)
from the CLI source files in the
[openthread](https://github.com/openthread/openthread/tree/main/src/cli)
GitHub repository.

This guide provides instructions on how to use our custom Doxygen comments
that we use to create the command list.

## Get started

To document a CLI command, complete the following steps. It's important that
you follow these steps in the order provided.

1.  Find the associated `Cmd` from the `openthread/src/cli`
    directory. For example, to find `ba state`, search for `Cmd("ba")`.
    Each command will have an associated function template:

    ```none
    template <> otError Interpreter::Process<Cmd("ba")>(Arg aArgs[])
    ```

1.  In the function template, locate the correct command logic.
    For example, `ba state`:

    ```none
    else if (aArgs[0] == "state")
    ```

1.  Before the logic begins, start the `@cli` Doxygen block:

    ```none
    /**
     * @cli ba state
    ```

1.  Next, immediately below `@cli`, add examples using `@code`. Include at
    least one example. Do not include the `>` prompt, and make sure to
    document the full return output, including the standard `Done` response.

    ```none
     * @code
     * ba state
     * Started
     * Done
     * @endcode
    ```

Because CLI commands provide command-line access to common API methods
and functions, some of the commands share the same description as their
corresponding API. If the command description is the same, complete the
following steps:

1.  Find the associated API from the `openthread/include/openthread`
    directory. For example, `ba state` maps to `otBorderAgentGetState`.

1.  Use the `api_copy` command, then enter the API name directly below it.
    In front of the API definition, make sure to use the pound sign `#`
    to create the automatic Doxygen link.

    ```none
    @par api_copy
    #otBorderAgentGetState
    ```

Here's a complete example of the minimum Doxygen comments required to
automatically generate an OT CLI command:

```none
/**
 * @cli ba state
 * @code
 * ba state
 * Started
 * Done
 * @endcode
 * @par api_copy
 * #otBorderAgentGetState
 */
```

To review the HTML output, refer to
[ba state](https://openthread.io/reference/cli/commands#ba_state).
For advanced examples and additional options, refer to the following sections.

## CLI template options

CLI commands are grouped by one continuous comment block, beginning with the
ALIAS tag `@cli`. Multiple `@code` examples are supported.

The order you specify each tag is important.

*   `@cparam` must come after `@code`
*   If you're copying an API description, `@par api_copy` must come after
    `@cparam`

```none
 * @cli command name (overload1,overload2)
 * @code
 * command name arg1
 * Done
 * @endcode
 * @cparam command name @ca{arg1} [@ca{opt-arg}] [@ca{choose-1}|@ca{choose-2}]
 * Optional parameter description; displays below the Parameter heading.
 * *   Markdown and lists are supported.
 * @par api_copy
 * #{apiName}
 * @par
 * Optional CLI specific paragraph. Markdown and lists are supported.
 * @note Notes are supported.
 * @csa{other command name}
 * @sa API method or function name
```

Next, learn more about how each ALIAS tag is used.

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
`netdata publish dnssrp unicast` provides a few options:

1.  Publish by address and port number
1.  Publish by port number and a device's Mesh-Local EID

If a command is overloaded, use parentheses to uniquely identify the command.

```none
 * @cli netdata publish dnssrp unicast (mle)
 * @cli netdata publish dnssrp unicast (addr,port)
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

## Add CLI paragraphs after `@api_copy`

Use `@par` to add more information to the description. Make sure to drop down to the
next line:

```none
 * @par api_copy
 * #otBorderAgentGetState
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

## Provide CLI-specific description only

Some CLI commands use multiple APIs, or differ from the API call.
Others don't have an associated API, for example `netdata help`.
To provide a separate description, use `@par`. Do not include a
`@par` title, and start your description on the next line. In
this scenario, you don't need to use `@api_copy`.

```none
/**
 * @cli netdata help
 * @code
 * netdata help
 * ...
 * show
 * steeringdata
 * unpublish
 * Done
 * @endcode
 * @par
 * Gets a list of `netdata` CLI commands.
 */
```

## Automatically link APIs

You can link to other API methods or functions with `#otFunctionName` or
`@sa`. Enter these links at the end of the CLI comment block.

```none
/**
 * @cli netdata unpublish dnssrp
 * @code
 * netdata unpublish dnssrp
 * Done
 * @endcode
 * @par api_copy
 * #otNetDataUnpublishDnsSrpService
 */
```

Links display in the **CLI and API References** heading. To review the HTML
output, refer to
[netdata unpublish dnssrp](https://openthread.io/reference/cli/commands#netdata_unpublish_dnssrp)

### Preventing automatic links

Sometimes, Doxygen might mistake a normal word as a link to a CLI class, for
example, the word `Joiner`. To prevent Doxygen from linking to keywords or class
names used in a sentence, use the `%` operator in front of the word,
for example:

```none
Clear the %Joiner discerner
```

For more information, refer to
[Automatic link generation](https://www.doxygen.nl/manual/autolink.html)
in the Doxygen guide.

## Automatically link to other commands

Use `@csa` to link to other commands.

```none
* @csa{netdata publish dnssrp anycast}
```

If a command is overloaded, include the parentheses and add a comma
if applicable. Don't use spaces inside the parentheses:

```none
* @csa{netdata publish dnssrp unicast (addr,port)}
* @csa{netdata publish dnssrp unicast (mle)}
```

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
