# AGENTS.md

Instructions for AI coding agents working in this repository.

## What this is

The source of https://www.polishedscreenshot.com/ (the bare domain redirects to `www`). It is one static page, "Every screenshot is a decision": five principles, a process, a Screenshot Sanity Test and sixteen guidelines in three groups (Write around it, Shoot and mark it, Replace it with text) for curating the screenshots in software documentation. It is written for the author who maintains the documentation and for their agent, not for the reader of the how-to.

GitHub Pages serves it straight from the `main` branch root. There is no build step and no framework. The page is plain HTML with inline CSS and JavaScript, and its one external dependency is a Google Fonts stylesheet.

## Files

| File | Purpose | Rule |
|---|---|---|
| `index.html` | The page | Edit this. |
| `hand-test.svg` | Animated illustration in the Screenshot Sanity Test section | Plain SVG: keep it free of scripts and external references. |
| `hand-test-static.svg` | One still frame of the same illustration, shown to people who prefer reduced motion | Regenerate it whenever `hand-test.svg` changes: remove the animations, set the card group to `opacity="1"` and the hand to `translate(60 0)`. |
| `CNAME` | Custom domain, one line: `www.polishedscreenshot.com` | Never delete or edit. Losing it detaches the domain. |
| `.nojekyll` | Stops GitHub running Jekyll over the files | Keep it. It is empty on purpose. |
| `AGENTS.md`, `DEPLOY.md` | These instructions | Update them when the workflow changes. |

## Rules

- **Everything in this repo is public.** GitHub Pages publishes every file in the root, these two included. Never commit secrets, keys, tokens or private notes.
- **Static only.** Pages has no server code, no custom headers and no server-side redirects. If a request needs any of those, say so and propose another host instead of faking it.
- **One page, relative paths.** Keep the page in `index.html`; the only other assets are the two SVGs above. Use lowercase filenames, because URLs are case-sensitive. If you test from a copy that links to other pages (for example a "Home" link), remove those links before the copy replaces `index.html`.
- **Keep the content visible in the HTML.** Use real `<a>` and `<button>` elements and labelled controls, and keep anything important out of metadata and out of JavaScript-only output. The page teaches this, so it should follow it.
- **Do not rewrite the author's words** (the principles, the guidelines, the author and audience lines, the credits to Brandon Harris and Danielle Fong) unless asked. Leave the text and links exactly as they are.
- **The "For your agent" callout** holds two copyable prompts. The copy buttons fill in the page's own address when clicked, so the first prompt must keep its `<span class="page-url">` after the word "Read". Keep the prompts short. They cite the page by section name (the Process, the Screenshot Sanity Test, the five-question check and the three guideline groups), so update them whenever one of those names changes.
- **Guidelines are cited by group and name**, for example "Shoot: crop to the minimum". Each card's label shows its group, its number within the group and that short name. If you rename, add or remove a guideline, update every citation: the prompts, the Process, the five-question check and its text version.
- **The "Note for agents" is an HTML comment** at the top of `index.html`, not visible text. Keep it there.
- **Pushing to `main` publishes immediately.** Only push when the owner has asked you to deploy. Work on a branch; pushing a branch does not change the live site (see `DEPLOY.md`).

## Before you push

Run the page locally and check it, then follow `DEPLOY.md`.

```bash
python3 -m http.server 8000
```

Open http://localhost:8000/ and check:

- the browser console shows no errors
- all 16 guidelines (5 Write, 5 Shoot, 6 Replace) and the 5 principles are present
- the five-question walkthrough and the Text version toggle work
- both Copy prompt buttons work
- the hand-test illustration loads and animates
- nothing jumps around while the page loads
