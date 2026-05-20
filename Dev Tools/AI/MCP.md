## Participants

MCP follows a client-server architecture where an MCP host — an AI application like [Claude Code](https://www.anthropic.com/claude-code) or [Claude Desktop](https://www.claude.ai/download) — establishes connections to one or more MCP servers. The MCP host accomplishes this by creating one MCP client for each MCP server. Each MCP client maintains a dedicated connection with its corresponding MCP server.Local MCP servers that use the STDIO transport typically serve a single MCP client, whereas remote MCP servers that use the Streamable HTTP transport will typically serve many MCP clients.The key participants in the MCP architecture are:

- **MCP Host**: The AI application that coordinates and manages one or multiple MCP clients
- **MCP Client**: A component that maintains a connection to an MCP server and obtains context from an MCP server for the MCP host to use. Understanding the distinction is important: the _host_ is the application users interact with, while _clients_ are the protocol-level components that enable server connections.
- **MCP Server**: A program that provides context to MCP clients

## Layers

MCP consists of two layers:
- **Data layer**: Defines the JSON-RPC based protocol for client-server communication, including lifecycle management, and core primitives, such as tools, resources, prompts and notifications.
- **Transport layer**: Defines the communication mechanisms and channels that enable data exchange between clients and servers, including transport-specific connection establishment, message framing, and authorization.

MCP uses [JSON-RPC 2.0](https://www.jsonrpc.org/) as its underlying RPC protocol. Client and servers send requests to each other and respond accordingly. Notifications can be used when no response is required.

SSE (Server-Sent Events) is a protocol over http to send events from the server to the client over the same http connection that can be renewed in the case of breakage. Streamable HTTP transport uses HTTP POST for client-to-server messages with optional SSE.

MCP supports real-time notifications that enable servers to inform clients about changes without being explicitly requested. This demonstrates the notification system, a key feature that keeps MCP connections synchronized and responsive.

## Primitives

MCP primitives are the most important concept within MCP. They define what clients and servers can offer each other. These primitives specify the types of contextual information that can be shared with AI applications and the range of actions that can be performed.MCP defines three core primitives that _servers_ can expose:

- **Tools**: Executable functions that AI applications can invoke to perform actions (e.g., file operations, API calls, database queries). Search flights, Send messages, Create calendar events... Model controls it.
- **Resources**: Data sources that provide contextual information to AI applications (e.g., file contents, database records, API responses). Retrieve documents, Access knowledge bases, Read calendars... Application controls it.
- **Prompts**: Reusable templates that help structure interactions with language models (e.g., system prompts, few-shot examples). Plan a vacation, Summarize my meetings, Draft an email... User controls it.

MCP also defines primitives that _clients_ can expose. These primitives allow MCP server authors to build richer interactions.

- **Sampling**: Allows servers to request language model completions from the client’s AI application. This is useful when server authors want access to a language model processing results. They can use the `sampling/createMessage` method to request a language model completion from the client’s AI application. Example: a server for booking travel may send a list of flights to an LLM and request that the LLM pick the best flight for the user.
- **Elicitation**: Allows servers to request additional information from users. This is useful when server authors want to get more information from the user, or ask for confirmation of an action. They can use the `elicitation/create` method to request additional information from the user. Example: a server booking travel may ask for the user’s preferences on airplane seats, room type or their contact number to finalize a booking.
- **Roots**: Allow clients to specify which directories servers should focus on, communicating intended scope through a coordination mechanism. Example: a server for booking travel may be given access to a specific directory, from which it can read a user’s calendar.

---
https://modelcontextprotocol.io/docs/getting-started/intro
