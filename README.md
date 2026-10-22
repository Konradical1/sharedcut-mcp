# SharedCut MCP

Lets Claude Code or Codex edit your [SharedCut](https://sharedcut.com) projects: cut, trim, titles, effects,
transitions, keyframes, color, captions, masks, tracking, uploads and exports. Edits show up live in
the editor, land in History, and can be undone like your own.

Color grading covers the whole Color page: wheels, log wheels, HSL curves, qualifier, gradient/circle/rectangle
windows, RGB mixer, film effects (halation, bloom, grain, vignette) and the looks library. `help color` lists every
control; `job color-measure` compares a shot with a reference picture and says which controls to move.

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

`npm run setup` asks for your SharedCut email and password, then registers the server as `sharedcut`
with Claude Code and Codex. Restart them, then ask something like:
"open my SharedCut project and show me the timeline".

- Session expired (after 30 days): `npm run login`
- Moved the folder: `npm run register`
- Update: `git pull`, then `npm install`

By hand instead of `npm run setup`:

```sh
claude mcp add --scope user sharedcut -- node /path/to/sharedcut-mcp/server.mjs
codex mcp add sharedcut -- node /path/to/sharedcut-mcp/server.mjs
```

Your session is stored in `~/sharedcut-agent/mcp.json` on your machine only.
