# Email Signature Generator

A simple web tool for Coforma staff to generate a correctly formatted email signature. Fill out the form, copy the result, and paste it into your email client.

## What it does

- Generates a Coforma-branded HTML email signature from a short form
- Live preview updates as you type
- Copy button puts the signature on your clipboard as rich HTML, ready to paste into Gmail, Outlook, Apple Mail, etc.

## How to use it

1. Open the tool in your browser
2. Fill in your name, pronouns (optional), title, phone (optional), and email
3. Optionally select a Medium link to include
4. Click **Copy** and paste into your email client's signature settings

## Deployment

This is a single static HTML file — no build step, no server, no dependencies.

It is deployed via **GitHub Pages** from the `main` branch. Any commit pushed to `main` is live within a minute or two at:

**https://coforma.github.io/email-signature-generator/**

## Local development

Open `index.html` directly in a browser, or serve from the repo root to load images correctly:

```sh
npx serve .
# or
python3 -m http.server
```

> Images use a relative path (`images/`) and won't load when opened as a `file://` URL directly — use a local server if you need to test the banner image.

## Making changes

Everything lives in `index.html`. There are no external files, no build tools, and no package manager.

| What you want to change | Where to look |
|---|---|
| Form fields or layout | HTML in the `<body>` |
| Styles for the generator UI | `<style>` block in `<head>` |
| Signature output HTML/styles | `makeSignature()` in the `<script>` block |
| Available Medium links | `links` array near the top of the `<script>` block |
| Fonts | `<link>` tags in `<head>` (Google Fonts) |

### Adding a new Medium link

Find the `links` array in the `<script>` block and add an entry:

```js
const links = [
  { title: "Your link label", href: "https://example.com" },
  // ...existing entries
];
```

### Signature output styles

Styles inside `makeSignature()` are intentionally inline — email clients strip `<style>` blocks and external stylesheets. Do not move them to the `<style>` block.

## Repo structure

```
index.html        — the entire application
images/           — static assets used in the signature output
AGENTS.md         — guidance for AI coding agents working in this repo
```
