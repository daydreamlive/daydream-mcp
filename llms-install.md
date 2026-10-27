# Installing the Daydream MCP server

For an AI agent (Cline and others) installing this server for a user.

Daydream is a hosted, remote MCP server. Do not clone, build or run anything: there is no package and no local
process. Add one remote server to the client's MCP settings.

## The server

- Name: `daydream`
- URL: `https://gen.daydream.live/mcp`
- Transport: Streamable HTTP
- Auth: OAuth 2.1 with dynamic client registration. The client opens a Daydream sign-in page the first time a tool
  runs. No API key is needed.

## Cline

Add this to `cline_mcp_settings.json`:

```json
{
  "mcpServers": {
    "daydream": {
      "type": "streamableHttp",
      "url": "https://gen.daydream.live/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## If the client can't do OAuth

Ask the user to make an API key at https://app.daydream.live/dashboard/api-keys (it is shown once), and send it as a
header:

```json
{
  "mcpServers": {
    "daydream": {
      "type": "streamableHttp",
      "url": "https://gen.daydream.live/mcp",
      "headers": { "Authorization": "Bearer <the user's key>" }
    }
  }
}
```

Never write the key into a file in the user's repository.

## Check it works

Call `get_songs_left`. It answers with the songs made and left this month. Then try `create_song` with
`{"prompt": "a calm village theme with flute and harp", "duration": 30}` and poll `get_song` with the id it returns
until the song is done (about a minute).

## Notes

- Free accounts have 10 songs a month.
- Songs are instrumental, up to 120 seconds, each with five stems: drums, bass, guitar, keys, other.
- Docs: https://gen.daydream.live/docs#mcp
