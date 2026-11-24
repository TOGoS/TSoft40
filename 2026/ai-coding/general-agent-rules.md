# General Agent Rules

Instructions for LLM coding agents that are reusable across projects.
Adapted on 2026-11-24 from the `AGENTS.md` and `CONTRIBUTING.md`
files in Scratch38 sub-projects S0036 and S0039, with
project-specific material (TEF, TOGETLTSV, toolchains, branch
prefixes) removed.

Language- and framework-specific guidance should live in separate
files alongside this one.

## Interaction style

Err on the side of asking clarifying questions early, unless the
user has indicated that they want you to work independently, in
which case make your best guess and proceed.

## Writing text documents

When authoring text documents -- including Git commit messages --
follow these line-wrapping conventions:

- Keep line lengths below about 75 characters.
- Prefer to split after periods, or after coherent sentence clauses.
- "and", "or", "that", "which", etc. are good words to start a new
  line with if the line would otherwise go over about 90 characters.
- 75 is a soft limit; it's better to have a cohesive 85-character
  sentence than one split into 65- and 20-character lines.
- It's better to split as you go, risking short lines here and
  there, than to strain to follow these suggestions perfectly.

## Design principles

Aim for simplicity in API, then simplicity in implementation.
Specifically:

- Recovering from badly-formed input is a non-goal.  Known typos
  should be errors.  Other unrecognized terms should be warnings.
- Erroneous syntax (malformed headers, unclosed tags in
  Org/JSON/XML, etc.) is definitely an error.  Continue parsing
  only when doing so does not complicate the parser implementation.
- **Do** make an effort to provide clear and specific error
  messages so that problems in the source material can be easily
  identified and corrected, or parser rules can be adjusted to
  accommodate intentional variations.  Tracking line/column numbers
  through the entire process helps with this.

## Documentation

Avoid redundant documentation -- it becomes confusing when things
change.

- Do not document implementation details in README or archived plan
  documents.
- When in doubt, avoid cluttering the README.
- Notes about the current implementation, changes you have made, or
  changes you are planning may go into the project devlog (named
  `DEVLOG.md` or `AGENT-DEVLOG.md`, depending on the project) under
  a header of the form "## <date> - <title> - <your name>".

## Text file conventions

- Text files are UTF-8 with LF line endings and a final newline.
- Indent with one tab per level of nesting.  Do not indent with
  spaces.
- Blank lines inside indented code are indented too, to the same
  depth as the line before them.  Do not leave a blank line
  completely empty inside a block.
- Some projects wrap their formatter in a task or script that
  restores blank-line indentation, since plain formatters often
  strip it.  Use the project's formatting task rather than running
  the raw formatter directly.

## Validation

Run the project's build, test, and check commands after code
changes, unless the change is documentation-only.  Look in the
project's `AGENTS.md` or tooling configuration for the specific
commands.

## Worktrees

When doing work in a branch, create a worktree in `./.worktrees/` +
short branch name.

## IDs for TODO items

When adding new items to a project task queue (e.g. TODO.org),
remove the first line from `.available-ticket-ids.txt` and use that
as the ID.  This file is not versioned, and different IDs are listed
in different checkouts.  This guarantees each task gets a unique ID,
races between checkouts notwithstanding (but this should not happen
given that you will normally only write new tasks in response to a
conversation with the architect, i.e. the human user).

## Commit attribution

When an agent makes a commit entirely on its own, set author
identity per commit command (e.g. using
`git -c user.name=... -c user.email=... commit ...`), rather than
changing repository or global git config.

The identity encodes the agent and the tool that ran it:

- Name: the agent's display name, e.g. `Master Haiku`.
- Email: `togos00+<agent>.<harness>@gmail.com`, where `<agent>` is
  the display name in lowercase with spaces replaced by dots, and
  `<harness>` is `opencode`, `copilot`, or the tool's name in
  lowercase.  The `togos00+` prefix records that the owner is
  responsible for the commit.

For example, `Master Haiku <togos00+master.haiku.opencode@gmail.com>`.
The same agent running under CoPilot uses
`togos00+master.haiku.copilot@gmail.com`.

## Project notes and agent handoff

- Keep changes consistent with the architecture described in the
  project's README and the design and documentation principles
  above.
- A project's `AGENTS.md` provides a reasonable primer for agents
  who are asked to contribute.

## Session priming

A good prompt to get your sessions primed:

> I would like you to internalize the information in AGENTS.md and
> perhaps do some extra reading on the side.  I will then fork this
> conversation for new tasks in order to have the core information
> cached.

In VS Code you can then fork the session to keep that info cached
and save some processing time.  OpenCode provides similar
facilities.
