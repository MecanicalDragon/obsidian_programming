**RPC (Remote Procedure Call)** — is a way to call a remote function like it was local.

**REST** — is an architecture style over HTTP where every resource has a URL and actions over these resources and performed with HTTP methods.

In a way REST request is an RPC call too; the difference is pretty vague and more ideological.

| RPC                                                                | REST                                             |
| ------------------------------------------------------------------ | ------------------------------------------------ |
| You make a function call (`getUser(id)`)                           | You operate with a resource (`GET /user/{id}`)   |
| You provide an IDL, a service contract - method and its arguments. | You define a resource and allow actions with it. |
| The focus is on the specific action.                               | The focus is on the resource.                    |
