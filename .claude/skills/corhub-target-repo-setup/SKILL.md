---
name: corhub-target-repo-setup
description: Sets up a target repository — a repository this project will work on — beside corhub. Checks that the token can read and update it, guides a token scope change if not, clones it, and optionally seats it.
when_to_use: Use when the user says /corhub-target-repo-setup, "add a repo", "add a target repo", "set up a repo to work on", "clone <repo> for this project", or names a seated repository that is not yet on this machine.
argument-hint: "[<repository URL>]"
---

Sets up one Target Repo for this Instance. Usable any time after initialization; the session also runs it by itself when a new project finishes initialization with no Target Repo set. The brief (`corhub/CLAUDE.md`) governs throughout; this skill adds no authority and relaxes no rule. In everything written for the Operator, use plain names, never step codes.

Invoking this skill is an Operator instruction to clone the repository, and so a naming: it gives edit authority over that repository for this session.

## Words to use with the Operator

Two different repositories are in play. Never let them be confused:

- **The project repository**: the `nexus-` repository, already set up. It holds this project's plans and records. Call it by its real name.
- **A target repository**: a repository this project will work on — where the actual changes are made. This skill sets one up.

When you open, say which one you mean, for example: "The project repository `nexus-billing` is set up. No target repository has been added yet: that is a repository this project will work on. Give me its URL, or say there is none for now." Do not say "code repository" or "the repository" on its own.

## Before starting

- The project must be initialized: `<root>/corhub/.nexus-git/` exists and has an `origin`. If not, say that this runs after initialization, and stop.
- Never ask for, read, write or echo a token. If one is pasted into the session, do not use it; say it is now in the session transcript and should be revoked.

## 1. Identify the target repository

- Take the URL from the argument, or ask for it in the words above.
- If the Operator says there is none for now, stop here: say that nothing was set up and that `/corhub-target-repo-setup` can be run at any time.
- Folder name: the repository name without `.git`. The Operator may give another.
- Refuse, and say why: a folder named `corhub`; a repository whose name begins with `nexus-` (that is a project repository, not a target repository).
- Target path: `<root>/<folder>`, beside `corhub/`.

## 2. Check credentials

Run `gh auth status`.

- `gh` missing: offer to install it; on yes, install it for the current user as the brief's Credentials rule describes.
- Not logged in: ask the Operator to run one command in a second terminal, outside this session, and give it as a full path ready to paste: `<root>/corhub/bin/corhub-github-auth`. It asks for the token without showing it and finishes the setup. When the Operator says "done", run `gh auth status` again yourself.

## 3. Check that it can be read

`git ls-remote <URL>`.

- Fails on authentication or "not found": the token does not cover this repository. Go to "Changing the token's scope", then repeat this step.
- Succeeds: continue. This proves reading only. A public repository can be read with no token at all, so success here says nothing about whether the project can update it.

## 4. Clone

- `<root>/<folder>` absent: `git clone <URL> <root>/<folder>`. Claude Code will ask the Operator to approve access to that folder; say so beforehand.
- Present, a git repository, and its `origin` is this URL: do not clone again; continue.
- Present and anything else: stop and report what is there. Do not move, overwrite or delete it.

Then add the line `_to_delete/` to `<root>/<folder>/.git/info/exclude` if it is not already there.

## 5. Check that it can be updated — always, public or private

This step is never skipped. Run:

```
git -C <root>/<folder> push --dry-run origin HEAD:refs/heads/corhub-access-check
```

A dry run contacts GitHub and asks for write access but sends nothing and creates nothing. It is an access check, not a push; the brief's rule against pushing a Target Repo without instruction is not engaged.

- Succeeds: the token can update this repository.
- Refused (permission denied, 403, authentication failed): the token can read this repository but cannot update it. Tell the Operator in those words, naming the repository, and say that work done here could not be sent back to GitHub. Then ask which they want:
  - **Update access:** go to "Changing the token's scope", then run this step again and report the new result.
  - **Read only for now:** record it and continue. Say plainly that pushes to this repository will fail until the token's scope is widened, and that this skill can be run again to re-check.

Do not report the repository as set up until this step has been run and its result stated.

## Changing the token's scope

Explain, naming the target repository, then wait for "done" and repeat the step that sent you here:

- **Widen the existing token.** On GitHub: Settings, Developer settings, Personal access tokens, Fine-grained tokens, open the token, Edit. Under Repository access add this repository. Under Permissions set Contents to read and write. Save. The token's value does not change, so nothing is re-entered on this machine.
- **Or replace it.** Create a new fine-grained token covering the project repository and every target repository the session should reach, Contents read and write, with an expiry. Then run `<root>/corhub/bin/corhub-github-auth` again in the second terminal and enter the new token.

A token regenerated on GitHub has a new value and must also be re-entered the same way.

## 6. Seat or not

Ask once:

- **Seat it:** standing authority on every machine that uses this project, from the next session on. Add one line, `<folder> <URL>`, under `## Seated` in `<root>/corhub/nexus/CLAUDE.md`; then `nexus-git add CLAUDE.md` and `nexus-git commit -m "air: seat <folder>"`; push per the brief's push timing.
- **This session only:** nothing is written. The repository stays editable until the session ends; a later session needs it named again.

If the repository is already listed under `## Seated`, skip the question and say it is already seated.

## 7. Report

In plain words:

- which target repository was set up, where it is on disk, and its URL;
- what the token can do with it, from steps 3 and 5: read and update, or read only;
- the authority that now applies: this session, or seated and standing from the next session;
- that nothing in it is changed or pushed until the Operator asks, and that commits will follow that repository's own conventions;
- if anything was left undone, what and why.

Do not start work in the repository as part of this skill.
