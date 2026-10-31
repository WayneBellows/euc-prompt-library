# PowerShell on AVD session hosts

- Session hosts run Windows PowerShell 5.1, not PowerShell 7. Write every script for 5.1.
- Never use `??`, the ternary `? :`, `&&` or `||`. They are parse errors on 5.1, and the script exits with code 1 and no useful message. Use a plain `if`.
- Before deploying, run the 5.1 parser on the script (`powershell.exe`, not `pwsh`). The [session-host-script-check](../skills/session-host-script-check/SKILL.md) skill has the command.
- A clean parse is not enough. Also check for a bare `(if ...)` passed as an argument, `@($list)` of an empty `List[object]` inside `[ordered]@{}`, and `$name?.Property`. These parse cleanly and fail at run time.
- `Get-ItemProperty -Name X -ErrorAction SilentlyContinue` still adds a record to `$Error` for every missing value. Read the whole key and test `$item.PSObject.Properties[$Name]`. For `Get-WinEvent` with no matching events, use `-ErrorAction Ignore`.
- Before any HTTPS call in SYSTEM context, set `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`.
- Keep scripts ASCII. An em dash or a Unicode arrow, even in a comment, can break parsing after the file is re-encoded on the way to the host.

Tested: the traps were re-tested 24 October 2026 on PowerShell 5.1.26100 and 7.6.8.
