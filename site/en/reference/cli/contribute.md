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

## Command descriptions

There are three ways to document command descriptions. You can copy the
API description, use the API description but add additional information,
or provide a completely unique description that belongs only to the
CLI command. In the next sections, we'll go over each method.

### Copy the corresponding API description

Use `api_copy` to copy the corresponding API method or function. When
you copy APIs, make sure that the description will work for both the
API and the CLI command. Avoid using phrases that apply only to a
function, for example `This function` or `This method`. Prefer
an active voice, for example `Gets the` or `Sets the`.

```none
 * @par api_copy
 * #otBorderAgentGetState
```

### Add more information to the API description

If you'd like to use `api_copy` but need to add additional information that
applies only to the CLI command, use `@par`. After the `@par` tag, make sure
to drop down to the next line.

```none
 * @par api_copy
 * #otBorderAgentGetState
 * @par
 * CLI description here; add a few things that do not apply to the API method.
 * @par
 * Start a new paragraph.
```

These paragraphs display after the API description.

### Provide CLI-specific descriptions only

Some CLI commands use multiple APIs, or differ from the API call.
Others don't have an associated API, for example `netdata help`.
To provide a separate description, use `@par`.
Do not include a `@par` title, and start your description on the next
line. In this scenario, do not use `@api_copy`.

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

### Parameters with markdown lists

You can also use lists after `@cparam`. This is helpful for when you'd like to
provide details about the command arguments that are used.

```none
 * @cparam netdata show [@ca{local}] [@ca{-x}]
 * *   The optional `-x` argument gets Network Data as hex-encoded TLVs.
 * *   The optional `local` argument gets local Network Data to sync with Leader.
```

To review the HTML output, refer to [netdata show](https://openthread.io/reference/cli/commands#netdata_show).

`@cli` blocks must be one continuous comment with no spaces. If you add
a list below `@cparam` and then need another paragraph below that list, use
a period `.` to manually end the list.

```none
* @cparam commandname [@ca{qmr}]
* [`q`, `m`, and `r`] map to #otLinkMetricsValues.
* *   `q`: Layer 2 LQI (#otLinkMetricsValues::mLqiValue).
* *   `m`: Link Margin (#otLinkMetricsValues::mLinkMarginValue).
* *   `r`: RSSI (#otLinkMetricsValues::mRssiValue).
* .
* Add another paragraph here. The dot above will end the HTML list that's generated.
* This paragraph displays under the Parameters heading, and not the command description.
```

For additional examples, refer to Doxygen's
[Lists](https://doxygen.nl/manual/lists.html).

## Automatically link APIs

You can link to other API methods or functions with `#otFunctionName` or
`@sa`. Enter these links at the end of the CLI comment block.

```none
/**
 * @cli netdata publish prefix
 * @code
 * netdata publish prefix fd00:1234:5678::/64 paos med
 * Done
 * @endcode
 * @cparam netdata publish prefix @ca{prefix} [@ca{padcrosnD}] [@ca{high}|@ca{med}|@ca{low}]
 * OT CLI uses mapped arguments to configure #otBorderRouterConfig values. @moreinfo{the @overview}.
 * @par
 * Publish an on-mesh prefix entry. @moreinfo{@netdata}.
 * @sa otNetDataPublishOnMeshPrefix
 */
```

`@sa` links display in the **CLI and API References** heading. To review the HTML
output, refer to
[netdata publish prefix](https://openthread.io/reference/cli/commands#netdata_publish_prefix).

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
| @dataset | @dataset | Creates a link to [Display and Manage Datasets with OT CLI](https://openthread.io/reference/cli/concepts/dataset). |
| @udp | @udp | Creates a link to [Test UDP Functionality With OT CLI](https://openthread.io/reference/cli/concepts/udp). |
| @moreinfo | @moreinfo{@netdata} | Creates a referral link. |
| @note | @note Important callout. | Creates a note callout box. |

## Broken lines from OpenThread `make pretty` script

Occasionally, the `make pretty` script causes long lines to break incorrectly.
This can happen with a long list of parameters or long lines of command output
that go beyond the column width limit that the `make pretty` check imposes. 
This situation can be addressed by enclosing line breaks with an HTML comment
tag, as shown in two different examples below. 

The first example is the `dns resolve` command that can take up to six
parameters. To correctly render the syntax using Doxygen while still passing
the `make pretty` check, the parameters must be split across three lines
in the source:

```none
* @cparam dns resolve @ca{hostname} [@ca{dns-server-IP}] <!--
* -->                 [@ca{dns-server-port}] [@ca{response-timeout-ms}] <!--
* -->                 [@ca{max-tx-attempts}] [@ca{recursion-desired-boolean}]
```
The second example is the output of the `history ipaddr list 5` command.
For the output to render properly and still pass the `make pretty` check,
each of the five output lines must be split into two lines, as follows:

```none
* history ipaddr list 5
* 00:00:20.327 -> event:Removed address:2001:dead:beef:cafe:c4cb:caba:8d55:e30b <!--
* -->prefixlen:64 origin:slaac scope:14 preferred:yes valid:yes rloc:no
* 00:00:59.983 -> event:Added address:2001:dead:beef:cafe:c4cb:caba:8d55:e30b <!--
* -->prefixlen:64 origin:slaac scope:14 preferred:yes valid:yes rloc:no
* 00:01:22.535 -> event:Added address:fd00:0:0:0:0:0:0:1 prefixlen:64 <!--
* -->origin:manual scope:14 preferred:yes valid:yes rloc:no
* 00:02:33.221 -> event:Added address:fdde:ad00:beef:0:0:ff:fe00:fc00 <!--
* -->prefixlen:64 origin:thread scope:3 preferred:no valid:yes rloc:no
* 00:02:33.221 -> event:Added address:fdde:ad00:beef:0:0:ff:fe00:5400 <!--
* -->prefixlen:64 origin:thread scope:3 preferred:no valid:yes rloc:yes
* Done
```

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
