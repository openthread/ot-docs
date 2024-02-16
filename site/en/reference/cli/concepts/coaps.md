# Test Connectivity to a Constrained Application Protocol Secure (CoAPS) Resource Using the OT CLI

OpenThread offers both CoAPS server and client functionality, enabling devices
to connect to resources on the CoAPS server and to observe each resource for
a change in its current state.

CoAPS uses Datagram Transport Layer Security (DTLS) to establish secure,
end-to-end connections.

The CoAPS agent provided in the CLI can act as either the CoAPS client or CoAPS server.

This guide provides basic tasks that use some of the more common `coaps` commands.

## CoAPS commands

For a list of  `coaps` commands, type `help`:

> coaps help
connect
delete
disconnect
get
isclosed
isconnactive
isconnected
post
psk
put
resource
set
start
stop
x509
Done


## CLI Command Reference

For descriptions and syntax of all commands, refer to the CLI Command Reference.
The CoAPS commands begin alphabetically with
[coaps connect](https://openthread.io/reference/cli/commands#coaps_connect).


## Example of CoAPS server and client command usage

This example uses basic CLI commands to start a CoAPS server and client,
create a test resource on the CoAPS server, and have the CoAPS client
interact with the resource. Sample data are used for illustrative purposes.


### Configure DTLS ciphersuites

The `coaps` CLI provides `psk` and `x509` commands that can be used with
the PSK key and X.509 Certificate.
For command syntax and examples, refer to:
  * [coaps psk](https://openthread.io/reference/cli/commands#coaps_psk) 
  * [coaps x509](https://openthread.io/reference/cli/commands#coaps_x509)


### Set up the CoAPS server

On the CoAPS server node, perform the following steps:

1. Start the CoAPS agent.

   ```
   > coaps start
   Done
             ```

1. Create a test resource.

   ```
   > coaps resource test-resource
   Done
            ```

### Set up the CoAPS client

On the CoAPS client node, perform the following steps:

1. Start the CoAPS agent:

   ```
   > coaps start
   Done
   ```

1. Run the `connect` command to initialize a DTLS session with a peer:

   ```
   > coaps connect fdde:ad00:beef:0:9903:14b:27e0:5744
   Done
   coaps connected
   ```

1. Run the `get` command to obtain information about the resource:

   ```
   > coaps get test-resource
   Done
   coaps response from fdde:ad00:beef:0:9903:14b:27e0:5744 with payload: 68656c6c6f576f726c6400
   ```

   The last portion of the server response is the term `with payload:`, followed
   by all payload bytes in hexadecimal digit format. In the example,
   `with payload: 68656c6c6f576f726c6400` indicates that the current payload
   for the resource is the hexadecimal value `68656c6c6f576f726c6400`, which converts to the string
   `helloWorld`. For more information about using the `payload` option, refer to
   [coaps post](https://openthread.io/reference/cli/commands#coaps_post).

1. You can modify the resource using the `put` command:

   ```
   > coaps put test-resource con hellothere
   Done
   coaps response from fdde:ad00:beef:0:9903:14b:27e0:5744
   ```
   In this example, `con` means that you want a reliable message, which is
   obtained using a confirmable message (`con`), to be sent to the CoAPS server.
   The default is to send a non-confirmable (`non-con`) message.

   The string `hellothere` is an example of using the optional `payload`
   parameter when the `type` is either `con` or `non-con`."
   For more information, refer to
   [coaps put](https://openthread.io/reference/cli/commands#coaps_put).

   The server responds with its IPv6 address to indicate the request was handled.

### Responses sent to CoAPS server

On the server, output from this example would be similar to the following:

```
coaps request from fdde:ad00:beef:0:9e68:576f:714c:f395 GET
coaps response sent
coaps request from fdde:ad00:beef:0:9e68:576f:714c:f395 PUT with payload: 68656c6c6f7468657265
coaps response sent
```

The `payload` value of `68656c6c6f7468657265` is the string `hellothere` converted
to ASCII code byte sequence.
     
