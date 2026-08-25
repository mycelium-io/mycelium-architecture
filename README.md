# Mycelium architecture — data over the wire

Professional architecture diagrams (D2, not mermaid) for how SLIM and A2A move
data through the Mycelium stack. Source `.d2` files + rendered `.svg` in `diagrams/`.

## Diagrams

| File | View |
|------|------|
| `00-system-context` | Thousand-foot view: every transport at once (HTTP / SLIM-MLS / A2A-HTTPS) |
| `01-turnbased-cli`  | Pattern A: normal message send — `await` / `respond`, backend is the sole SLIM speaker |
| `02-a2a-outbound`   | Pattern B: a remote A2A agent joins the room as a member (proxied, not MLS) |
| `03-a2a-inbound`    | Pattern C: the room exposed AS an A2A agent (its Agent Card) |

## The one-line model

```
Turn-based CLI ──HTTP──► backend (moderator, holds MLS key) ──SLIM/MLS──► SLIM node ──► MLS members
                                        │
                                        └──HTTPS JSON-RPC──► remote A2A agent   (bypasses the node)
External A2A client ──HTTPS JSON-RPC──► backend room card ──► injected onto SLIM
```

Legend: **blue = HTTP**, **violet = SLIM/MLS**, **orange dashed = A2A over HTTPS**.

## Open the diagrams

The `.svg` files are just images — open them any of these ways:

```bash
open diagrams/00-system-context.svg          # default app (browser/Preview)
open -a "Google Chrome" diagrams/*.svg       # all four in Chrome tabs
open diagrams                                 # Finder, then double-click
```

## Re-render after editing a .d2

```bash
d2 --layout elk diagrams/00-system-context.d2 diagrams/00-system-context.svg
# live preview in the browser while editing:
d2 --watch diagrams/00-system-context.d2
# export PNG instead of SVG:
d2 --layout elk diagrams/00-system-context.d2 diagrams/00-system-context.png
```

D2 installed via `brew install d2` (v0.8.2). ELK layout engine for the
orthogonal, boxes-and-buses look.
