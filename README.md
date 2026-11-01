# SharedCut MCP

An [MCP](https://modelcontextprotocol.io) server that lets Claude Code or Codex edit your [SharedCut](https://sharedcut.com) projects: cut, trim, titles, effects, transitions, keyframes, color, captions, masks, tracking, uploads and exports, plus the Plan workspace (script, shot list, mood board). Edits show up live in the editor, land in History, and can be undone like your own.

Color grading covers the whole Color page: wheels, log wheels, HSL curves, qualifier, gradient/circle/rectangle windows, RGB mixer, film effects (halation, bloom, grain, vignette) and the looks library. `help color` lists every control. `job color-measure` compares a shot with a reference picture and tells you which controls to move.

SharedCut itself, a collaborative video editor, is at [sharedcut-editor](https://github.com/Konradical1/sharedcut-editor). This repository holds the standalone build of its `mcp/` folder.

## How it works

The server doesn't call special back-end functions. It runs the SharedCut editor's own state store without a screen and saves through the same HTTP API the browser uses. So an edit made by an agent is checked by the server, merged with what collaborators are doing, shown to them live, and can be undone, exactly like an edit made by a person. Ids are printed as short handles (`@3fa9c1d2`) and results are kept compact to save tokens.

`server.mjs` and `setup.mjs` are generated bundles. The source is in [`mcp/`](https://github.com/Konradical1/sharedcut-editor/tree/main/mcp) of sharedcut-editor, and `npm run mcp:bundle` there produces this repository.

## Tools

| Tool | What it does |
|---|---|
| `projects` | List projects and open one |
| `timeline` | Tracks and clips, one line each (filter by time or track) |
| `media` | The project's media and bins |
| `get` | Full JSON of one clip, node, track, media item or effect |
| `act` | Run editor actions in order, saved as one batch (all or nothing) |
| `job` | Server work: uploads, captions, masks, depth, stabilize, noise reduction, tracking, color assistant, exports, Resolve, sounds, fonts, Plan import/export |
| `frame` | A small picture of the timeline or a source frame, so the agent can check its work |
| `help` | Catalogs: actions, effects, transitions, titles, shapes, color, properties, jobs |
| `api` | Escape hatch to any `/api` endpoint |

## You need

- A SharedCut account on sharedcut.com, with the projects you want shared with you
- [Node.js](https://nodejs.org) 22.18 or newer
- Claude Code and/or Codex

## Install

```sh
git clone https://github.com/Konradical1/sharedcut-mcp.git
cd sharedcut-mcp
npm install
npm run setup
```

`npm run setup` asks for your SharedCut email and password, then registers the server as `sharedcut` with Claude Code and Codex. Restart them, then ask something like "open my SharedCut project and show me the timeline".

- Session expired (after 30 days): `npm run login`
- Moved the folder: `npm run register`
- Update: `git pull`, then `npm install`

To register it by hand instead of with `npm run setup`:

```sh
claude mcp add --scope user sharedcut -- node /path/to/sharedcut-mcp/server.mjs
codex mcp add sharedcut -- node /path/to/sharedcut-mcp/server.mjs
```

Your session is stored only on your machine, in `~/sharedcut-agent/mcp.json`. Set `FRAME_LINE_URL` (and `FRAME_LINE_COOKIE`) to point it at a different SharedCut server.
