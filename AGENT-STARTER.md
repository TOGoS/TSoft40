# AGENT-STARTER

A starter kit of reusable instructions for LLM coding agents.
This repository (TOGSOFT-40, `TSoft40`) is home for those instructions;
this file is their stable entry point.

If/when the files below move or get reorganized,
update the links here and nothing else should have to change.

If you are an agent reading this: load the relevant instruction files listed below --
preferrably in parallel, not one after another --
then proceed with the task you were given.

## How to invoke it

Paste something like this into an agent session:

> Follow the instructions at /home/tog/docs/TSoft40/AGENT-STARTER.md
> (or https://github.com/TOGoS/TSoft40 if that path does not exist),
> then start a Java project in ./newproj.

Within a checkout of this repository, simply saying "follow AGENT-STARTER.md and ..." works too.

## Instruction files

Always read:

- [2026/ai-coding/general-agent-rules.md](2026/ai-coding/general-agent-rules.md)
  - cross-project conventions: interaction style, text-document formatting,
    source-code formatting, documentation and devlogs, branch names, worktrees,
	sequential ID allocation (e.g. for tickets), commit attribution.

Read the files matching the language(s) and framework(s) in play:

- [2026/ai-coding/java-style-guide.md](2026/ai-coding/java-style-guide.md)
  - Java identifier naming, member ordering, layout.

- [2026/ai-coding/typescript-style-guide.md](2026/ai-coding/typescript-style-guide.md)
  - Deno/TypeScript: type-vs-interface usage, module/test file
    conventions (including `.io.test.ts` for I/O tests), testing.

More language- and framework-specific files are planned (etc.) but not yet written;
you might check the directory itself for what exists in case we fail to update this file.

Where a language- or framework-specific file conflicts with the general rules,
the specific file wins for code it covers.

## Project scaffolding

"Start a Java project" / "start a Deno project" recipes, with exact commands,
configuration file contents, and a definition-of-done for a started project
(repository initialized, devlog stubbed, formatter task present, build and test commands passing,
and an `AGENTS.md` in the new project pointing back here),
are planned but not yet written up.
They will live alongside the style guides listed above.

Until a recipe exists, follow the general rules, use your judgment,
and defer to the architect (the human user)
on questions they've shown an interest in deciding.
