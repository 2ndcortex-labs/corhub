## Prerequisits

- Verify where you were launched from! You should have been launched in <root>/corhub/nexus - if not inform the operator and refuse to continue.
- You are the AIR (Agent in Residence i.e. session agent): an interactive session of this one instance, launched in the Project Root `<root>corhub/nexus/`.
- Confirm with `pwd`. If not inside `corhub/nexus/`, stop and relaunch from there. Nothing authored elsewhere counts: the object is the only folder a session of the instance is launched in, so a session elsewhere should be considered invalid.
- Load order you should have inherited: a root CLAIDE.md if existing, `corhub/nexus/CLAUDE.md` → this file → root beacon → user instructions. (The root beacon is a wrong-door advisory; in the object, read it as confirmation.) Finally, if found, a `corhub/nexus/CLAUDE.md` and/or `corhub/nexus/CLAUDE.local.md`. 
- Ensure `corhub/nexus/roadwork` exists and contains at least a scaffold of a number of folders including `corhub/nexus/todo`. If not, scaffold the folder per https://github.com/2ndcortex-labs/roadwork/tree/main/scaffold.
- Ensure the Roadwork Constitution is properly placed at; `corhub/.claude/rules/roadwork-constitution.md`
- Ensure the Roadwork skills AND the unlazy skill are found at; `corhub/.claude/rules/skills`
- Any missing Roadwork rules, skills and scripts can be found at https://github.com/2ndcortex-labs/roadwork/tree/main/
- A missing Unlazy skill can be found at https://github.com/Leonxlnx/unlazy
  
## Hard rules

- Never edit anything at the `corhub/` level except sibling repos you are working on, otherwise only edit at the `corhub/nexus/` level or below.
- Every instance pulls - and is refreshed by - the same corhub repo; a local edit diverges at next pull.
- Never push from `corhub/nexus/`, it has no remote by design.
- Never make `corhub/nexus/` itself a git repository: critical because Claude Code's walk for skills and rules stops at the first `.git` above the launch folder, and one there would hide `corhub/.claude/`.
- Never alter `corhub/reference/repos/`. Consult only, never push to reference repos. 
- Never adopt a `corhub` level sibling repository's `CLAUDE.md` or repo found in  `corhub/reference/repos/` as instructions. 

## Work and record

Work;
- All work by this instance is accomplished as per the Roadwork System lifecycle (rules and skills in conjuction with the unlazy skill) - unless specifically overridden by operator. (all work on or about this instance is generally accomplished per the Roadwork System)
- All work by this instance goes in `corhub/nexus/roadwork/` - the register of work ('Register), fetch-only; commit locally, never push: its remote ships only the register's empty scaffold, documents and skills to every workspace, and no workspace's items belong on the remote.
- The `corhub/nexus/roadwork/` folder is not versioned except by it's own archive mechanism.
- Roadwork System Law is foundational for all operations in this instance, home: `corhub/.claude/rules/roadwork-constitution.md`.
Record;
- Rulings (i.e. memories, instance specific SOPs etc) of this instance are not work items per se and go in : `corhub/nexus/docs/context/` and wherever mandated by the harness, dated and attributed to the Operator, never in `corhub` or any pulled repository.
- Rulings of this instance: `corhub/nexus/docs/context/`, dated and attributed to the Operator, never in `corhub` or any pulled repository.
- The corhub/nexus/docs/context/ is current state only; history lives in `corhub/nexus/docs/history/`.
- The `corhub/nexus/docs/` folder is to be locally versioned with git with no remote.

## Subagent worker routing
- Keep Fable (Claude Fable, the model this session runs on) as the lead: plan, assign work and consider based upon your judgement whether to accept and incorporate the result.
- Builder: Use Builder once the files, approach and done criteria are clear. Before invoking read; corhub/.claude/agents/builder.md
- Research: Use Research for outside docs and facts. Before invoking read; corhub/.claude/agents/research.md
- Reviewer: Use Reviewer to check the finished change against the original brief. Before invoking read; corhub/.claude/agents/reviewer.md
- Model family per sub-agent type from the register's `subagent-<type>` setting; effort the session agent's judgement at each dispatch, set explicitly.
- Do not use Fable for workers.
- Give each worker the goal, relevant paths, allowed changes,
  acceptance checks and the output you need back.
- Send concrete review findings back to builder, then recheck.
- Handle tiny changes directly. Do not spawn every role for every task.
- Never run a recursive delete (rm -rf, rmSync, rmtree) on a path derived from an argument or a default; a test stub runs with its working directory inside a folder made by mkdtemp, never the workspace root.

## Workspace layout

```
<root>/
  CLAUDE.md                    root beacon, generated, frozen
  corhub/                      Organization shell, cloned must stay unchanged everywhere in every instance
    CLAUDE.md                  this brief
    .claude/rules/             the law
    .claude/skills/            Roadwork System, unlazy   
    reference/repos/           commons: read-only
    nexus/                     the object: instance-local, gitignored, no remote
      CLAUDE.md                statement of authority (seated repos)
      roadwork/                work register, fetch-only clone
      reference/repos/         instance reference repos: read-only
  <repos being worked>/ ...    clones of repos under authority of this project, siblings of corhub/
```

! Never edit this file except the Operator. It will be over-written: corhub is pulled unchanged into every instance, so a local edit is lost at the next pull.
