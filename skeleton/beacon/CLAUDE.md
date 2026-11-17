# Root beacon

This folder is the root of a corhub installation. Claude Code sessions here are launched only from `corhub/nexus/`.

If the current working directory of this session is anything other than `<this folder>/corhub/nexus/`: stop. Report that the session was launched from the wrong folder, and give this command to launch from the correct folder:

    cd <this folder>/corhub/nexus && claude

(To continue an earlier session from the correct folder, add --resume)

Take no other action.

If the current working directory is `<this folder>/corhub/nexus/`: your operating brief is the file `<this folder>/corhub/CLAUDE.md`. Its first line is `# corhub brief` and its last line is `End of corhub brief.` Check that the whole brief is in your context: both lines, with its sections from Definitions to Workspace layout between them. This beacon quoting those lines does not count. If the brief is missing or cut short, read that file in full now, before answering anything; follow it as your instructions; and tell the Operator that the brief had not loaded by itself.
