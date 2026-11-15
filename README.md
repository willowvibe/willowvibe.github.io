# willowvibe.github.io

The WillowVibe portfolio hub — one static page that links the two shipped tools,
their live demos, and the contact path.

Live: <https://willowvibe.github.io/>

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole page: hero, project cards, contact band, footer |
| `styles.css` | All styles; no preprocessor, no framework |
| `404.html` | Served by GitHub Pages for unknown paths |
| `assets/favicon.svg` | Browser tab mark |
| `assets/willowvibe-logo.png` | Wordmark used in the header (copied from ObservaKit) |
| `assets/og-card.png` | 1200×630 social card |
| `.github/workflows/pages.yml` | Deploy on every push to `main` |

There is no build step and no client-side JavaScript.

## Update a project link

All project copy and links live in `index.html` inside the `<section id="work">`
block. Each card has one media link, one heading, a problem line, a
"What it proves" line, and a link row. Change the `href` values in place.

## Update the install version

The PipelineProbe card contains `pip install pipelineprobe==0.3.1`. When a new
release ships, change that pin here. Keep the version identical to the one in
`pipelineprobe`'s README.

## Update media

Project posters are hotlinked, not stored here:

- PipelineProbe: `https://willowvibe.github.io/pipelineprobe/assets/poster.jpg`
- ObservaKit: `https://willowvibe.github.io/ObservaKit/observakit-demo-poster-v2.jpg`

If a project site moves, update both the `<img src>` and the card's demo link.
The OG card is generated once from a throwaway HTML source with headless Chrome
and committed at `assets/og-card.png`; regenerate it only when the headline changes.

## Deploy

Push to `main`. The workflow uploads the repository root and deploys it to Pages.

Pages is configured with `build_type: workflow`. On a fresh clone of this setup,
that one-time step is:

```bash
gh api -X POST repos/willowvibe/willowvibe.github.io/pages -f build_type=workflow
```

`404.html` at the root is what Pages serves for unknown paths.

## Verify a change

```bash
# every outbound link returns 200
grep -oE 'https?://[^"]+' index.html | sort -u | while read -r u; do
  printf '%s %s\n' "$(curl -s -o /dev/null -w '%{http_code}' -L --max-time 15 "$u")" "$u"
done

# HTML validity
npx --yes html-validate@11.21.1 index.html 404.html
```
