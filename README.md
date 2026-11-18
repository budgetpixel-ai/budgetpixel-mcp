# BudgetPixel MCP Server

Generate AI images, video, music, and sound effects — and upscale them — from any
MCP-compatible client.

BudgetPixel is a remote MCP server. There is nothing to install and no API keys to manage:
connect once, sign in with Google, and your BudgetPixel plan credits and discounts apply
automatically.

**Endpoint:** `https://mcp.budgetpixel.com/mcp` (Streamable HTTP, OAuth)

## Setup

**Claude Code**

```bash
claude mcp add budgetpixel --transport http https://mcp.budgetpixel.com/mcp
```

**Cursor, Windsurf, and other config-file clients**

```json
{
  "mcpServers": {
    "budgetpixel": {
      "type": "http",
      "url": "https://mcp.budgetpixel.com/mcp"
    }
  }
}
```

**Claude, ChatGPT, and other connector UIs** — add a custom connector pointing at
`https://mcp.budgetpixel.com/mcp`.

Step-by-step guides for each client: [budgetpixel.com/mcp](https://budgetpixel.com/mcp)

## Tools

| Tool | What it does |
|---|---|
| `generate_image` | Text-to-image, editing, image-to-image, and multi-reference generation |
| `generate_video` | Text-to-video, image-to-video (start plus optional end frame), reference-to-video, and video-to-video editing |
| `generate_music` | Full songs from a prompt and/or your own lyrics, or instrumentals |
| `generate_sound_effect` | Foley, ambience, UI sounds, impacts, and transitions — optionally synced to a video's events |
| `upscale_image` | Higher resolution in seconds, or creative detail enhancement steered by a prompt |
| `upscale_video` | 1080p or 4K at 30 or 60 fps via Topaz Labs, inputs up to 20 seconds |
| `list_models` | The live model catalog with prices, role hints, and per-model input limits |
| `upload_media` | Upload a local file and get a URL to use as a generation input |
| `check_generation_status` | Poll a running job and collect the finished media |
| `get_generation_history` | Your recent generations, reusable as inputs |
| `get_credit_balance` | Remaining plan credits |
| `create_social_post` | Publish a finished generation to the BudgetPixel community feed |

## How jobs work

Generation tools return a job id rather than the finished media.

| Tool | Job id | Poll with | Typical wait |
|---|---|---|---|
| `generate_image` | numeric | `check_generation_status` | seconds |
| `generate_video` | uuid | `check_generation_status` | minutes |
| `generate_music` | numeric | `check_generation_status` with `type: "music"` | a few minutes |
| `generate_sound_effect` | numeric | `check_generation_status` with `type: "sound_effect"` | ~15s text, minutes for video |
| `upscale_image` | — | returns the result directly | seconds |
| `upscale_video` | numeric | call `upscale_video` again with just `job_id` | minutes |

Music and sound-effect ids are numeric like image ids, so the `type` argument is **required**
for those two — without it the id is read as an image job.

Local files must go through `upload_media` first: video and audio inputs on `generate_video`
accept URLs only.

## Requirements

A BudgetPixel account. Browsing the model catalog is free; generation, upscaling, uploads,
and publishing require any paid plan (Starter, Basic, Premium, Pro or Ultra) — see
[budgetpixel.com/pricing](https://budgetpixel.com/pricing).

Video is billed per second and by resolution, so confirm cost before long or high-resolution
jobs. Failed generations are never charged.

## Links

- [budgetpixel.com/mcp](https://budgetpixel.com/mcp) — setup guides
- [budgetpixel.com](https://budgetpixel.com) — the platform
- [Cursor plugin](https://github.com/budgetpixel-ai/cursor-plugin)

## Support

[support@budgetpixel.com](mailto:support@budgetpixel.com)

## More from BudgetPixel

The same BudgetPixel account also runs these studios in the browser:

- [Director](https://budgetpixel.com/director): an AI movie maker that turns an idea into a short film, step by step.
- [Music Video Studio](https://budgetpixel.com/music-video): an AI music video maker. Bring your song; the singers lip-sync to it.
- [Recast](https://budgetpixel.com/recast): recast a short video with a new cast, keeping the moves, timing and sound.
- [Clip Studio](https://studio.budgetpixel.com): an [AI shorts generator](https://studio.budgetpixel.com/shorts-studio), an [AI B-roll generator](https://studio.budgetpixel.com/broll-studio) and a [faceless video generator](https://studio.budgetpixel.com/explainer-studio) for your own footage.
- [Design Studio](https://budgetpixel.com/design): an AI designer with free, editable templates.

## License

MIT — see [LICENSE](LICENSE).
