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

## Design principles

Aim for simplicity in API, then simplicity in implementation.

### Design principles for parsers

- Recovering from badly-formed input is a non-goal.  Known typos
  should be errors.  Other unrecognized terms should be warnings.
- Erroneous syntax (malformed headers, unclosed tags in Org/JSON/XML, etc.) is definitely an error.
  Continue parsing only when doing so does not complicate the parser implementation.
- **Do** make an effort to provide clear and specific error
  messages so that problems in the source material can be easily
  identified and corrected, or parser rules can be adjusted to
  accommodate intentional variations.
  Tracking line/column numbers through the entire process helps with this.

## Documentation

Avoid redundant documentation -- it becomes confusing when things change.

- Do not document implementation details in README or archived plan documents.
- When in doubt, avoid cluttering the README.
- Notes about the current implementation, changes you have made,
  or changes you are planning may go into the project devlog
  (named `DEVLOG.md` or `AGENT-DEVLOG.md`, depending on the project)
  under a header of the form "## <date> - <title> - <your name>".

## Text file conventions

- Text files are UTF-8 with LF line endings and a final newline.

### Fancy unicode characters

Avoid unnecessary use of unicode characters.
E.g.
- Prefer regular double-quotes (`"`)
- Prefer `--` to actual em-dash character

### Line-wrapping

When authoring text documents -- including Git commit messages --
follow these line-wrapping conventions:

- Aim to keep line lengths below about 75 characters;
  this is a soft limit; 90 may be a reasonable 'firm' limit;
  there is no 'hard' limit since there will sometimes be single tokens
  that will go beyond that regardless of how early in the line they are started,
  in which case the line will be long and if there is a short (less than about 20-character)
  prefix (e.g. `- Look at this URL: `), don't bother splitting that line before the long token.
- Prefer to split between coherent sentence clauses.
  After a period is generally a good idea unless the sentences are very short.
- Parenthesized clauses and those beginning with "and", "or", "that", "which", etc.
  are good candidates before and/or after which to break lines.
- 75 is a soft limit; it's better to have a cohesive 85-character
  sentence than one split into 65- and 20-character lines.
- It's better to split as you go, risking short and long lines here and there,
  than to strain to follow these suggestions perfectly.

### Formatting source code

These guidelines may be superceded depending on context.
e.g. in projects that use a language's standard formatter (e.g. `rust fmt`),
let the formatter decide.

If a project provides its own formatting script or instructions
(which ought to be mentioned in `CONTRIBUTING.md` or `AGENTS.md`),
follow them before committing.

Otherwise, the architect's preferred formatting style,
which applies to languages with C-like syntax
(Java, JavaScript, TypeScript, PHP, etc, but sometimes
Lisp-like languages or those from other families, also)
is as follows:

#### Tabs and indentation

- "[Smart tabs](https://www.nuke24.net/docs/2012/Tabs.html)" for indentation;
  i.e. one tab per level of indent, and spaces for vertical alignment
  (e.g. to put all the `=` signs in the same column) within a block.
  - Spaces may be allowed for a block indent in special cases,
    e.g. to ensure vertical alignment between parts of an outer and inner block,
    after tabs for their shared indentation.  LLM agents should not try
    to apply this style, but they should accept it as valid if they find it.
- Use one level of indent not only for curly-brace-delimited bocks,
  but also for syntactically block-like structures,
  e.g. parameter lists where groups of parameters (often of size 1) get their own line.
- In general, an 'indented block' for formatting purposes starts
  after a line ending with `{`, `[`, `(`, etc, and ends before
  a corresponding line that starts with `}`, `]`, `)`, etc.
  Multiple levels of indent may therefore be consolidated into one
  so long as this rule is followed (see 'Block indentation examples', below).
- Blank lines inside indented code are indented too, to the same depth as the line before them.
  Do not leave a blank line completely empty inside a block.
- Some projects wrap their formatter in a task or script that restores blank-line indentation,
  since plain formatters often strip it.
  Use the project's formatting task rather than running the raw formatter directly.

#### Block indentation examples

For compactness, we let the first two elements sit on
the same line as the opening brace of the third:

```pseudocode
foo( ['bar', 'baz', {
	quux,
	[
		'hi there',
		'cowboy'
	],
}])
```

Alterntively we could give those first to elements of the list their own lines,
in which case the square brackets start
a new level of indent, and the curly braces start another:

```pseudocode
foo( [
	'bar',
	'baz',
	{
		quux,
		[
			'hi there',
			'cowboy'
		],
	}
])
```

This convention leads naturally to cuddled-elses
and other lines that close one block and open another, e.g.

```pseudocode
deffunc foo(
	param1, param2,
	param3
) : some-modifier, another {
	function body or whatever
}
```

## Validation

Run the project's build, test, and check commands after code changes
unless the change is documentation-only.

Look in the project's `AGENTS.md` or tooling configuration for the specific commands.

## Branch names

Some projects share a repository with others, in which case the common convention
is to give each sub-project its own master branch with independent history
with branches named like `subtrees/` + project slug + `/` + intra-project branch name,
where 'intra-project branch name' is `master` or whatever one would normally pick
for a branch name if we were not sharing a repository.  e.g. `subtrees/s0038/refactor-my-shoes`
(`s` + four digit code is used to identify sub-projects of `Scratch38`;
other umbrella projects have their own prefixes or other conventions).

## Worktrees

If asked to do work in a worktree, create a worktree in `./.worktrees/` + intra-project branch name.

If you are not asked to work in a worktree, default to working in the current directory/branch.

If in doubt, ask, unless you've been asked to work independently, in which case default to using a worktree.

## IDs for TODO items

Some projects have an OID [sub-]namespace reserved for tracking
to-do items and/or other notes about the project itself,
in which case there may be a file named like `.available-ticket-ids.txt`,
providing one ID or number per line.

This file is normally not tracked in Git,
so that each working directory has its own pool of unique identifiers
that can be pulled from without risking re-use of a given ID.

If you need to independently 'mint' an ID and such a file exists,
remove one line from it and use that.

Sometimes there me a table of IDs / meanings / reservations,
in which case rather than removing one you should fill in
the appropriate column to mark it as taken and document its meaning.

## Commit attribution

When an agent makes a commit entirely on its own, set author
identity per commit command
(e.g. using `git -c user.name=... -c user.email=... commit ...`),
rather than changing repository or global git config.

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
  project's README and the design and documentation principles above.
- A project's `AGENTS.md` provides a reasonable primer for agents who are asked to contribute.

## Session priming

A good prompt to get your sessions primed:

> I would like you to internalize the information in AGENTS.md and
> perhaps do some extra reading on the side.  I will then fork this
> conversation for new tasks in order to have the core information
> cached.

In VS Code you can then fork the session to keep that info cached and save some processing time.
OpenCode provides similar facilities, such as the `/fork` command in the TUI.
