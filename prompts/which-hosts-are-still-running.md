# Which hosts are still running, and why?

**Use it when:** you want the same answer as [Why didn't this host stop?](why-didnt-this-host-stop.md), but straight from Nerdio Manager, with no export.

## Prompt

```text
Using the Nerdio task log, look at host pool <name> for the last 30 days. List every host that was started and never stopped afterwards, and any change to the auto-scale settings in the period, with who made it. For each host still running, say what the log shows about why, quoting the log line, or say that the log does not show it. Do not change anything.
```

## Needs

- Nerdio Manager for Enterprise 8.2 or later, with the MCP server turned on (public preview, read-only).
- An agent connected through Nerdio's bridge (see the [README](../README.md)). You sign in with your own Entra ID account, so the agent sees only what your Nerdio Manager role sees.

## You still check

- That the quoted task lines exist. The answer includes task IDs, so you can look them up in the console.
- Whether the host is meant to be on.
- Whether a status or heartbeat time is current. Old timestamps can mislead.

## Tested

Lab, 16 October 2026. 85 seconds. The agent found the host that had been left running and, because it could open each task's detail, the line that explained why: `Enable Auto-Scale: True -> False`. The exported task summary used for the file-based prompt only said "Update dynamic scale settings".
