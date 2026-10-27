<meta
  name="description"
  content="ai-ui.fscss — pure CSS AI interface design system for FSCSS. Prompts, streams, messages, thinking, citations, artifacts, workflows, usage dashboards."
/>
<meta
  name="keywords"
  content="ai-ui, ai-ui.fscss, FSCSS, AI UI, chat UI, copilot UI, pure CSS, design system, streaming caret, citations, artifacts"
/>

# ai-ui.fscss

> Pure-CSS presentation layer for AI SaaS UIs in the FSCSS ecosystem.
> No models. No streaming clients. No backend — tokens, layout, and chrome only.
>
> **Shell · Messages · Prompt · Stream · Thinking · Citations · Artifacts · Workflow · Usage**

**MIT Licensed** · [github.com/fscss-ttr/ai-ui.fscss](https://github.com/fscss-ttr/ai-ui.fscss)  
Requires FSCSS **v1.2.6+**

---

## Table of Contents

1. [What is ai-ui?](#1-what-is-ai-ui)
2. [How it works](#2-how-it-works)
3. [Installation](#3-installation)
   - [CDN / Runtime Mode](#cdn--runtime-mode)
   - [CLI / Compiled Mode](#cli--compiled-mode)
   - [FSCSS IDE (xfcmd)](#fscss-ide-xfcmd)
4. [Design tokens — `@ai-root`](#4-design-tokens--ai-root)
5. [Themes — `@ai-theme-*`](#5-themes--ai-theme-)
6. [Composition presets — `@ai-compose-*`](#6-composition-presets--ai-compose-)
7. [Shell & layout](#7-shell--layout)
8. [Messages](#8-messages)
9. [Prompt / input](#9-prompt--input)
10. [Stream & thinking](#10-stream--thinking)
11. [Citations](#11-citations)
12. [Artifacts](#12-artifacts)
13. [Workflow](#13-workflow)
14. [Usage & stats](#14-usage--stats)
15. [Tools / chips](#15-tools--chips)
16. [Utilities](#16-utilities)
17. [Full examples](#17-full-examples)
18. [Design token reference](#18-design-token-reference)
19. [Helper API reference](#19-helper-api-reference)
20. [Performance & scope](#20-performance--scope)
21. [Integrations](#21-integrations)

---

## 1. What is ai-ui?

**ai-ui.fscss** is a presentation system for AI product surfaces. It ships selector-parameterized `@define` helpers so you can compose:

| Surface | Typical use |
|---------|-------------|
| **Shell** | App grid: sidebar + main + prompt |
| **Messages** | User / assistant rows, bubbles, avatars |
| **Prompt** | Textarea + send control |
| **Stream** | Streaming caret on assistant text |
| **Thinking** | Processing indicator (dots) |
| **Citations** | Source chips under answers |
| **Artifacts** | Code / JSON / doc cards |
| **Workflow** | Agent / step nodes + edges |
| **Usage** | Token and metric cards |

Your application owns data, events, and model I/O. This module owns **look and structure**.

---

## 2. How it works

Same contract as other FSCSS modules (e.g. st-core):

1. **Import** registers `@define` helpers (nothing paints yet).
2. **Call** the helpers you need with a CSS selector (or use defaults).
3. **Markup** uses those class names; tokens flow through CSS variables.

```fscss
@import((*) from ai-ui)

@ai-compose-copilot()          /* tokens + theme on :root */

@ai-shell(.ai-shell)
@ai-main(.ai-main)
@ai-messages(.ai-messages)
@ai-message(.ai-message)
@ai-message-user(.ai-message-user)
@ai-message-assistant(.ai-message-assistant)
@ai-bubble(.ai-bubble)
@ai-avatar(.ai-avatar)
@ai-prompt(.ai-prompt)
@ai-prompt-box(.ai-prompt-box)
@ai-prompt-send(.ai-prompt-send)
```

**Rule of thumb:** pass the **host selector** into each helper. Children inherit design tokens from `:root` (or a scoped root).

---

## 3. Installation

### CDN / Runtime Mode

```html
<script src="https://cdn.jsdelivr.net/npm/fscss@1.2.6/runtime.min.js" defer></script>
```

```css
@import((*) from "https://cdn.jsdelivr.net/gh/fscss-ttr/ai-ui.fscss@main/ai-ui.fscss")
```

Or a local path:

```css
@import((*) from "../ai-ui.fscss")
```

Use a plain `<style>` block (not `type="text/fscss"`) so the browser applies the compiled sheet after the runtime expands it.

### Selective import

```css
@import((
  ai-root,
  ai-theme-research,
  ai-shell,
  ai-messages,
  ai-message,
  ai-message-assistant,
  ai-bubble,
  ai-citations,
  ai-citation,
  ai-stream
) from ai-ui)
```

### CLI / Compiled Mode

```bash
npm install -g fscss@latest
fscss page.fscss page.css
```

Ship only the compiled CSS in production if you prefer zero runtime preprocessor.

### FSCSS IDE (xfcmd)

```text
libon ai-ui
```

Runs `xfcmds/enable.xfcmd`: snippets, demo files under `ai-ui-demo/`, compile + preview.

```text
liboff ai-ui
```

Runs `xfcmds/disable.xfcmd` (demo folder is kept).

---

## 4. Design tokens — `@ai-root`

```css
@ai-root()                 /* :root */
@ai-root(root.my-scope)    /* scoped */
```

Writes surface, accent, text, radius, space, type, motion, and layout variables (`--ai-bg`, `--ai-accent`, `--ai-sidebar-w`, …). Call once before themes or components.

---

## 5. Themes — `@ai-theme-*`

Themes **override** token values on a root selector. Always pair with `@ai-root` (or use a compose preset).

| Helper | Character |
|--------|-----------|
| `@ai-theme-minimal()` | Near-monochrome, quiet chrome |
| `@ai-theme-copilot()` | GitHub-like blues / greens |
| `@ai-theme-research()` | Slate + cyan, citation-heavy UIs |
| `@ai-theme-support()` | Calm green support desk |
| `@ai-theme-dashboard()` | Pink accent agent / ops board |

```css
@ai-root()
@ai-theme-research()
```

---

## 6. Composition presets — `@ai-compose-*`

One call applies **root tokens + theme**:

| Preset | Theme |
|--------|--------|
| `@ai-compose-minimal()` | minimal |
| `@ai-compose-copilot()` | copilot |
| `@ai-compose-research()` | research |
| `@ai-compose-support()` | support |
| `@ai-compose-dashboard()` | dashboard |

```css
@ai-compose-copilot()
/* equivalent to @ai-root() + @ai-theme-copilot() */
```

Still call shell / message / prompt helpers for structure.

---

## 7. Shell & layout

```css
@ai-shell(.ai-shell)       /* grid: sidebar | main */
@ai-sidebar(.ai-sidebar)   /* left column; hidden < 768px */
@ai-main(.ai-main)         /* flex column: messages + prompt */
@ai-messages(.ai-messages) /* scrollable message list */
```

```html
<div class="ai-shell">
  <aside class="ai-sidebar">…</aside>
  <main class="ai-main">
    <div class="ai-messages">…</div>
    <form class="ai-prompt">…</form>
  </main>
</div>
```

---

## 8. Messages

```css
@ai-message(.ai-message)
@ai-message-user(.ai-message-user)           /* end-aligned, user bubble */
@ai-message-assistant(.ai-message-assistant) /* start-aligned, card bubble */
@ai-bubble(.ai-bubble)
@ai-avatar(.ai-avatar)
```

```html
<div class="ai-message ai-message-user">
  <div class="ai-avatar">U</div>
  <div class="ai-bubble">Track order 12389</div>
</div>
<div class="ai-message ai-message-assistant">
  <div class="ai-avatar">AI</div>
  <div class="ai-bubble">Out for delivery…</div>
</div>
```

---

## 9. Prompt / input

```css
@ai-prompt(.ai-prompt)
@ai-prompt-box(.ai-prompt-box)
@ai-prompt-send(.ai-prompt-send)
```

```html
<form class="ai-prompt" onsubmit="return false">
  <textarea class="ai-prompt-box" placeholder="Ask…" rows="1"></textarea>
  <button class="ai-prompt-send" type="submit" aria-label="Send">↑</button>
</form>
```

---

## 10. Stream & thinking

```css
@ai-stream(.ai-stream)       /* blinking caret via ::after */
@ai-thinking(.ai-thinking)   /* three .ai-dot children */
```

```html
<div class="ai-bubble ai-stream">Partial answer…</div>
<div class="ai-thinking">
  <span class="ai-dot"></span>
  <span class="ai-dot"></span>
  <span class="ai-dot"></span>
  Thinking…
</div>
```

---

## 11. Citations

```css
@ai-citations(.ai-citations)
@ai-citation(.ai-citation)
```

```html
<div class="ai-citations">
  <a class="ai-citation" href="#"><span class="ai-cite-num">1</span> source.api</a>
</div>
```

---

## 12. Artifacts

```css
@ai-artifacts(.ai-artifacts)
@ai-artifact-header(.ai-artifact-header)
@ai-artifact-body(.ai-artifact-body)
```

```html
<div class="ai-artifacts">
  <div class="ai-artifact-header">
    <span>status.json</span>
    <span>JSON</span>
  </div>
  <div class="ai-artifact-body">{ … }</div>
</div>
```

---

## 13. Workflow

```css
@ai-workflow(.ai-workflow)
@ai-node(.ai-node)
@ai-edge(.ai-edge)
```

```html
<div class="ai-workflow">
  <div class="ai-node is-active">Parse</div>
  <div class="ai-edge"></div>
  <div class="ai-node">Generate</div>
</div>
```

---

## 14. Usage & stats

```css
@ai-usage(.ai-usage)
@ai-stat(.ai-stat)
```

```html
<div class="ai-usage">
  <div class="ai-stat">
    <span class="ai-stat-label">Tokens today</span>
    <span class="ai-stat-value">48.2k</span>
    <span class="ai-stat-delta up">+12%</span>
  </div>
</div>
```

Use `.up` / `.down` on `.ai-stat-delta`.

---

## 15. Tools / chips

```css
@ai-tools(.ai-tools)
@ai-tool-chip(.ai-tool-chip)
```

```html
<div class="ai-tools">
  <span class="ai-tool-chip is-done">✓ Search</span>
  <span class="ai-tool-chip is-running">● Generate</span>
</div>
```

---

## 16. Utilities

```css
@ai-card(.ai-card)
@ai-divider(.ai-divider)
@ai-scroll-fade(.ai-scroll-fade)
```

---

## 17. Full examples

Runnable pages (module inlined for `file://`):

| File | Composition |
|------|-------------|
| `examples/index.html` | Launcher |
| `examples/demo.html` | `@ai-compose-copilot` full chat |
| `examples/research.html` | `@ai-compose-research` + citations |
| `examples/dashboard.html` | `@ai-compose-dashboard` + workflow / stats |
| `examples/support.html` | `@ai-compose-support` |

Composition-only `.fscss` sources: `examples/chat-copilot.fscss`, `research-workspace.fscss`, `dashboard-depth.fscss`.

### Minimal copilot shell

```css
@import((*) from ai-ui)

@ai-compose-copilot()
@ai-shell(.ai-shell)
@ai-main(.ai-main)
@ai-messages(.ai-messages)
@ai-message(.ai-message)
@ai-message-user(.ai-message-user)
@ai-message-assistant(.ai-message-assistant)
@ai-bubble(.ai-bubble)
@ai-avatar(.ai-avatar)
@ai-prompt(.ai-prompt)
@ai-prompt-box(.ai-prompt-box)
@ai-prompt-send(.ai-prompt-send)
```

### With stream, tools, citations, artifact

```css
@ai-stream(.ai-stream)
@ai-thinking(.ai-thinking)
@ai-tools(.ai-tools)
@ai-tool-chip(.ai-tool-chip)
@ai-citations(.ai-citations)
@ai-citation(.ai-citation)
@ai-artifacts(.ai-artifacts)
@ai-artifact-header(.ai-artifact-header)
@ai-artifact-body(.ai-artifact-body)
```

### Depth with st-core charts

`ai-ui` and **st-core@v2** share no selector collisions. Import both for AI chrome + pure-CSS charts:

```css
@import((*) from ai-ui)
@import((*) from st-core@v2)

@ai-compose-dashboard()
@st-root()
/* @st-chart-points / fill / line on a card inside .ai-main */
```

See `examples/dashboard-depth.fscss`.

---

## 18. Design token reference

| Token | Role |
|-------|------|
| `--ai-bg` / `--ai-surface` / `--ai-card` | Surfaces |
| `--ai-accent` / `--ai-accent-2` / `--ai-accent-dim` | Brand |
| `--ai-user` / `--ai-assistant` | Message roles |
| `--ai-success` / `--ai-warning` / `--ai-danger` | Semantic |
| `--ai-text` / `--ai-text-muted` / `--ai-placeholder` | Type color |
| `--ai-border` / `--ai-ring` | Chrome |
| `--ai-radius-*` | Radius scale |
| `--ai-space-*` | Spacing scale |
| `--ai-font` / `--ai-font-mono` / `--ai-fs-*` | Typography |
| `--ai-sidebar-w` / `--ai-prompt-h` / `--ai-msg-max` | Layout |

Override per theme or locally:

```css
.ai-shell {
  --ai-accent: #a78bfa;
  --ai-sidebar-w: 240px;
}
```

---

## 19. Helper API reference

| Helper | Default selector / args |
|--------|-------------------------|
| `ai-root` | `root:root` |
| `ai-theme-minimal` … `ai-theme-dashboard` | `root:root` |
| `ai-compose-minimal` … `ai-compose-dashboard` | `root:root` |
| `ai-shell` | `.ai-shell` |
| `ai-sidebar` | `.ai-sidebar` |
| `ai-main` | `.ai-main` |
| `ai-messages` | `.ai-messages` |
| `ai-message` | `.ai-message` |
| `ai-message-user` | `.ai-message-user` |
| `ai-message-assistant` | `.ai-message-assistant` |
| `ai-bubble` | `.ai-bubble` |
| `ai-avatar` | `.ai-avatar` |
| `ai-prompt` | `.ai-prompt` |
| `ai-prompt-box` | `.ai-prompt-box` |
| `ai-prompt-send` | `.ai-prompt-send` |
| `ai-stream` | `.ai-stream` |
| `ai-thinking` | `.ai-thinking` |
| `ai-citations` | `.ai-citations` |
| `ai-citation` | `.ai-citation` |
| `ai-artifacts` | `.ai-artifacts` |
| `ai-artifact-header` | `.ai-artifact-header` |
| `ai-artifact-body` | `.ai-artifact-body` |
| `ai-workflow` | `.ai-workflow` |
| `ai-node` | `.ai-node` |
| `ai-edge` | `.ai-edge` |
| `ai-usage` | `.ai-usage` |
| `ai-stat` | `.ai-stat` |
| `ai-tools` | `.ai-tools` |
| `ai-tool-chip` | `.ai-tool-chip` |
| `ai-card` | `.ai-card` |
| `ai-divider` | `.ai-divider` |
| `ai-scroll-fade` | `.ai-scroll-fade` |

Smoke list: `tests/smoke.fscss`.

---

## 20. Performance & scope

- **Presentation only** — no JS chart or chat runtime required for static demos.
- Compiled helper output is small CSS; toggle themes by swapping variables.
- Prefer CLI compile for production if you want zero FSCSS runtime on the page.

---

## 21. Integrations

| Stack | Notes |
|-------|--------|
| **FSCSS runtime** | `fscss@1.2.6` CDN + `@import` |
| **FSCSS CLI** | `fscss in.fscss out.css` |
| **st-core@v2** | Charts beside AI chrome (separate prefixes) |
| **IDE** | `libon ai-ui` / `liboff ai-ui` via `xfcmds/` |

---

## License

MIT · **fscss-ttr**
