# AGENTS.md

Instructions for AI coding agents working in this repository.

## What this is

The source of https://www.polishedscreenshot.com/ (the bare domain redirects to `www`). It is one static page, "Every screenshot is a decision": twelve guidelines and three principles for deciding which screenshots to keep in software documentation.

GitHub Pages serves it straight from the `main` branch root. There is no build step and no framework. The page is plain HTML with inline CSS and JavaScript, and its one external dependency is a Google Fonts stylesheet.

## Files

| File | Purpose | Rule |
|---|---|---|
| `index.html` | The whole site | Edit this. |
| `CNAME` | Custom domain, one line: `www.polishedscreenshot.com` | Never delete or edit. Losing it detaches the domain. |
| `.nojekyll` | Stops GitHub running Jekyll over the files | Keep it. It is empty on purpose. |
| `AGENTS.md`, `DEPLOY.md` | These instructions | Update them when the workflow changes. |

## Rules

- **Everything in this repo is public.** GitHub Pages publishes every file in the root, these two included. Never commit secrets, keys, tokens or private notes.
- **Static only.** Pages has no server code, no custom headers and no server-side redirects. If a request needs any of those, say so and propose another host instead of faking it.
- **One page, relative paths.** Keep everything in `index.html`. Use lowercase filenames, because URLs are case-sensitive. If you test from a copy that links to other pages (for example a "Home" link), remove those links before the copy replaces `index.html`.
- **Keep the content visible in the HTML.** Use real `<a>` and `<button>` elements and labelled controls, and keep anything important out of metadata and out of JavaScript-only output. The page teaches this, so it should follow it.
- **Do not rewrite the author's words** (the guidelines, the principles, the byline, the credits to Brandon Harris and Danielle Fong) unless asked. Leave the text and links exactly as they are.
- **The "For your agent" callout** holds two copyable prompts. The copy buttons fill in the page's own address when clicked, so the first prompt must keep its `<span class="page-url">` after the word "Read". Keep the prompts short.
- **Pushing to `main` publishes immediately.** Only push when the owner has asked you to deploy.

## Before you push

Run the page locally and check it, then follow `DEPLOY.md`.

```bash
python3 -m http.server 8000
```

Open http://localhost:8000/ and check:

- the browser console shows no errors
- all 12 guidelines and the 3 principles are present
- the lens chips, the five-question walkthrough and the Text version toggle work
- both Copy prompt buttons work
- nothing jumps around while the page loads
