# az vm run-command

- The returned output is cut at about 4,096 bytes, with no warning. Do not use it to pull a report or a file off a VM, and do not try to squeeze it through with base64.
- To get a file out, upload it from the VM to blob storage and download it locally. Build the complete upload URL with `az storage blob generate-sas --full-uri` on your own machine, never by joining strings on the VM. Test the URL with a tiny upload before pushing the real script.
- Sending a script in is fine. A large script passed as `--scripts @file` works; the limit is on what comes back.
- Do not start a child `powershell.exe -File ...` inside the run-command script. The outer shell exits 0 whatever the child does, so a failure looks like a success. Pass the script directly with `--scripts @file`.
