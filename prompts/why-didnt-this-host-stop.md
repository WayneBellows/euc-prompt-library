# Why didn't this host stop?

**Use it when:** a pooled or personal host ran all night (or for days) and you want to know why.

## Prompt

```text
Here is a host pool task log export (JSON). For each night, list every scale-in action per host: drain, stop, or taken out of drain. Then list every host that was started and never stopped afterwards, and any change to the auto-scale settings in the period, with who made it. For each host that did not stop, say what the log shows about why, quoting the log line, or say that the log does not show it. Do not suggest changes yet.
```

## Needs

- The host pool's task log as JSON, covering at least a few nights.
- Any agent that can read an attached file. It needs no access to your environment.

## You still check

- That each quoted line is really in the log.
- Whether the host was on for a reason the log cannot show: a night shift, a test, a change someone already raised.
- Which fix to make: the scale-in trigger, its threshold, session time limits, or minimum active hosts.

## Tested

Lab, 16 October 2026, 57 tasks over four weeks:

- **Version 1** asked only for scale-in actions. In 46 seconds it produced an accurate table of every nightly stop, including one stop that failed and then worked on a retry. It missed the real problem. One host had been running for nine days: auto-scale had been switched off, and someone then started the host by hand. Nothing ever tried to stop it, so a prompt that only looks for stop actions could not see it.
- **Version 2** (above) also asks for hosts that were started and never stopped, and for setting changes. In 76 seconds it found the manual start, the date of the auto-scale change and who made it. Where the log gave no reason, it said so.

**Lesson:** ask about what did not happen, not just what did.
