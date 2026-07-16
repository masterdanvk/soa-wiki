# SOA Wiki — Shinobi Online Adventure

Community wiki for SOA/GOA: player-facing lore & quest guides, plus a Developer Handbook for volunteer content authors. Built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/), hosted free on GitHub Pages, moderated through pull requests.

## How moderation works

- The published site only builds from `main`.
- Anyone can propose an edit (the pencil icon on any page → GitHub web editor → pull request).
- **Admins = people with merge rights.** They review and accept/reject every proposal. Branch protection (below) makes this mandatory even for collaborators.
- Every PR runs a strict build check; broken links block merging automatically.

## One-time setup (project owner)

1. **Create the repo.** Make a new GitHub repo named `soa-wiki` (public) and push this folder to it:
   ```bash
   cd soa-wiki
   git init
   git add .
   git commit -m "SOA wiki initial scaffold"
   git branch -M main
   git remote add origin https://github.com/masterdanvk/soa-wiki.git
   git push -u origin main
   ```
2. **Fill in your username.** In `mkdocs.yml`, replace `masterdanvk` in `site_url` and `repo_url` (2 places). Commit and push.
3. **Enable Pages.** The push to `main` runs the workflow, which creates a `gh-pages` branch. Then: repo **Settings → Pages → Source: Deploy from a branch → Branch: `gh-pages` / root → Save**. Your wiki is live at `https://masterdanvk.github.io/soa-wiki/` a minute later.
4. **Protect `main`** (this is what enforces "admins approve everything"): **Settings → Branches → Add branch ruleset** for `main`, enable:
   - *Require a pull request before merging* (+ required approvals: 1)
   - *Require status checks to pass* → select the `build` check
5. **Appoint admins.** **Settings → Collaborators** → add trusted people with **Write** access. With the ruleset above they can approve/merge PRs but still can't push to `main` directly unless you allow it.

## Local preview (optional)

```bash
pip install -r requirements.txt
mkdocs serve   # http://127.0.0.1:8000, live-reloads as you edit
```

## Structure

```
docs/
  index.md            Home
  lore/               Era, timeline, villages, canon policy
  quests/             Player-facing quest pages
  npcs/               NPC pages
  items/              Artifacts & quest items
  dev/                Developer Handbook (quest system internals,
                      data registry, page templates, coordination rules)
  contributing.md     How to propose edits
mkdocs.yml            Site config + navigation (add new pages to nav:)
.github/workflows/    PR build check + auto-deploy on merge
```

## Future: auto-generated quest data

The game persists quest templates to `savefiles/quest_templates.json`. Once real quests exist, a small script can diff that JSON against the [Quest Data Registry](docs/dev/quest-data-registry.md) to catch wiki drift — or generate registry entries outright. Not built yet by design.
