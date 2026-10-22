# Deploying

Every push to `main` deploys. GitHub Pages rebuilds the site automatically (source: branch `main`, folder `/`). There is no CI file and nothing to configure. A build usually takes under a minute.

Run the commands below from the repository root.

## Working on a branch

Pushing any branch other than `main` does not change the live site, because Pages builds from `main` only. Do bigger changes on a branch, then merge to `main` when the owner asks you to deploy.

```bash
git switch -c my-branch      # edit, test locally, commit
git switch main && git merge my-branch && git push origin main   # only when told to deploy
```

A pushed branch is still visible in the public repository, so ask before pushing one.

## Steps

1. Edit `index.html` and test it locally (see `AGENTS.md`).

2. Review exactly what will go live. Only the files you meant to change should appear.

   ```bash
   git status
   git diff
   ```

3. Commit and push. Do this only when the owner has asked you to deploy.

   ```bash
   git add index.html   # plus any other file you changed, such as an SVG
   git commit -m "Short description of the change"
   git push origin main
   ```

4. Wait for the build to finish. Poll until it prints `built` (`building` means wait).

   ```bash
   gh api repos/Minda/polishedscreenshot.com/pages --jq .status
   ```

   If the build fails, read why:

   ```bash
   gh api repos/Minda/polishedscreenshot.com/pages/builds/latest --jq '.status, .error.message'
   ```

5. Check that the live site matches the repo exactly.

   ```bash
   curl -s "https://www.polishedscreenshot.com/?t=$RANDOM" | cmp - index.html && echo identical
   ```

   Browsers cache the page for up to 10 minutes (`cache-control: max-age=600`). Reload, or compare with `curl`, before deciding a change is missing.

6. If you touched `CNAME` or anything near it, check the redirects. `http://polishedscreenshot.com/` should answer `301` with a `location` of `https://www.polishedscreenshot.com/`.

   ```bash
   curl -sI http://polishedscreenshot.com/ | grep -i -E '^(HTTP|location)'
   ```

## Undo

```bash
git revert <commit-sha>
git push origin main
```

Do not force-push.

## Settings that are already done (do not redo)

- Pages source is branch `main`, folder `/`. The custom domain is `www.polishedscreenshot.com`, and Enforce HTTPS is on.
- DNS is at Namecheap: four A records on `@` (185.199.108.153, 185.199.109.153, 185.199.110.153, 185.199.111.153) and a CNAME on `www` pointing to `minda.github.io.` Do not edit DNS.
- The repository must stay public for free Pages.

## If something is wrong

| Symptom | Likely cause |
|---|---|
| The custom domain stops working, or the github.io address shows | `CNAME` was deleted or changed. Restore it as one line: `www.polishedscreenshot.com`. |
| 404 on a path | The filename or its case differs. URLs are case-sensitive. |
| A stylesheet or script is missing | The path is not relative, or the file was not committed. Run `git status`. |
| The old page still shows | Browser cache, up to 10 minutes. Compare with `curl` first. |
| The build shows `errored` | Read the build error with the second command in step 4. |
