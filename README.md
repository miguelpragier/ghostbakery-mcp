# Ghost Bakery MCP server

`ghostbakery-mcp` connects Claude, Cursor or any other [MCP](https://modelcontextprotocol.io) client to [Ghost Bakery](https://ghostbakery.com), so an assistant can write, check and run fake-data recipes for you, with a spending limit you set.

Ask for *"500 Brazilian customers with a valid CPF as CSV"*: the assistant looks up the field types, writes a recipe, validates it, shows you sample records and tells you the cost before generating anything.

This repository publishes the release binaries. Full guide: **<https://ghostbakery.com/docs/guides/mcp>**.

## Install

1. Download the file for your system from the [latest release](https://github.com/miguelpragier/ghostbakery-mcp/releases/latest):

   | System | File |
   | --- | --- |
   | Linux x86-64 | `ghostbakery-mcp_<version>_linux_amd64` |
   | Linux ARM64 | `ghostbakery-mcp_<version>_linux_arm64` |
   | macOS Intel | `ghostbakery-mcp_<version>_darwin_amd64` |
   | macOS Apple silicon | `ghostbakery-mcp_<version>_darwin_arm64` |
   | Windows x86-64 | `ghostbakery-mcp_<version>_windows_amd64.exe` |

2. Check it against `SHA256SUMS` from the same release:

   ```sh
   sha256sum --check --ignore-missing SHA256SUMS
   ```

3. Rename it to `ghostbakery-mcp` (`ghostbakery-mcp.exe` on Windows), make it executable (`chmod +x`) and put it on your `PATH`.

It is a single static program with no dependencies. On macOS, if the system blocks a downloaded binary, allow it with `xattr -d com.apple.quarantine ghostbakery-mcp`.

## Configure

Create an API key in your Ghost Bakery account (*Account → API keys*, starting point **AI agent (MCP)**). The server reads:

| Variable | Meaning |
| --- | --- |
| `GHOSTBAKERY_API_KEY` | Your API key. Required. |
| `GHOSTBAKERY_API_URL` | The API address. Optional; defaults to `https://api.ghostbakery.com`. |

**Claude Code:**

```sh
claude mcp add ghostbakery --env GHOSTBAKERY_API_KEY=grd_your_key -- ghostbakery-mcp
```

**Claude Desktop, Cursor and other clients:**

```json
{
  "mcpServers": {
    "ghostbakery": {
      "command": "ghostbakery-mcp",
      "env": { "GHOSTBAKERY_API_KEY": "grd_your_key" }
    }
  }
}
```

## Your credits are protected

- Every spending tool requires a `max_cost`; the request is priced first and nothing is sent if it costs more.
- Repeated calls reuse an idempotency key, so a retry is never charged twice.
- The key's credit limit and expiry are enforced by the API, whatever the assistant does.

See the [guide](https://ghostbakery.com/docs/guides/mcp) for the full list of tools and troubleshooting.

## Support

Questions and problems: open an [issue](https://github.com/miguelpragier/ghostbakery-mcp/issues).
