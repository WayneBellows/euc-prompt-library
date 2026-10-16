# EUC prompt library

Prompts I use to let an AI agent do the reading on Azure Virtual Desktop and Nerdio Manager work, while the admin keeps the decision.

Every prompt here has been run against a real environment (my lab) before it was added. Each entry says what it needs, what you still have to check yourself, and what happened when I tested it, including when the first version got it wrong.

## The rules every prompt follows

1. **Read-only.** The prompts ask an agent to read and report. None of them changes anything, and they say so ("Do not change anything").
2. **Quote the evidence.** The agent quotes the log line or the query result it relied on, so you can check it.
3. **Say "the log does not show it".** A good prompt asks the agent to admit when the data does not explain something, instead of guessing.
4. **Ask about what did not happen.** A host that nothing ever tried to stop never appears in a list of stop actions. Ask for both.
5. **You make the call.** The agent reads; the admin decides.

## Prompts

| Prompt | Works with | Tested |
|---|---|---|
| [Why didn't this host stop?](prompts/why-didnt-this-host-stop.md) | Any agent that can read a file (a task log export) | 16 Oct 2026 |
| [Which hosts are still running, and why?](prompts/which-hosts-are-still-running.md) | An agent connected to the Nerdio Manager MCP server | 16 Oct 2026 |
| [Which counter is my Log Analytics bill?](prompts/which-counter-is-my-log-analytics-bill.md) | An agent connected to Microsoft's Azure MCP Server | 15 Oct 2026 |

## Connecting an agent (MCP)

MCP (Model Context Protocol) gives an agent a set of tools it can call with your permissions. Both servers below can run read-only, and neither is tied to one AI vendor. I tested both with an agent other than the one I normally use.

**Azure MCP Server (Microsoft).** Runs on your machine and signs in as you through the Azure CLI (`az login`). Start it read-only:

```json
{
  "mcpServers": {
    "azure": {
      "command": "npx",
      "args": ["-y", "@azure/mcp@latest", "server", "start", "--read-only"]
    }
  }
}
```

Docs: https://learn.microsoft.com/en-us/azure/developer/azure-mcp-server/overview

**Nerdio Manager for Enterprise MCP server.** Public preview with Nerdio Manager 8.2, read-only. An admin turns it on in Nerdio Manager. You connect through Nerdio's bridge, which signs you in with your own Entra ID account, so the agent sees only what your Nerdio Manager role sees:

```json
{
  "mcpServers": {
    "nerdio": {
      "command": "npx",
      "args": ["-y", "@nerdio-inc/mcp-bridge", "https://<your-nerdio-manager-app>/mcp"]
    }
  }
}
```

Field names differ between clients (`mcpServers`, `servers`, or a TOML table), so check your agent's documentation.

## Disclaimer

These are my own prompts, shared as they are, with no warranty. They are not an official Nerdio or Microsoft resource. Run them against a test environment first, check the agent's answer against the source, and follow your organisation's rules on AI tools and data.

## Licence

MIT. See [LICENSE](LICENSE).
