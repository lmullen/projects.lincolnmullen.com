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

The rules for working with Git and GitHub are the same in all my repositories and are not repeated here: the `lmullen-agent` identity, an issue for every change, the project board, branches and worktrees, pull requests, merging, and cleanup after a merge. In a Docker sandbox they come from the lmullen-agent kit, whose context file you must read before any Git or GitHub operation. In a Claude Code cloud session they are in `~/.claude/CLAUDE.md`. Their source is [`lmullen-agent-context.md`](https://github.com/lmullen/dotfiles/blob/main/sandboxes/kits/lmullen-agent/lmullen-agent-context.md) in `lmullen/dotfiles`; change a rule there, not here.

This section holds only the facts about this repository.

### Project board

- The board is <https://github.com/users/lmullen/projects/4> (owner `lmullen`, project number `4`). It spans all of my repositories.
- Its `Status` columns are `Triage`, `TODO`, `In progress`, `To review`, and `Done`.
- Issues opened in this repository are not added to the board automatically. Add each one: `gh project item-add 4 --owner lmullen --url <issue url>`.
