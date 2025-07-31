**[[RPC]] (Remote Procedure Call)** — is a way to call a remote function like if it was local.

**[[REST]]** — is an architecture style over HTTP implying that every resource has its own URL and actions over these resources are performed with HTTP methods.

In a way REST request is an RPC call too; the difference is pretty vague and more ideological.

| RPC                                                                                                   | REST                                             |
| ----------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| You make a function call (`getOrder(id)`)                                                             | You operate with a resource (`GET /order/{id}`)  |
| You provide an IDL (Interface Definition Language) and a service contract - method and its arguments. | You define a resource and allow actions with it. |
| The focus is on the specific action.                                                                  | The focus is on the resource.                    |
