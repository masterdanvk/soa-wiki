# Contributing

Anyone can propose changes to this wiki. **Nothing goes live without admin approval** — every edit becomes a pull request that a wiki admin reviews.

## The 60-second edit (no tools needed)

1. On any wiki page, click the **pencil icon** (top right). It opens the page's source on GitHub.
2. Make your change in the web editor (pages are plain Markdown).
3. Click **Commit changes** → GitHub will prompt you to create a fork and open a **pull request**.
4. Describe *what* and *why* in the PR message. Done — an admin will review it.

If the PR build check fails, an admin will tell you what to fix (usually a broken link).

## Adding a new page

1. Use the matching skeleton from [Page Templates](dev/page-templates.md).
2. Create the file in the right folder (`docs/quests/`, `docs/npcs/`, `docs/lore/`…).
3. Add the page to `nav:` in `mkdocs.yml` and to its section's index table — the strict build fails on orphaned or missing pages, which is intentional.

## Rules

- **Lore and quests must pass the [Canon Policy](lore/canon-policy.md).** Big arcs need pre-approval via an Issue before you draft.
- **Quest content PRs must update both pages**: the public quest page *and* its [Quest Data Registry](dev/quest-data-registry.md) entry.
- Keep player pages spoiler-appropriate for their section; internals belong in the Developer Handbook.
- Be excellent to each other in reviews.

## Roles

| Role | Who | Can |
|---|---|---|
| Reader | everyone | Read, open Issues/Discussions |
| Contributor | everyone with a GitHub account | Propose edits via PR |
| Admin | repo maintainers | Approve/merge PRs, rule on canon conflicts, manage the site |

Want admin? Contribute consistently and ask the project owner.
