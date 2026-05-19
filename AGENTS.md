# AGENTS.md

## Repo Overview

Single-file static web app. No build system, no package manager, no dependencies.

- `index.html` — the entire application (HTML + CSS + JS inline)
- `images/` — static assets referenced by absolute path in the signature HTML

## Architecture

- All logic is in a self-invoking IIFE inside `index.html`
- Signature HTML is generated client-side via `makeSignature()` and injected into `#signature`
- The email field accepts a full address; `getEmail()` strips the domain — `@coforma.io` is hardcoded in the output
- Phone formatting is applied on `input` event via `formatPhone()`: strips non-digits (`/\D/g`), takes first 10, formats as `###.###.####`
- The "Medium Link" `<select>` is populated from a hardcoded `links` array via `DocumentFragment` — add new options there
- The banner image path is relative (`images/2024BPTW_WinnerEmailSignature.png`) — must be served from the repo root, not opened as a `file://` URL if images are needed
- Pronouns and phone are optional: both use `getValue(id, { strict: true })` / conditional rendering — blank fields produce no output
- `getValue(id, { strict: true })` returns `false` when a field is empty instead of falling back to placeholder text

## Development

Open `index.html` directly in a browser, or serve from repo root:

```sh
npx serve .
# or
python3 -m http.server
```

No build step, no install, no CI.

## Editing Conventions

- Keep CSS in the `<style>` block, JS in the `<script>` block — no external files
- Signature output HTML uses inline styles (required for email client compatibility — do not move styles to a stylesheet)
- Google Fonts (`Public Sans`, `Work Sans`, `Signika`) are loaded via `<link>` in `<head>` — keep them there
- The Copy button uses `ClipboardItem` with `text/html` MIME type so email clients receive rich HTML on paste, not a raw string
- `aria-live="polite"` is on both `#signature` and `#copy-status` — do not remove; they announce updates to screen readers
- The `<select>` first option (`value=""`, not disabled) is the default "no link" state — selecting it omits the link line from the signature entirely
