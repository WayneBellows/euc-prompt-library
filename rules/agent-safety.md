# Agent safety

The same five rules every prompt and skill in this repo follows.

1. **Read-only first.** Connect MCP servers read-only where they offer it. A prompt that reads says "Do not change anything".
2. **Quote the evidence.** Every finding quotes the log line, task ID or query result it came from, so the admin can check it.
3. **Say "the log does not show it".** When the data does not explain something, say so. Never guess.
4. **Ask about what did not happen.** A host nothing tried to stop never appears in a list of stop actions. Ask for both.
5. **The admin decides.** The agent reads and suggests. A person makes every change.

Also: never print a secret, token or connection string, even while troubleshooting. Report only whether it is set and its length.
