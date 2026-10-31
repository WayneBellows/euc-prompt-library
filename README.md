# Inch

<img src="assets/inch-loop.gif" alt="Inch, a small teal pixel-art window with a progress bar" width="200" align="right">

Inch is what I call the agent that runs my skills, prompts and rules for Azure Virtual Desktop and Nerdio Manager work. This repo is everything Inch is made of, shared so you can give the same things to your own agent.

Inch reads, checks and suggests. The admin decides. Every prompt and skill here has been run against a real environment (my lab) before it was added, and each one says what it needs, what you still check yourself, and what happened when I tested it, including when the first version got it wrong.

The progress bar is the point: one small fix at a time, written down so it is never made twice. It never reaches 100%.

## What is in here

| Folder | What it is | Use it |
|---|---|---|
| [prompts/](prompts/) | One question for an agent, tested | Paste into any agent |
| [skills/](skills/) | A repeatable job, in the open `SKILL.md` format | Copy the folder into your agent's skills folder (for instance `.claude/skills/` for Claude Code) |
| [rules/](rules/) | Mistakes written down once | Copy into `CLAUDE.md`, `AGENTS.md` or `.github/copilot-instructions.md` |

## The rules every prompt and skill follows

1. **Read-only.** They ask an agent to read and report. None of them changes anything, and they say so.
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

## Skills

| Skill | What it does | Tested |
|---|---|---|
| [avd-hosts-left-running](skills/avd-hosts-left-running/SKILL.md) | Finds hosts that were started and never stopped, and quotes why | 16 Oct 2026 |
| [session-host-script-check](skills/session-host-script-check/SKILL.md) | Checks a PowerShell script will run on a 5.1 session host | 24 Oct 2026 |
| [nerdio-rest-api](skills/nerdio-rest-api/SKILL.md) | Reads from the Nerdio Manager REST API, and avoids its traps | 5 Oct 2026 |

## Rules

See [rules/](rules/README.md): PowerShell on session hosts, Nerdio scripted actions, `az vm run-command`, and agent safety.

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

These are my own prompts, skills and rules, shared as they are, with no warranty. They are not an official Nerdio or Microsoft resource. Run them against a test environment first, check the agent's answer against the source, and follow your organisation's rules on AI tools and data.

## Licence

MIT. See [LICENSE](LICENSE).
