<p align="center">
  <img src="assets/daydream-logo-512.png" alt="Daydream" width="96" height="96">
</p>

# Daydream MCP server

Describe a song and Daydream makes it: an original instrumental, up to two minutes, in the genre, mood, tempo and key
you ask for, or a seamless loop for a game level. Every song is made as five stems (drums, bass, guitar, keys, other)
that add up to the mix exactly, so one part can be made again with the other four unchanged, and undone. Royalty-free,
for commercial games and videos.

This is a hosted server. There is nothing to install or run: point your client at the address below and sign in.

```
https://gen.daydream.live/mcp
```

- **Transport:** Streamable HTTP
- **Sign-in:** OAuth 2.1 with dynamic client registration (your client opens a Daydream sign-in page), or a Daydream
  API key sent as `Authorization: Bearer <key>`
- **Registry name:** `live.daydream.gen/music` in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=live.daydream.gen)
- **Docs:** [gen.daydream.live/docs#mcp](https://gen.daydream.live/docs#mcp)

This repository holds the listing and client settings only. The server itself is not open source, and no code or
credentials live here.

## Add it to your client

| Client | How |
|---|---|
| Claude (claude.ai, desktop, phone) | Customize → Connectors → Add custom connector, and paste `https://gen.daydream.live/mcp` |
| Claude Code | `claude mcp add --transport http daydream https://gen.daydream.live/mcp`, then `/mcp` in a session to sign in |
| ChatGPT | Add the [Daydream app](https://chatgpt.com/plugins/plugin_asdk_app_6ada33e71edc8191a47e362e40f5aba0) from the app directory |
| Cursor | [Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=daydream&config=eyJ1cmwiOiJodHRwczovL2dlbi5kYXlkcmVhbS5saXZlL21jcCJ9), or copy [`clients/cursor.json`](clients/cursor.json) into `~/.cursor/mcp.json` |
| VS Code | [Add to VS Code](vscode:mcp/install?%7B%22name%22%3A%22daydream%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fgen.daydream.live%2Fmcp%22%7D), or copy [`clients/vscode.json`](clients/vscode.json) into `.vscode/mcp.json` |
| Windsurf | Copy [`clients/windsurf.json`](clients/windsurf.json) into `~/.codeium/windsurf/mcp_config.json` |
| Goose | Add the extension in [`clients/goose.yaml`](clients/goose.yaml) to `~/.config/goose/config.yaml` |
| Cline | Ask Cline to install it from this repository; it follows [`llms-install.md`](llms-install.md) |
| Clients that only run local servers | Bridge it with `npx -y mcp-remote https://gen.daydream.live/mcp`, as in [`clients/claude-desktop.json`](clients/claude-desktop.json) |

The first time a tool runs, your client asks you to sign in to Daydream (or make a free account).

### With an API key instead

Where a client can't do OAuth, make a key on your [Daydream dashboard](https://app.daydream.live/dashboard/api-keys)
and send it as a header. In Cursor, for example:

```json
{
  "mcpServers": {
    "daydream": {
      "url": "https://gen.daydream.live/mcp",
      "headers": { "Authorization": "Bearer ${env:DAYDREAM_API_KEY}" }
    }
  }
}
```

A key can do anything on its account. Keep it in your environment, not in a file you commit.

## Tools

| Tool | What it does |
|---|---|
| `create_song` | Make a song from words. Optional: `duration` in seconds (10 to 120), `loop`, `bpm`, `key` (like `"F# minor"`), `variations` (two versions count as one song), `seed`. Returns it queued; it takes about a minute |
| `get_song` | Where a song stands, and once it's done, each version's MP3 and WAV links and its five stems as FLAC |
| `redo_stem` | Make one stem of a finished song again (new drums, say) with the other four exactly as they are. Counts as one song |
| `undo_stem` | Put back the part from before the last `redo_stem`. Costs nothing |
| `list_songs` | Your songs, newest first |
| `get_songs_left` | Songs made and left this month, and when the count starts again |

In Claude and ChatGPT the song plays in the chat with its stems. In Claude Code, Cursor or VS Code there's no player:
the agent downloads the files you ask for into your project.

### Try asking

- "Make a 60-second looping theme for the forest level and save the stems in `assets/music/forest`."
- "A tense boss fight with taiko drums and brass, 150 bpm, in D minor. Two versions."
- "Soft piano for a podcast intro, 20 seconds." Then: "Make the keys again, warmer."

## Pricing

Every account gets 10 songs a month free. The Songs plan is 500 songs a month for $4. The MCP server, the
[HTTP API](https://gen.daydream.live/docs) and the [generator](https://gen.daydream.live) share one allowance. See
[every plan](https://daydream.live/pricing), in US dollars.

An agent with no account can pay per song over HTTP 402 instead: $0.05 a song in USDC, or 10 for $0.50 in USDC or by
card through Link. See [Music for agents](https://daydream.live/for-agents).

## What you can do with the music

Every song is royalty-free for commercial games, videos, streams and apps, on every plan, Free included, and it's
yours. Daydream makes instrumental music only: words asking for vocals or lyrics are dropped, and artist names are
removed from prompts before the song is made.

## Links

- [Docs](https://gen.daydream.live/docs#mcp) · [OpenAPI](https://gen.daydream.live/openapi.json) · [llms.txt](https://gen.daydream.live/llms.txt)
- [Server card](https://gen.daydream.live/.well-known/mcp.json)
- [Privacy policy](https://daydream.live/privacy-policy) · [Terms](https://daydream.live/terms) · [Help](https://daydream.live/learn)

## Licence

The files in this repository are under the [MIT licence](LICENSE). The hosted server and the music it makes are
covered by Daydream's [terms](https://daydream.live/terms).
