# Testing  Connectivity to a Constrained Application Protocol (CoAP) Resource Using the OT CLI

OpenThread offers both CoAP server and client functionality, enabling devices
to connect to resources on the CoAP server, and observe each resource for
a change in its current state.

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
The SRP server commands begin alphabetically with
[coap cancel](https://openthread.io/reference/cli/commands#coap_cancel).

## Example of CoAP server and client command usage

This example use basic CLI commands to start a CoAP server and client, create
a test resource on the CoAP server, and have the CoAP client interact with the resource.
Sample data are used for illustrative purposes.

### Set up the CoAP server

On the CoAP server node, perform the following steps:

1. Start the CoAP server:
   ```
   > coap start
   Done
   ```
1. Create a test resource:
   ```
   > coap resource test-resource
   Done
   ```

### Set up the CoAP Client

On the CoAP client node, perform the following steps:

1. Start the CoAP service:

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
   The `with payload: 30` item in the response from the server indicates that
   the current payload for the resource is set to sent 30 blocks of information.
   For more information about sending blocks of information and using the
   `payload` option, refer to 
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



