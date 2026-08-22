# Herdr Browser (Deprecated)

> [!WARNING]
> This project is no longer maintained. Use
> [terminal-browser](https://github.com/zenbu-labs/terminal-browser) instead.

Herdr Browser was an experiment in making browser automation visible and
interactive inside a Herdr pane. Its rendering path captures Chromium frames
through the Chrome DevTools Protocol and sends encoded PNG images through
Herdr's pane graphics stream:

```text
Chromium → CDP screencast → PNG → pane.graphics.stream → Herdr → terminal
```

This requires Chromium to encode browser frames as PNGs and the display path to
decode them again.

Terminal Browser uses Electron's offscreen rendering surfaces and Herdr's newer
file-backed frame API:

```text
Electron offscreen frame
  → IOSurface (macOS) or shared-memory buffer (Linux)
  → native compositor
  → raw RGBA frame file
  → pane.graphics.stream
  → Herdr → terminal
```

Instead of sending encoded image data for every frame, it writes raw RGBA frames
to files in Herdr's frame directory and submits lightweight metadata such as:

```json
{
  "format": "rgba",
  "file": { "path": "..." },
  "sequence": 42,
  "revision": 0
}
```

Herdr acknowledges each frame before its buffer can be reused. This avoids the
PNG encode/decode cycle and gives Terminal Browser a faster foundation for a
full browser experience.

## Migrate

Install Terminal Browser on macOS or Linux:

```bash
curl -fsSL https://terminal-browser.sh/install | bash
```

See the [Terminal Browser repository](https://github.com/zenbu-labs/terminal-browser)
for current usage and documentation.
