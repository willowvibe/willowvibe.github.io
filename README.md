# willowvibe.github.io

The WillowVibe portfolio hub — one static page that links the five shipped tools,
their live demos, and the contact path. The tools are PipelineProbe, ObservaKit,
plm-bom-pipeline, data-pipeline-template and rag-docs-chatbot.

Live: <https://willowvibe.github.io/>

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole page: hero, project cards, client work, contact band, footer |
| `styles.css` | All styles; no preprocessor, no framework |
| `404.html` | Served by GitHub Pages for unknown paths |
| `robots.txt` | Allows all crawlers and points to the sitemap |
| `sitemap.xml` | Lists the hub root for search engines |
| `assets/favicon.svg` | Browser tab mark used in the header |
| `assets/og-card.png` | 1200×630 social card |
| `.github/workflows/pages.yml` | Deploy on every push to `main` |

There is no build step and no client-side JavaScript.

## Update a project link

All project copy and links live in `index.html` inside the `<section id="work">`
block. Each tool card has one media link, one heading, a problem line, a
"What it proves" line, and a link row. Change the `href` values in place.

## Client work

The "Client work" section (inside `<section id="work">`, after the five tool
cards) lists live client sites: Cosmic Saju (<https://www.cosmicsaju.com>) and
Dikshithra Atelier (<https://dikshithra.com>). Each is a compact card with a
heading, a one-line description, a "Visit site" link and a "Live" status badge.
Update the copy and `href` values in place.

## Update the install version

The PipelineProbe card contains `pip install pipelineprobe==0.3.1`. When a new
release ships, change the pin in `index.html` to match the latest
`pipelineprobe` release (currently 0.3.1).

## Update media

Project posters are hotlinked, not stored here:

- PipelineProbe: `https://willowvibe.github.io/pipelineprobe/assets/poster.jpg`
- ObservaKit: `https://willowvibe.github.io/ObservaKit/observakit-demo-poster-v2.jpg`
- plm-bom-pipeline: `https://willowvibe.github.io/plm-bom-pipeline/plm-bom-pipeline-demo-poster.jpg`
- data-pipeline-template: `https://willowvibe.github.io/data-pipeline-template/data-pipeline-template-demo-poster.jpg`
- rag-docs-chatbot: `https://willowvibe.github.io/rag-docs-chatbot/assets/rag-docs-chatbot-demo-poster-v2.jpg`

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

The repository's social preview image is a manual GitHub setting (Settings →
General → Social preview → upload `assets/og-card.png`); it cannot be set from
code.

## Verify a change

```bash
# every outbound link returns 200 (LinkedIn returns 403 to automated clients by design)
bad=""
for u in $(grep -oE 'https?://[^"]+' index.html | sort -u); do
  status=$(curl -s -o /dev/null -w '%{http_code}' -L --max-time 15 "$u")
  printf '%s %s\n' "$status" "$u"
  if [ "$status" != "200" ] && [ "$u" != "https://www.linkedin.com/company/willowvibe/" ]; then
    bad="$bad $status $u"
  fi
done
if [ -n "$bad" ]; then
  printf 'non-200 links:%s\n' "$bad" >&2
  exit 1
fi
printf 'All links 200 (LinkedIn 403 to automated clients is the accepted exception).\n'

# HTML validity
npx --yes html-validate@11.21.1 index.html 404.html
```
