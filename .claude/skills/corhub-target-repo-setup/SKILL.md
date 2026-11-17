---
name: corhub-target-repo-setup
description: Sets up a code repository beside corhub for this project — checks access, guides a token scope change if needed, clones it, and optionally seats it.
when_to_use: Use when the user says /corhub-target-repo-setup, "add a repo", "set up a code repo", "clone <repo> for this project", or names a seated repository that is not yet on this machine.
argument-hint: "[<repository URL>]"
---

Sets up one Target Repo for this Instance. Usable any time after initialization. The brief (`corhub/CLAUDE.md`) governs throughout; this skill adds no authority and relaxes no rule. In everything written for the Operator, use plain names, never step codes.

Invoking this skill is an Operator instruction to clone the repository, and so a naming: it gives edit authority over that repository for this session.

## Before starting

- The project must be initialized: `<root>/corhub/.nexus-git/` exists and has an `origin`. If not, say that this runs after initialization, and stop.
- Never ask for, read, write or echo a token. If one is pasted into the session, do not use it; say it is now in the session transcript and should be revoked.

## 1. Identify the repository

- Take the URL from the argument, or ask for it.
- Folder name: the repository name without `.git`. The Operator may give another.
- Refuse, and say why: a folder named `corhub`; a repository whose name begins with `nexus-` (that is a project repository, not a code repository).
- Target path: `<root>/<folder>`, beside `corhub/`.

## 2. Check credentials

Run `gh auth status`. If `gh` is missing or not logged in, give the setup steps and wait for "retry":

1. In a second terminal, outside this session: `gh auth login --with-token`
2. Paste the token, press Enter, then Ctrl-D (on Windows, Ctrl-Z then Enter).
3. `gh auth setup-git`

## 3. Check access to this repository

- Read access: `git ls-remote <URL>`.
- Push access, GitHub only: `gh api repos/<owner>/<name> --jq .permissions.push`.

Report what the current token can do with this repository: nothing, read only, or read and push.

If it cannot do what the Operator wants, the token's scope has to change. Explain, then wait for "retry":

- **Widen the existing token.** On GitHub: Settings, Developer settings, Personal access tokens, Fine-grained tokens, open the token, Edit. Under Repository access add this repository. Under Permissions set Contents to read and write. Save. The token's value does not change, so nothing is re-entered on this machine.
- **Or replace it.** Create a new fine-grained token covering the project's `nexus-` repository and every code repository the session should reach, Contents read and write, with an expiry. Then repeat the three steps in section 2 with the new token.

A token regenerated on GitHub has a new value and must also be re-entered with the steps in section 2.

If the Operator wants read access only, record that and continue; pushes to this repository will fail until the scope is widened.

## 4. Clone

- `<root>/<folder>` absent: `git clone <URL> <root>/<folder>`. Claude Code will ask the Operator to approve access to that folder; say so beforehand.
- Present, a git repository, and its `origin` is this URL: do not clone again; continue.
- Present and anything else: stop and report what is there. Do not move, overwrite or delete it.

Then add the line `_to_delete/` to `<root>/<folder>/.git/info/exclude` if it is not already there.

## 5. Seat or not

Ask once:

- **Seat it:** standing authority on every machine that uses this project, from the next session on. Add one line, `<folder> <URL>`, under `## Seated` in `<root>/corhub/nexus/CLAUDE.md`; then `nexus-git add CLAUDE.md` and `nexus-git commit -m "air: seat <folder>"`; push per the brief's push timing.
- **This session only:** nothing is written. The repository stays editable until the session ends; a later session needs it named again.

If the repository is already listed under `## Seated`, skip the question and say it is already seated.

## 6. Report

In plain words:

- where the repository is on disk, and its URL;
- what the token can do with it: read, or read and push;
- the authority that now applies: this session, or seated and standing from the next session;
- that nothing in it is changed or pushed until the Operator asks, and that commits will follow that repository's own conventions;
- if anything was left undone, what and why.

Do not start work in the repository as part of this skill.
