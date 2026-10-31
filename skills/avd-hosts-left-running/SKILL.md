---
name: avd-hosts-left-running
description: Find Azure Virtual Desktop session hosts that were started and never stopped, and say why, from the Nerdio Manager task log (MCP) or a task log export. Read-only. Use when hosts are still running overnight, auto-scale savings look low, or someone asks "why didn't this host stop?".
---

# Hosts left running

Read-only. Never start, stop, drain or change anything. The admin decides what to do with the answer.

## Pick the source

- **Nerdio Manager MCP server connected** (Nerdio Manager 8.2 or later, public preview): read the task log directly. Better, because each task's detail is readable.
- **No MCP:** ask the admin for a task log export (CSV) of the host pool for the last 30 days.

## Ask the right question

A list of stop actions never shows a host that nothing tried to stop. Always ask for both:

1. Every host that was **started and never stopped** afterwards in the period.
2. Every **change to the auto-scale settings** in the period, with who made it.

## Steps

1. Confirm the host pool name and the period (default: last 30 days).
2. Read the task log for that pool and period.
3. Build the two lists above.
4. For each host still running, say what the log shows about why, **quoting the log line and task ID**. If the log does not explain it, say "the log does not show it". Do not guess.
5. Flag timestamps that look stale (an old heartbeat or status time can mislead).

## Report

One short table: host, started (time), stopped (time or "never"), the quoted reason or "the log does not show it". Then a list of auto-scale setting changes with who and when. End with what the admin still needs to check:

- that the quoted task lines exist (look up the task IDs in the console),
- whether each host is meant to be on.

## Tested

Lab, 16 October 2026.

- Export version: the first prompt asked only for scale-in actions. It returned a tidy table in 46 seconds and missed a host that had been running for nine days, because auto-scale had been switched off and the host was started by hand. Asking for "started and never stopped" plus setting changes found it in 76 seconds.
- MCP version: 85 seconds, and it found the line that explained why: `Enable Auto-Scale: True -> False`.

The prompts behind this skill: [Why didn't this host stop?](../../prompts/why-didnt-this-host-stop.md) and [Which hosts are still running?](../../prompts/which-hosts-are-still-running.md)
