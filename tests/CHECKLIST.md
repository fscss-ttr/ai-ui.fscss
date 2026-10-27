# ai-ui.fscss quality checklist

## Compile

- [ ] `fscss tests/smoke.fscss /tmp/ai-ui-smoke.css` exits 0
- [ ] Output contains `.ai-shell`, `.ai-bubble`, `--ai-accent`, `@keyframes ai-msg-in`
- [ ] No unresolved `@use` or circular import markers

## Visual / interactive

- [ ] Open `examples/demo.html` with FSCSS runtime 1.2.6+
- [ ] User bubble right-aligned, assistant left-aligned
- [ ] Stream caret blinks on `.ai-stream`
- [ ] Thinking dots animate
- [ ] Citation chips hover
- [ ] Prompt box focus ring uses `--ai-ring`
- [ ] Sidebar hidden under 768px

## Composition

- [ ] Swap `@ai-compose-copilot()` → `@ai-compose-research()` changes accent + surfaces
- [ ] Selective import of only `ai-message`, `ai-bubble`, `ai-stream` still works

## Depth

- [ ] Coexist with `@import((*) from st-core@v2)` without selector clashes
- [ ] Coexist with `circle-progress` tokens

## Package

- [ ] `package.json` `file.usage.helpers` lists every public `@define`
- [ ] `fscss_version` ≥ 1.2.6
- [ ] README documents all five presets
