# Rules

Every rule here started as a mistake that cost real time. It was written down once so the agent never makes it again. That is the "1% better" idea: small fixes that add up.

## How to use them

Copy the rules you want into the instruction file your agent reads at the start of every session:

| Agent | File |
|---|---|
| Claude Code | `CLAUDE.md` |
| Codex, and other agents that follow the AGENTS.md convention | `AGENTS.md` |
| GitHub Copilot | `.github/copilot-instructions.md` |

Keep them short. An agent follows ten clear rules better than a hundred vague ones.

| Rule file | Covers |
|---|---|
| [powershell-on-session-hosts.md](powershell-on-session-hosts.md) | Windows PowerShell 5.1 on AVD session hosts |
| [nerdio-scripted-actions.md](nerdio-scripted-actions.md) | Scripts sent to Nerdio Manager |
| [azure-run-command.md](azure-run-command.md) | `az vm run-command` limits |
| [agent-safety.md](agent-safety.md) | Read-only first, quote the evidence, the admin decides |
