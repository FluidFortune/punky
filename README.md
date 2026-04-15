# Punky

**Lightweight GitHub-native blog editor**

A single HTML file that runs in your browser. Write posts in a clean word-processor-style editor, then publish directly to your GitHub Pages blog with one click. No server. No install. No subscription. No bloat.

---

## Philosophy

WordPress is a CMS that became an empire. Punky is a text editor that became a publish button. That's the whole thing.

You own the editor (it lives on your machine). You own the posts (they live in your repo). You own the blog (it runs on GitHub Pages for free). Nothing is rented.

---

## Quick Start

1. Download `punky.html`
2. Open it in any modern browser (Chrome or Firefox recommended)
3. Create a GitHub repo for your blog (e.g. `yourusername/blog`)
4. Generate a GitHub Personal Access Token with write access to that repo (see below)
5. Enter your token, repo, branch, and posts folder in the Punky sidebar
6. Write. Publish. Done.

Your blog will be live at `yourusername.github.io/blog` or your custom domain.

---

## Getting a Personal Access Token

1. Go to **github.com → Settings → Developer Settings → Personal Access Tokens → Fine-grained tokens**
2. Click **Generate new token**
3. Set an expiration date
4. Under **Repository access** → select **Only select repositories** → pick your blog repo
5. Under **Permissions → Repository permissions** → set **Contents** to `Read and write`
6. Click **Generate token** — copy it immediately (you only see it once)
7. Paste it into Punky's sidebar

Punky stores your token in `localStorage` — it stays on your machine and is never transmitted anywhere except the GitHub API.

---

## Blog Repo Setup

Your blog repo needs two things:

```
your-blog-repo/
├── index.html        ← the blog index page (copy from punky/blog-index.html)
├── index.json        ← auto-created by Punky on first publish
└── posts/
    ├── my-first-post.html
    └── another-post.html
```

**index.html** — copy `blog-index.html` from the Punky repo into the root of your blog repo and rename it `index.html`. Update the `MANIFEST_URL` constant at the top of the script to point to your raw `index.json` URL:

```js
const MANIFEST_URL = 'https://raw.githubusercontent.com/YOURUSERNAME/blog/main/index.json';
```

**index.json** — Punky creates and maintains this automatically. It is the manifest of all your posts — title, slug, excerpt, category, tags, created date, and modified date. The blog index page reads this file to render your posts. You never need to edit it manually.

---

## How Publishing Works

When you hit **PUBLISH TO GITHUB**:

1. Punky builds a fully-styled, self-contained HTML file for your post
2. Pushes it to `posts/your-slug.html` in your blog repo via the GitHub API
3. Reads `index.json`, upserts your post's metadata, sorts by created date, pushes the updated manifest back
4. GitHub Pages deploys the changes (usually within 1–2 minutes)

On first publish of a post, a `created` timestamp is set and never changed. On subsequent edits, a `modified` timestamp is updated. Both appear on the post and in the manifest.

---

## Editor Features

| Feature | How |
|---|---|
| Bold / Italic / Underline | Toolbar buttons |
| Headings (H1–H3) | Toolbar buttons |
| Bullet & numbered lists | Toolbar buttons |
| Pull quote / blockquote | Toolbar button |
| Code block | Toolbar button |
| Inline code | Toolbar button |
| Links | Toolbar → modal |
| Images | Toolbar → modal, URL or file picker, or **drag & drop** onto editor |
| YouTube / Vimeo embed | Toolbar → paste full URL, auto-parsed |
| Custom HTML / script | Toolbar → raw HTML input |
| Preview | PREVIEW button — renders post as it will appear |
| Export HTML | Saves standalone `.html` file to your downloads |
| Local draft save | SAVE DRAFT — persists to localStorage |
| Auto-save | Every 60 seconds to localStorage |

---

## Output Format

Each published post is a self-contained HTML file. No framework dependencies. No JavaScript required to read. Just HTML and inline CSS. It will render correctly in any browser indefinitely, with or without the rest of the blog infrastructure.

---

## Blog Index Features

- Loads posts from `index.json` manifest
- Shows 5 most recent posts by default
- Category filter buttons (Tech, Privacy, AI, Hardware, Opinion, Projects)
- "Load older entries" pagination — no page reloads
- Created date displayed prominently; edited date shown subtly if post has been updated
- Fully responsive

---

## Customization

Punky outputs posts with the Fluid Fortune terminal aesthetic by default (dark background, green/amber accents, Orbitron + Share Tech Mono fonts). To reskin for your own blog, edit the `<style>` block inside the `buildOutputHTML()` function and the `blog-index.html` CSS variables.

All colors are CSS custom properties — changing the `:root` variables is enough to retheme everything.

---

## Repo Structure

```
punky/
├── punky.html          ← the editor (this is the whole app)
├── blog-index.html     ← starter blog index page (copy to your blog repo)
├── README.md
├── LICENSE
└── ROADMAP.md
```

---

## Architecture Notes

Punky is intentionally a single HTML file. No build step. No npm. No dependencies. This is a design choice:

- **Portable** — lives on your desktop, USB drive, anywhere
- **Auditable** — the entire codebase is visible in one file
- **Durable** — will still work in 10 years
- **Trojan Horse compatible** — runs as a native desktop app inside the Trojan Horse wrapper with full filesystem access

---

## Browser Compatibility

| Browser | Status |
|---|---|
| Chrome 90+ | ✅ Full support |
| Firefox 88+ | ✅ Full support |
| Safari 14+ | ✅ Full support (drag-and-drop images may vary) |
| Edge 90+ | ✅ Full support |

---

## Privacy

- Your GitHub token is stored only in your browser's `localStorage`
- Draft content is stored only in your browser's `localStorage`
- No analytics, no telemetry, no external requests except Google Fonts (for the editor UI) and the GitHub API (only when you explicitly publish)
- Your post content never touches any server except your own GitHub repo

---

## Part of Fluid Fortune

Punky is built and maintained by [Fluid Fortune](https://fluidfortune.com) — an independent forge building tools for the era of private intelligence.

> The internet should be a resource, not a dependency.

---

## License

MIT — do what you want with it. If you build something cool on top of it, a mention is appreciated but not required.

---

## Utilities

### `punky-rss-bootstrap.html`

A one-time tool for generating `feed.xml` from an existing `index.json`. Use it when:

- You have existing posts published before Punky's RSS support was added
- You migrated from another editor and already have posts in `index.json`
- Your `feed.xml` was accidentally deleted and needs to be regenerated
- You manually edited `index.json` and want to rebuild the feed from scratch

**How to use:** Upload to any GitHub Pages repo, open via `https://`, enter your token and blog repo, hit the button. It reads your `index.json`, builds the feed from all posts, and pushes `feed.xml` to your repo. Run it once and delete it — Punky maintains the feed automatically on every publish from that point forward.

---

## Utilities

### `punky-rss-bootstrap.html`

A one-time tool for generating `feed.xml` from an existing `index.json`. Use it when:

- You have existing posts published before RSS support was added
- You migrated from another editor and already have posts in `index.json`
- Your `feed.xml` was accidentally deleted and needs to be rebuilt
- You manually edited `index.json` and want to resync the feed

**How to use:** Upload to any GitHub Pages repo, open via `https://`, enter your token and blog repo, hit the button. Punky maintains the feed automatically after that — the bootstrap is a one-time repair tool, not part of the regular workflow. You can delete it from your public repo after running it.

---

## Contributing

Issues and PRs welcome. The entire application is `punky.html` and if you choose to use it with Github Pages as a root file just change it or create a copy called index.html for your personal use. Key sections are marked with `// ──` comment headers for navigation.
