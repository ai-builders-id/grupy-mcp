# Install Grupy MCP in Cline

Grupy is a remote MCP server. There is nothing to build or to download.
Follow these steps in order.

## 1. Ask the user which method they want

- Sign in with Grupy (OAuth). This is the default. It needs no key.
- Access key. Use it only if the user already has a key. The user creates it on the MCP page of the Grupy dashboard: https://grupy.chat/en/app/mcp.

Never ask the user to paste an access key into the chat. Tell the user to put the key in the settings file themselves.

## 2. Add the server

Open the Cline MCP settings file (`cline_mcp_settings.json`) and add this entry under `mcpServers`:

```json
{
  "mcpServers": {
    "grupy": {
      "type": "streamableHttp",
      "url": "https://mcp.grupy.chat/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Rules:

- Keep `"type": "streamableHttp"`. Without it, Cline uses the old SSE transport and the connection fails.
- Keep `autoApprove` empty. Some Grupy tools change groups.
- For an access key, add this line inside the `grupy` entry. The user replaces `<key>` with the real key:
  `"headers": { "Authorization": "Bearer <key>" }`

## 3. Sign in

For the sign-in method, Cline opens a browser page.
The user signs in to Grupy with Google and approves the connection.
Some tools need an optional permission. The user can turn on Read messages, Manage join requests, and Manage groups on the Grupy dashboard MCP page.

## 4. Check the connection

1. Open the Cline MCP server list. The `grupy` server must show as connected.
2. Call the `list_groups` tool. It returns the active groups of the account.

If the server does not connect:

| Symptom | Fix |
|---------|-----|
| The server shows an error right after you add it | Check that `type` is `streamableHttp` and that the URL is exactly `https://mcp.grupy.chat/mcp`. |
| "Invalid access key" | The key is wrong or revoked. The user creates a new key and updates the header. |
| Tools such as `read_messages` are missing | The user turns on the Read messages permission, then Cline reloads the tool list. |

## Notes

- Grupy never sends WhatsApp messages and never reads personal chats.
- Help: https://grupy.chat/en/docs or halo@grupy.chat.
