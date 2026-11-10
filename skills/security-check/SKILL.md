---
name: security-check
description: Use when the user asks whether their computer, phone or a program is safe, asks to check installed software for known security issues, or asks about a CVE, a malicious package or what is new in security incidents. Uses the Fixlane MCP tools.
---

# Security check with Fixlane

Fixlane serves reviewed fix procedures for known security incidents in developer software. Use only its tools for incident data and procedures.

## Which tool

- One program, package or CVE ("is Chrome safe?", "CVE-2026-1234"): call `check_software` with the name and the user's `os`.
- "What is new?": call `latest_incidents` (optionally with `os`).
- "Fix everything" on a desktop: call `get_digest` with the `os` (`macos`, `windows` or `linux`).
- "Is my machine safe?" or "check my software": follow the steps below.

## Check the whole machine

1. Take a read-only inventory of the installed developer software and browsers with the platform's own listing commands. Never install, update or download anything, never use sudo or an administrator shell, never read secrets, tokens, keys or credential files, never change or disable a security tool.
   - macOS: `sw_vers -productVersion`, `brew list --versions`, `ls /Applications`, `mdls -name kMDItemVersion -raw "/Applications/<App>.app"`.
   - Linux: `cat /etc/os-release`, `dpkg-query -W -f='${Package} ${Version}\n'` or `rpm -qa --qf '%{NAME} %{VERSION}\n'`, `npm ls -g --depth=0`, `pip list`, and `<tool> --version` for browsers and editors.
   - Windows (PowerShell): `[System.Environment]::OSVersion.Version`, `Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*, HKLM:\Software\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion`, `npm ls -g --depth=0`, `pip list`.
   - Phones: you cannot list apps. Ask the user for the program names and versions, then use `check_software`.
2. Keep developer tools, browsers and packages; at most 50 items. Send names and versions only, never paths, user names, host names or file contents. Tell the user which names and versions you are about to send.
3. Call `check_inventory` with `items` (`name`, `version`) and the `os`. Items can be `affected`, `not_affected` or `unknown`; report `unknown` as unknown, never as safe.
4. For each `affected` item call `get_fix_prompt` with its `incident_id` and the `os`.
5. Show the user the findings: program, installed version, incident, severity, fixed version. Ask before changing anything.
6. After the user agrees, follow the returned procedure exactly and in order: its read-only detection first, then the fix it describes. Run only the commands it gives; add none of your own. Never pipe a download into a shell. Stop and tell the user when a step does not match the machine.

## Phones

`ios` and `android` get manual steps only. Show the steps to the user; do not run anything.
