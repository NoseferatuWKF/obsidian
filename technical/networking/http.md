[rpc vs rest](https://aws.amazon.com/compare/the-difference-between-rpc-and-rest/)

| **RPC** | **REST** |
| ---- | ---- |
| A system allows a remote client to call a procedure on a server as if it were local. | A set of rules that defines structured data exchange between a client and a server. |
| Performing actions on a remote server. | Create, read, update, and delete (CRUD) operations on remote objects. |
| When requiring complex calculations or triggering a remote process on the server. | When server data and data structures need to be exposed uniformly. |
| Stateless or stateful. | Stateless. |
| In a consistent structure defined by the server and enforced on the client. | In a structure determined independently by the server. Multiple different formats can be passed within the same API. |

# CORS
```
Access-Control-Allow-Origins
Access-Control-Allow-Methods
Access-Control-Allow-Headers
```

[HTTP Security Response Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html)