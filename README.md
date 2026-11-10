# Fixlane

Fixlane tells you whether the developer software on your computer has known security problems, and gives your AI assistant a reviewed procedure to fix each one. It is a plugin for Claude Code, Claude, Codex and ChatGPT, and a remote [MCP](https://modelcontextprotocol.io) server that works with any MCP client.

The data is public and read-only. No account, no API key, nothing to install locally: the plugin only connects your assistant to `https://fixlane.poparecon.com/mcp`.

## What it does

Ask "is my computer safe?" or "is my Chrome affected by a known issue?". The `security-check` skill makes the assistant:

1. list your installed developer software and versions with read-only commands (no install, no sudo, no secrets read);
2. compare the names and versions with the versions that fix known incidents;
3. show you every affected program and its fixed version, and ask before changing anything;
4. follow the returned fix procedure exactly: a read-only check first, then the fix. Phones get manual steps only.

## Tools

| Tool | Use |
| --- | --- |
| `check_software` | A program, package or CVE id: known incidents, severity, whether it is exploited, the fixed version. |
| `get_fix_prompt` | An incident id and the operating system: the reviewed fix procedure (agent prompt for macOS, Windows, Linux; manual steps for iOS, Android). |
| `latest_incidents` | The newest incidents, optionally for one operating system and since a date. |
| `get_digest` | One fix prompt for everything to update now on a desktop system. |
| `check_inventory` | Up to 50 installed programs with versions: which are affected, not affected or unknown. |

All five tools are read-only.

## Install

Claude Code:

```
/plugin marketplace add lightsmite/fixlane-plugin
/plugin install fixlane@fixlane
```

Codex:

```
codex plugin marketplace add lightsmite/fixlane-plugin
codex plugin add fixlane@fixlane
```

Any other MCP client (Streamable HTTP, no authentication):

```json
{
  "mcpServers": {
    "fixlane": {
      "type": "http",
      "url": "https://fixlane.poparecon.com/mcp"
    }
  }
}
```

Claude Code without the plugin: `claude mcp add --transport http fixlane https://fixlane.poparecon.com/mcp`.

## Privacy Policy

The Fixlane server stores nothing about your questions: no query text, no software names, no IP address. Names and versions you send to `check_inventory` are used only to answer the call. A proxy or CDN in front of the server may handle connection data under its own policy. The full policy is at <https://fixlane.poparecon.com/privacy>; terms are at <https://fixlane.poparecon.com/terms>.

## Links

- Site: <https://fixlane.poparecon.com/>
- Docs: <https://fixlane.poparecon.com/docs>
- Support: <https://fixlane.poparecon.com/support>

## License

MIT, see `LICENSE`.
