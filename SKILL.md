---
name: htmlify
description: Generate rich self-contained HTML for any topic or content type. Two actions: generate (write + preview locally) and publish (upload to htmlify.app and get a share URL). Use for learning topics, visual summaries, slide decks, data visualizations — anything better shown in HTML than markdown.
---

# htmlify skill

Two actions, both invoked as `/htmlify`:

- `/htmlify [topic or request]` — **generate**: create HTML, save locally, open in browser
- `/htmlify publish` — **publish**: upload last generated file, get share URL

---

## Action: generate

When the user runs `/htmlify [topic or request]`:

### Step 1: Generate the HTML

Produce a complete, self-contained HTML file. Rules:
- All CSS inline in `<style>` tags in `<head>`
- All JavaScript inline in `<script>` tags
- Zero external dependencies — no CDN links, no `<link rel="stylesheet">` to external URLs, no `src` pointing outside
- Use SVG for diagrams and illustrations (inline SVG, not `<img>` tags)
- Use CSS for color-coding sections, layout, and visual hierarchy
- Use JavaScript only for simple interactions (tabs, toggles, expand/collapse)
- For learning topics: use analogies, visual metaphors, color-coded sections, diagrams over bullet lists
- Mobile-readable: no horizontal scroll, max-width container, responsive

The HTML should be rich and visually engaging — the point is that HTML communicates better than markdown.

### Step 2: Save to local file

Derive a slug from the topic (lowercase, hyphens, no spaces). Create the directory:

```bash
mkdir -p ~/htmlify
```

Write the HTML to `~/htmlify/[topic-slug].html` using the Write tool.

### Step 3: Open in browser

```bash
open ~/htmlify/[topic-slug].html
```

### Step 4: Confirm

Tell the user: "Saved to `~/htmlify/[topic-slug].html` and opened in browser. Run `/htmlify publish` when you're ready to share it."

---

## Action: publish

When the user runs `/htmlify publish`:

### Step 1: Find the last generated file

```bash
ls -t ~/htmlify/*.html | head -1
```

Read that file's content using the Read tool.

### Step 2: Extract title

Use the `<title>` tag content from the HTML as the title. If none, use the filename (without extension).

### Step 3: POST to the API

```bash
curl -s -X POST https://htmlify.vercel.app/api/publish \
  -H "Content-Type: application/json" \
  -d "{\"title\": \"TITLE\", \"html\": $(cat ~/htmlify/FILENAME.html | python3 -c 'import json,sys; print(json.dumps(sys.stdin.read()))')}"
```

Replace `FILENAME.html` with the actual filename from Step 1.

### Step 4: Print the URL

Parse the `url` field from the JSON response and print:

```
Published: <url>
```

Then copy to clipboard:
```bash
echo "<url>" | pbcopy
```

---

## HTML quality guidelines

**Structure:** Card-based or two-column layout. Each major concept gets its own colored section.

**Analogies:** Lead with a real-world analogy before the technical explanation. E.g., "A React component is like a stamp — define it once, use it anywhere."

**Diagrams:** Inline SVG for flow diagrams, relationship maps, lifecycle diagrams. Prefer over bullet lists.

**Code samples:** `<pre><code>` blocks with background color contrast. Manual syntax highlighting with `<span>` + color.

**Interactions:** Tabs, accordions OK. Keep JS minimal — no frameworks.

**Typography:** Large headings, comfortable spacing, max-width ~800px. Consistent dark or light theme.
