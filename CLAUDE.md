# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About This Repository

Standalone projects served at <https://projects.lincolnmullen.com>, a subdomain of my professional website, <https://lincolnmullen.com>. The Hugo site that generates lincolnmullen.com lives in a separate repository (`lmullen/lincolnmullen.com`); this repository holds only the static projects.

## Technical Details

- Each project is a directory of static files under `deploy/` (for example `deploy/us-boundaries/`), served at `https://projects.lincolnmullen.com/<directory>/`.
- `deploy/index.html` redirects the subdomain's root to lincolnmullen.com.
- The site is a Cloudflare Worker with static assets (`wrangler.jsonc`, `assets.directory` set to `./deploy`). Deploy with `npx wrangler deploy`.
- Projects use Domesday (`lmullen/domesday`), my own analytics, for tracking, not Google Analytics.

## Git and GitHub

### Issues and the project board

All work on this repository is tracked on the GitHub project at <https://github.com/users/lmullen/projects/4> (owner `lmullen`, project number `4`). The project spans all of my repositories. Every change belongs to an issue, and every pull request is linked to one. Keep the board true to what is actually happening: when you start, pause, or finish something, the issue's status should say so.

**Find the issue first.** Before starting work, look for an open issue in this repository that covers it: `gh issue list --state open`, or `gh issue list --search "<words>"` for a larger backlog. Use an existing issue even when its title is not an exact match, and don't open a duplicate. Also check the board for a draft item describing the work (`gh project item-list 4 --owner lmullen --format json --limit 500`, type `DraftIssue`); if one matches, ask the user to convert it to an issue in this repository rather than creating a second one.

**Otherwise create one.** `gh issue create --title "<title>" --body "<what and why>"`. The title names the outcome in sentence case, like a commit subject. The body gives enough context to stand on its own: what the user asked for, why, and anything that constrains the approach. Apply an existing label when one fits (`gh label list`); don't invent labels. One issue per unit of work: if the user asks for several unrelated things, open one issue each.

**Put it on the board.** `gh project item-add 4 --owner lmullen --url <issue url>`. Adding an issue that is already on the board is harmless (it returns the existing item), so always run it. Note the item id it prints; status moves need it.

**Move it as the work moves.** Status is the board's `Status` single-select field. Read its option names and ids once per session with `gh project field-list 4 --owner lmullen --format json`; don't assume the column names.

- When you start on an issue, move it to the in-progress column.
- When you open a pull request, move it to the review column, if the board has one.
- When the pull request merges, `Closes #<n>` closes the issue and the board's workflow moves it to the done column. If the board didn't, move it.
- If work stalls or is abandoned, move it back to the backlog or to-do column and comment on the issue to say why.

Change a status with:

```bash
gh project item-edit --project-id <project id> --id <item id> --field-id <Status field id> --single-select-option-id <option id>
```

The project id comes from `gh project view 4 --owner lmullen --format json --jq .id`; the field and option ids from `field-list`; and the item id from `item-add`, or from `gh project item-list 4 --owner lmullen --format json --limit 500 --jq '.items[] | select(.content.url == "<issue url>") | .id'`.

**Link the pull request.** Put `Closes #<n>` in the pull request body so GitHub links the PR to the issue and closes it on merge, and name the issue in the commit message when there is a single commit. A pull request normally closes one issue; if it genuinely closes several, write a `Closes #<n>` line for each. Don't close an issue by hand while a pull request for it is open.

**Keep the issue current.** When scope changes, when you discover a blocker, or when you leave part of the work undone, say so in a brief comment on the issue, not only in the chat, so the board tells the whole story. One comment per real update; don't narrate.

**Token scope.** `gh project` needs the `project` scope. If it reports a missing scope, run `gh auth refresh -s project` (or ask the user to run it when you can't).

**When the project API is unavailable.** Claude Code cloud sessions (claude.ai/code) block GitHub's GraphQL API, so `gh project`, `gh issue create`, and `gh pr create` fail there. Still follow the rest of this section through the REST API: create the issue with `gh api repos/lmullen/<repo>/issues -f title=... -f body=...`, open the pull request with `gh api repos/lmullen/<repo>/pulls -f title=... -f head=<branch> -f base=main -f body=...` (with `Closes #<n>` in the body), and tell the user which board moves you could not make so they can make them.
