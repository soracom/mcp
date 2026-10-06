# Soracom Knowledge MCP Server

The Soracom Knowledge MCP Server is an [MCP (Model Context Protocol)](https://modelcontextprotocol.io/) server that lets AI agents search Soracom documentation and Soracom API references directly from your development environment. When connected to an MCP client such as [Claude Code](https://docs.claude.com/en/docs/claude-code/overview), [OpenAI Codex](https://developers.openai.com/codex/), or Claude Desktop, the agent can look up service guides, Soracom User Console procedures, pricing, IoT recipes, Soracom API references, CLI commands, and SAM permissions on demand, and ground its answers in the official documentation instead of relying only on its training data.

The server is hosted by Soracom as a shared, read-only endpoint. It does not modify any of your Soracom resources—the agent only searches and reads documentation.

## Available Tools

The server exposes three read-only tools to the connected agent. None of them modify your account or its resources.

The server's default `hybrid` search mode combines keyword search with semantic search (using OpenAI embeddings) and merges the results using Reciprocal Rank Fusion (RRF).

### search_soracom_docs

Searches Soracom service guides, Soracom User Console procedures, service overviews and pricing, and IoT recipes.

| Parameter | Description |
|---|---|
| `query` | A natural-language search query (required). |
| `document_names` | Restricts the search to specific sites (e.g., `["developers.soracom.io", "users.soracom.io"]`). The available sites are listed in the tool schema. Omit it to search all sources. |
| `search_mode` | `hybrid` (default; combines keyword and semantic search), `keyword` (keyword search only), or `semantic` (semantic similarity only). |
| `language` | `English` or `Japanese`. Auto-detected from the query when omitted. |
| `max_results` | Number of results to return. Default 16, maximum 32. |

### search_api_docs

Searches Soracom API references, CLI commands, and SAM permissions.

| Parameter | Description |
|---|---|
| `query` | A natural-language question or an API endpoint name. Optional when at least one of `tag`, `method`, or `path` is supplied. |
| `tag` | Restricts results to a service tag (e.g., `Sim`, `Subscriber`, `Flux`). Case-insensitive exact match. |
| `method` | Restricts results to an HTTP method (e.g., `GET`, `POST`). Case-insensitive exact match. |
| `path` | Restricts results to an API path fragment (e.g., `/sims`). Case-insensitive substring match. |
| `search_mode` | `hybrid` (default; combines keyword and semantic search), `keyword` (keyword search only), or `semantic` (semantic similarity only). |
| `language` | `English` or `Japanese`. Auto-detected from `query` when `query` is provided. If you omit `query`, specify `language` explicitly. |
| `max_results` | Number of results to return. Default 16, maximum 32. |

### get_document

Fetches the full Markdown of a page returned by `search_soracom_docs` or `search_api_docs`.

| Parameter | Description |
|---|---|
| `url` | The URL of the page to fetch (required). Pass the `url` value from a search result verbatim. A search result may return a URL that is not directly viewable in a browser (such as a fragment of a split API reference), but this tool can still fetch its content. |

## Connecting the Server

The server is reachable at a stable public HTTPS endpoint and uses the MCP Streamable HTTP transport:

```text
https://knowledge-mcp.soracom.com
```

> [!WARNING]
> The server has no per-user authentication. It only searches and returns public Soracom documentation, but any query you enter is sent to the endpoint. With the `hybrid` or `semantic` search mode, your query is also sent to OpenAI to generate embeddings. Do not include confidential information, account identifiers, credentials, or personal data in your queries.
>
> For Soracom's terms and privacy practices, see the [Terms and Conditions](https://www.soracom.io/terms/) and [Privacy Policy](https://www.soracom.io/privacy-policy/). Soracom does not record or retain the content of your queries in its server logs. Soracom also does not share OpenAI API inputs or outputs with OpenAI for model improvement, evaluation, fine-tuning, or training. For details, see [OpenAI's Business data privacy, security, and compliance](https://openai.com/business-data/).
>
> Use of this server is subject to our [Site Policy](https://soracom.jp/share/sitepolicy/) (available in Japanese), which includes disclaimers, copyright terms, and provides that use is governed by the laws of Japan, with the Tokyo District Court as the exclusive court of first instance. The Japanese text prevails.

### Claude Code

Claude Code supports remote HTTP MCP servers natively. Add the server with the `claude mcp add` command:

```bash
claude mcp add --transport http soracom-knowledge https://knowledge-mcp.soracom.com
```

Verify that the server is connected:

```bash
claude mcp list
```

The `soracom-knowledge` server and its three tools are now available in your Claude Code sessions. To remove it later, run `claude mcp remove soracom-knowledge`.

### OpenAI Codex

The Codex CLI supports remote MCP servers over HTTP. Add the server with:

```bash
codex mcp add soracom-knowledge --url https://knowledge-mcp.soracom.com
```

List configured servers to confirm:

```bash
codex mcp list
```

## Notes and Limitations

- This server is provided free of charge on a best-effort basis and without an SLA. Its specifications may change without notice as the service is improved.
- Access may be temporarily restricted if an excessive number of requests are detected.
- A result set may include pages that are only loosely related to your query. The agent is expected to filter these; treat individual hits as candidates rather than authoritative matches.
- The information returned by this server is provided "AS-IS", and answers generated by an AI agent are not guaranteed to be accurate. Verify important details against the linked source pages.
- The search index is updated periodically, so it may take some time for changes to the public documentation to be reflected in search results.

---

This page mirrors the official documentation: [English](https://docs.soracom.io/en/developers/knowledge-mcp-server) | [Japanese](https://users.soracom.io/ja-jp/tools/soracom-knowledge-mcp-server/). Registry: `com.soracom/knowledge` on the [Official MCP Registry](https://registry.modelcontextprotocol.io).
