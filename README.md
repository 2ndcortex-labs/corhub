# corhub

corhub is the shared shell for Claude Code projects. Every project installation clones this repository. It supplies the operating brief, the rules, skills and agents a session loads, and the skeleton each new project starts from.

A project's own files do not live in this repository. They live in a separate repository named `nexus-<project>`, checked out inside the clone at `corhub/nexus/`.

## How an installation is laid out

```
<root>/
  CLAUDE.md            beacon: stops a session launched from the wrong folder
  corhub/              clone of this repository
    CLAUDE.md          the brief: operating rules for the session
    .claude/           rules, skills, agents, output style
    bin/               nexus-git, vendor-sync
    skeleton/          templates copied into a new project
    nexus/             the project; Claude Code is launched here
  <code repos>/        the project's code repositories, beside corhub
```

`<root>` is any folder. One `<root>` is one installation of one project.

## Set up a new project

You need the GitHub CLI, `gh`, on the machine. The session uses it to reach your repositories and never handles the token itself.

1. Create an empty GitHub repository for the project. Its name must begin with `nexus-`, for example `nexus-billing`.
2. Create a fine-grained GitHub token limited to that repository, plus any code repositories the session should push to, with Contents read and write, and an expiry.
3. Give the token to `gh`, in an ordinary terminal:
   ```
   gh auth login --with-token
   ```
   Paste the token, press Enter, then Ctrl-D (on Windows, Ctrl-Z then Enter). Then:
   ```
   gh auth setup-git
   ```
   Never paste a token into a Claude Code session. Sessions are saved as plain text.
4. Pick a `<root>` folder and clone corhub into it:
   ```
   git clone https://github.com/2ndcortex-labs/corhub <root>/corhub
   ```
5. Copy the beacon into place:
   ```
   cp <root>/corhub/skeleton/beacon/CLAUDE.md <root>/CLAUDE.md
   ```
6. Launch:
   ```
   cd <root>/corhub/nexus && claude
   ```

The session runs its startup checks on your first message, whatever that message is. On first launch it asks for the `nexus-` repository URL, sets up the project's git, copies the skeleton into `nexus/`, and pushes. Do not create anything in `nexus/` yourself beforehand.

To join a project that already exists, do the same steps and give the existing `nexus-` URL. The session checks the project out and asks you to relaunch.

## Day to day

- Always launch from `<root>/corhub/nexus/`. Add `--resume` to continue an earlier session.
- To add a code repository to the project, type `/corhub-target-repo-setup`. The session checks that your token covers it, tells you how to widen the token if not, clones it beside `corhub/`, and asks whether to keep it attached to the project permanently.
- Claude Code asks permission the first time a session reaches a folder outside `nexus/`, such as a code repository. Approve what the work needs.
- Each launch pulls corhub and runs eight startup checks on your first message, before any work. If one fails, the session says which and what it needs from you.
- Never edit files in `corhub/` on an installed machine, apart from `nexus/`. The next launch detects the change and halts. Changes to corhub are made in this repository and reach every installation by pull.

## How the project is versioned

`nexus/` has no `.git` folder. A `.git` there would stop Claude Code from finding corhub's rules, skills and agents. The project's git data is kept in `corhub/.nexus-git/` instead, and `bin/nexus-git` is plain git pointed at it:

```
corhub/bin/nexus-git status
corhub/bin/nexus-git log --oneline
```

From inside `nexus/`, a bare `git` command addresses corhub, not the project. Use `nexus-git` for the project.

## What is in this repository

| Path | What it is |
|---|---|
| `CLAUDE.md` | The brief. Defines the startup checks, what the session may edit, and how project commits and pushes happen. |
| `.claude/rules/` | The Roadwork constitution. |
| `.claude/skills/` | Nineteen Roadwork skills, the unlazy skill, and corhub's own `corhub-target-repo-setup`. `MANIFEST` lists them. |
| `.claude/agents/` | Six subagents: reader, finder, explorer, researcher, builder, reviewer. |
| `.claude/output-styles/` | The "Terse bullets" response style. Select it once per installation with `/output-style Terse bullets`. |
| `bin/nexus-git` | Git wrapper for the project repository. |
| `bin/vendor-sync` | Keeps the vendored upstream files in step with `vendor.lock`. |
| `vendor.lock` | The pinned commit of each upstream. |
| `skeleton/nexus/` | Starting content for a new project, including the Roadwork register. |
| `skeleton/beacon/CLAUDE.md` | The beacon template. |
| `nexus/` | Empty apart from `.gitkeep`. The project is checked out here on an installed machine. |
| `reference/repos/` | Read-only snapshots for reference. |

## Vendored upstreams

The rules, skills, agents and register skeleton are copied from two upstream repositories and are never edited here:

- Roadwork: https://github.com/2ndcortex-labs/roadwork
- unlazy: https://github.com/Leonxlnx/unlazy

`vendor.lock` records the commit each copy came from. To work with them, use a maintainer's clone of corhub, not an installed one:

```
bin/vendor-sync check              # is an upstream ahead of its pin?
bin/vendor-sync verify             # do the vendored files match the pins?
bin/vendor-sync update roadwork    # re-vendor at upstream head and move the pin
```

`update` commits nothing. Review the result, commit, and open a pull request. A workflow runs `verify` on every push and pull request, and opens an issue weekly when an upstream has moved past its pin.

## Changing corhub

Edit in a maintainer's clone and push to `main`. Installed machines pick the change up at their next launch. A change to `CLAUDE.md` or anything under `.claude/` makes a running session stop and ask to be relaunched, since those files are only read at session start.
