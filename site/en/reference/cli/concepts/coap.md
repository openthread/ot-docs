# Test  Connectivity to a Constrained Application Protocol (CoAP) Resource Using the OT CLI

OpenThread offers both CoAP server and client functionality, enabling devices
to connect to resources on the CoAP server and to observe each resource for
a change in its current state. The CoAP agent provided in the CLI can act
as either the CoAP client or server.

## CoAP commands

For a list of `coap` commands, type `help`:

> coap help
help
cancel
delete
get
observe
parameters
post
put
resource
set
start
stop
Done

## CLI Command Reference

For descriptions and syntax of all commands, refer to the CLI Command Reference.
The `coap` commands begin alphabetically with
[coap cancel](https://openthread.io/reference/cli/commands#coap_cancel).

## Example of CoAP server and client command usage

This example uses basic CLI commands to start a CoAP server and client, create
a test resource on the CoAP server, and have the CoAP client interact with the resource.
Sample data are used for illustrative purposes.

### Set up the CoAP server

On the CoAP server node, perform the following steps:

1. Start the CoAP agent.

   ```
   > coap start
   Done
   ```

1. Create a test resource.

   ```
   > coap resource test-resource
   Done
   ```

### Set up the CoAP Client

On the CoAP client node, perform the following steps:

1. Start the CoAP agent:

   ```
   > coap start
   Done
   ```

1. Run the `get` command to obtain information about the resource:

   ```
   > coap get fdde:ad00:beef:0:d395:daee:a75:3964 test-resource
   Done
   coap response from [fdde:ad00:beef:0:2780:9423:166c:1aac] with payload: 30
   ```
   The last portion of the server `response` is the term
   `with payload:`, followed
   by all payload bytes in hexadecimal digit format.
   Therefore, in the example, `with payload: 30` indicates that
   the current payload for the resource is set to
   one byte of payload information with a 0x30 hexadecimal value.
   For more information about using the `payload` option, refer to 
   [coap post](https://openthread.io/reference/cli/commands#coap_post). 

1. You can modify the resource using the `put` command:

   ```
   > coap put fdde:ad00:beef:0:2780:9423:166c:1aac test-resource con hellothere
   Done
   coap response from [fdde:ad00:beef:0:2780:9423:166c:1aac]
   ```
   In this example, `con` means that you want a reliable message, which is
   obtained using a confirmable message (`con`), to be sent to the CoAP server.
   The default is to send a non-confirmable (`non-con`) message.

   The string `hellothere` is an example of using the optional `payload`
   parameter when the `type` is either `con` or `non-con`."
   For more information, refer to
   [coap put](https://openthread.io/reference/cli/commands#coap_put).

   The server responds with its IPv6 address to indicate the request was handled.

### Responses sent to CoAP server

On the server, output from this example would be similar to the following:

```
coap request from [fdde:ad00:beef:0:b3:e3f6:2dcc:4b79] GET
coap response sent
coap request from [fdde:ad00:beef:0:b3:e3f6:2dcc:4b79] PUT with payload: 68656c6c6f7468657265
coap response sent
```

The `payload` value of `68656c6c6f7468657265` is the string `hellothere` converted
to ASCII code byte sequence.



