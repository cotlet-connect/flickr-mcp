# Flickr MCP server for ChatGPT and Claude

Connect ChatGPT, Claude and other AI agents to your own Flickr account through Cotlet.dev. The hosted MCP connection provides 13 operations for photos, albums, tags, privacy, comments and groups. Uploads, new albums, privacy changes, comments and group additions ask for approval by default. Cotlet.dev is independent and is not affiliated with Flickr.

## What you can ask

> List my Flickr albums with their photo counts.

> Find photos uploaded last week that have no tags.

> Read this photo before drafting a new title and tags.

> Find photos that do not belong to an album.

> Draft a comment reply and ask before publishing it.

> Ask me before changing this photo to family-only visibility.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings > Apps & Connectors > Create. Use `https://cotlet.dev/mcp/v1/connect` with OAuth.

### Claude

Settings > Connectors > Add custom connector. Use `https://cotlet.dev/mcp/v1/connect`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Flickr tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Clone this repository and run `python3 flickr_agent.py serve` with `COTLET_API_KEY` set.

Or with Docker:

```bash
docker build -t flickr-mcp .
docker run -i --rm -e COTLET_API_KEY flickr-mcp
```

The local server exposes seven browser helper tools, distinct from the 13 hosted workflow tools described on the live page.

Local tools: `flickr_guide`, `flickr_status`, `flickr_open`, `flickr_read`, `flickr_click`, `flickr_type`, `flickr_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. Run from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 flickr_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 flickr_agent.py status    # is Flickr ready in your Cotlet.dev account
python3 flickr_agent.py guide     # Flickr screens and sign-in link
python3 flickr_agent.py read      # open Flickr, read the page, close the browser
python3 flickr_agent.py tools     # list Cotlet.dev tools
python3 flickr_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Flickr users

**Does Flickr have an API or an official MCP server?**

The live Cotlet.dev page, checked 17 October 2026, describes a full Flickr API for photos, albums, comments, groups and uploads, with no official MCP server. Cotlet.dev provides an independent hosted MCP connection through your own signed-in account.

**Which account do I need?**

Use your own Flickr account, free or Pro. Sign in through your Cotlet Cloud Browser and enable Flickr for agents.

**Which changes ask for approval?**

Creating albums, uploading photos, changing privacy, commenting and adding photos to groups ask for approval by default. You can change approval settings per tool.

**Does uploading make a photo public?**

The upload operation defaults to private. Adding a non-public photo to a group makes it visible to that group, so group additions ask for approval by default.

**Does removing a photo from an album delete it?**

No. The photo remains in your photostream and other albums.

## Live integration

[Flickr tools and connection guide](https://cotlet.dev/integrations/flickr-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=flickr)

---

More: https://cotlet.dev/integrations/flickr-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=flickr

Start free: https://cotlet.dev/signup?app=flickr&utm_source=github&utm_medium=readme&utm_campaign=flickr
