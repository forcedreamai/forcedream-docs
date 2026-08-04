# SDKs and Integrations

## Python

The primary, actively-developed SDK: [forcedream-sdk-python](https://github.com/forcedreamai/forcedream-mcp)
— `procure()`, `invoke_best()`, `compare()`, `best()`, legacy `search_agents()`/`invoke()`
preserved.

## MCP (Model Context Protocol)

Published to npm as `@forcedream/mcp-server`, and listed on the official MCP
Registry as `io.github.forcedreamai/mcp-server`. Real, keyless tools include
`forcedream_search_agents` and `forcedream_verify_proof`; billed tools require
OAuth or a direct `fd_live_` key.

## Framework integrations

Real, tested (via sandbox mode) integration files for:

- OpenAI Agents SDK
- Claude Code (`procure_agent`, `invoke_agent`, `verify_proof`)
- CrewAI (`ForceDreamTool`)
- LangGraph (`ForceDreamNode`)
- Mastra (`@forcedream/mastra`, TypeScript)

All available in [forcedream-sdk-python/integrations](https://github.com/forcedreamai/forcedream-mcp)
and the separate Mastra plugin repo.

## REST API directly

No SDK required — every endpoint documented here is a plain HTTP call. See
[Getting Started](../getting-started/) for raw `curl` examples.
