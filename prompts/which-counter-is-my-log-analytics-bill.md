# Which counter is my Log Analytics bill?

**Use it when:** the Log Analytics workspace behind your AVD monitoring costs more than you expect.

## Prompt

```text
Find the Log Analytics workspace that holds my AVD performance counters. Rank the counters by billable size over the last 30 days. Show the top five and flag anything over 50%. Do not change anything.
```

Add your subscription ID to the prompt if you have more than one.

## Needs

- An agent connected to Microsoft's Azure MCP Server, started with `--read-only` (see the [README](../README.md)).
- An Azure CLI sign-in with read access to the workspace, for example Log Analytics Reader.

## You still check

- That the agent picked the right workspace.
- That the share is of billable data (`_BilledSize` where `_IsBillable` is true).
- Whether anyone uses that counter (workbooks, alerts) before you sample it less often or switch it off. You make that change yourself; the prompt does not.

## Tested

Lab, 15 October 2026. About two minutes. The agent checked seven workspaces, found the one that held the AVD performance data, and ranked the counters. One counter, **User Input Delay per Process**, was **88.7%** of the billable performance data over 30 days; the next was 1.6%. A manual KQL query gave the same answer.

I have seen the same counter take most of the ingestion in real customer environments, so it is worth checking first.
