# SORACOM MCP Catalog

Official catalog of remote [Model Context Protocol (MCP)](https://modelcontextprotocol.io) servers provided by [SORACOM](https://soracom.io).

> [!NOTE]
> This repository contains documentation and MCP registry manifests (`server.json`) only.
> The server implementations are hosted by SORACOM and are not open source.

## Servers

| Server | Registry name | Endpoint | Auth | Docs |
|---|---|---|---|---|
| Soracom Knowledge MCP Server | `com.soracom/knowledge` | `https://knowledge-mcp.soracom.com` | None | [servers/knowledge](servers/knowledge/) · [EN](https://developers.soracom.io/en/docs/tools/knowledge-mcp-server/) · [JA](https://users.soracom.io/ja-jp/tools/soracom-knowledge-mcp-server/) |

All servers are published to the [Official MCP Registry](https://registry.modelcontextprotocol.io) under the `com.soracom` namespace. To verify:

```bash
curl -sS "https://registry.modelcontextprotocol.io/v0/servers?search=soracom"
```

## Repository layout

- `servers/<name>/README.md` — what the server does, its tools, and client setup
- `servers/<name>/server.json` — the canonical manifest for publication to the Official MCP Registry

## Feedback

For questions and feedback about the servers listed here, contact [SORACOM Support](https://support.soracom.io/hc/).

## License

[MIT](LICENSE)
