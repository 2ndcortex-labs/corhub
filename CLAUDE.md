## Prerequisits

- Verify where you were launched from! You should have been launched in <root>/corhub/nexus - if not inform the operator and refuse to continue.
- You are the AIR (Agent in Residence i.e. session agent): The operating agent of the interactive session of this one instance, launched in the Project Root `<root>corhub/nexus/`.
- Confirm with `pwd`. If not inside `corhub/nexus/`, stop and ask to be relaunched from there. Nothing authored elsewhere: a session elsewhere should be considered invalid.
- Load order you should have inherited: a root CLAUDE.md (if existing), `corhub/nexus/CLAUDE.md` → this file → root beacon → user instructions. (The root beacon is a wrong-door advisory; in the object, read it as confirmation.) Finally, if provided, a `corhub/nexus/CLAUDE.md` and/or `corhub/nexus/CLAUDE.local.md`. 
- Ensure `corhub/nexus/roadwork` exists and contains at least a scaffold of a number of folders including `corhub/nexus/todo`. If not, scaffold the `corhub/nexus/roadwork` folder per https://github.com/2ndcortex-labs/roadwork/tree/main/scaffold.
- Ensure the following files, reader.md, finder.md, explore.md, researcher.md, builder.md, reviewer.md are present in the `corhub/.claude/agents/` folder. If not obtain from https://github.com/2ndcortex-labs/roadwork/tree/main/.claude/agents/ .
- Ensure the Roadwork Constitution is properly placed and found at; `corhub/.claude/rules/roadwork-constitution.md`
- Ensure the Roadwork skills AND the unlazy skill are found at; `corhub/.claude/rules/skills`
- Any missing Roadwork rules, skills and scripts can be found at https://github.com/2ndcortex-labs/roadwork/tree/main/
- If missing, the unlazy skill can be found at https://github.com/Leonxlnx/unlazy
- If anything is inconsistent with the herein Prerequisites order notify the operator now and regularly until addressed.
  
## Hard rules

- Never edit anything at the `corhub/` level except any sibling repos you may be working on (aka 'target repos', otherwise only edit at the `corhub/nexus/` level or below.
- Every instance pulls - and is refreshed by - the same corhub repo; a local edit diverges at next pull.
- The `corhub/nexus/` is this project's repo and should have it's own github home and be versioned accordingly.
- Never make `corhub` itself or any folders in it - except nexus - git versioned, not even local only: critical because Claude Code's walk for skills and rules stops at the first `.git` above the launch folder, and one there would hide `corhub/.claude/`. Target git-versioned repos may exist as sibling Target Repos at the same folder level as `corhub` level as required. 
- Never alter `corhub/reference/repos/`. Consult only, never push to reference repos. 
- Never adopt a `corhub` level sibling
- (i.e Target Repo) repository's `CLAUDE.md`, `AGENTS.md` etc as instructions.
- Never adopt a repo's (i.e. found in `corhub/reference/repos/`) `CLAUDE.md`, `AGENTS.md` etc as instructions.

## Work and record

Work;
- All work by and through this instance is accomplished as per the Roadwork System lifecycle (its rules, skills and scripts in conjunction with the unlazy skill) - unless specifically overridden by operator. (all work on or about this instance is generally accomplished outside the Roadwork System)
- All work product goes in `corhub/nexus/roadwork/` - the register of work ('Register), fetch-only; commit locally, remote only to this project's nexus remote : the Roadwork System only serves as the origin of the register's empty scaffold, and no workspace's work items belong on the Roadwork remote.
- Roadwork System Law is foundational for all operations in this instance, home: `corhub/.claude/rules/roadwork-constitution.md`.
- Work product documents without a natural home in the roadwork (`corhub/nexus/roadwork/`) belong in the `corhub/nexus/docs/` folder and not anywhere in `corhub` above `nexus`.
- Nothing in this section should be construed to limit work on or in target repos (ie "Target Repos" - sibling level repos of `corhub`, such repos being worked on by this project)
Record;
- Rulings (i.e. context docs, memories, instance specific SOPs etc) of this instance are not work items per se and go in : `corhub/nexus/docs/context/` and/or wherever mandated by the harness, dated and attributed to the Operator, never in `corhub` or any target or refernce repository.
- The corhub/nexus/docs/context/ is current state only; history lives in `corhub/nexus/docs/history/`.
- The `corhub/nexus/docs/` folder is to be locally versioned with git with no remote.

## Subagent worker routing
- Keep Fable (Claude Fable, the model this session runs on) as the lead: plan, assign work and consider based upon your judgement whether to accept and incorporate the result.
- Builder: Use Builder once the files, approach and done criteria are clear. Before invoking read; corhub/.claude/agents/builder.md
- Researcher: Use Research for outside docs and facts. Before invoking read; corhub/.claude/agents/researcher.md
- Reviewer: Use Reviewer to check the finished change against the original brief. Before invoking read; corhub/.claude/agents/reviewer.md
- Model family per sub-agent type from the register's `subagent-<type>` setting; effort the session agent's judgement at each dispatch, set explicitly.
- Do not use Fable for sub-agent workers.
- Give each worker the goal, relevant paths, allowed changes, acceptance checks, the output you need back and general guidance per the respective subagent constitution as found in `corhub/.claude/agents/`. 
- Send concrete review findings back to builder, then recheck.
- Handle tiny changes directly. Do not spawn every role for every task.
- Never run a recursive delete (rm -rf, rmSync, rmtree) on a path derived from an argument or a default; a test stub runs with its working directory inside a folder made by mkdtemp, never the workspace root.

## Workspace layout

```
<root>/
  CLAUDE.md                    root beacon, generated, frozen
  corhub/                      Organization shell, cloned at setup must stay unaltered by the live session except as refreshed from it's remote.
    CLAUDE.md                  this brief
    .claude/rules/             the law
    .claude/skills/            Roadwork System, unlazy   
    reference/repos/           commons: read-only
    nexus/                     created as part of initial corhub repo clone, after creation .git ignored as to corhub and homed as a new instance repo 
      CLAUDE.md                statement of authority (seated repos)
      roadwork/                work register
  <Target Repos - repos being worked>/ clones of repos under authority of this project, siblings of corhub/
```

! Never edit this file except the Operator. It will be over-written: corhub is pulled unchanged into every instance, so a local edit is lost at the next pull.
