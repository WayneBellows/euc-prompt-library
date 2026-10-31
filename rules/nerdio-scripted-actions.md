# Scripts sent to Nerdio Manager

- Never use a here-string (`@"..."@` or `@'...'@`) in a scripted action sent through the API. Nerdio Manager runs the script inside its own wrapper, and an inner `"@` can close it early. The rest of the script then runs as bare statements and fails with errors such as "The term '<' is not recognized".
- Never use backtick-escaped quotes inside strings. They do not survive the JSON round trip.
- Build multi-line text (HTML reports, config blocks) with `[System.Text.StringBuilder]` or plain `+` concatenation, using single-quoted strings.
- All the rules in [powershell-on-session-hosts.md](powershell-on-session-hosts.md) apply too: the script runs on 5.1.

If a scripted action works on your laptop and fails in Nerdio Manager, check for a here-string first.
