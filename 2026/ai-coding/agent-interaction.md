# Advice For Interacting With AI Coding Agents

This file contains TOGoS's guidelines for delegating coding tasks to AI agents.
Target audience is TOGoS himself, or whoever is playing architect.

If an AI agent has been directed to this file,
the intent is probably to explain the role expected of it,
which could occasionally include reminding TOGoS of the guidelines and the principles behind them,
and offering some soft push-back against actions contraindicated by them.

'You' in the rest of this document refers to the architect:
whoever owns the project and has the final word on the specification.
That's probably TOGoS, the human user, but theoretically the role may be delegated.

## Architect should own the specification

It usually pays to be as precise as possible about what you want up front.
This includes coding standards, API constraints, implementation strategy
(if you have one in mind that you want to use), and anything else that you care about.
Keep the specification in files that you own and only you modify.

Agents really want to start writing code.
Explicitly go into planning mode when you're just using them to help brainstorm
(unless capabilities outside of planning mode are useful for the
brainstorming process, such as having two agents argue about it).

Let agents suggest ideas and wording for the specification,
but the final word is yours, and you should type it up yourself,
whether that's prose in README or other documents,
or machine-readable API specifications, type definitions,
or whatever defines the invariants you have in mind.
This keeps it short, keeps it as close as possible to your intent,
and reduces feedback loops that often result in agents turning molehills into mountains,
interpreting offhand comments or examples as invariants
("`tog-scad-directives` don't have to be on line 3! That was just an example!").

## Backtracking

If agents start building in the wrong direction, it may be best
to revert changes, fix the specification, and try again.
Cleaning up after a misunderstanding is often more work,
more confusing to the agent, and less fun for the human
than starting fresh from the last good state.

## Specify as much as possible

A well-documented mental model makes it easy to talk about things at a
high level without having to dig through and analyze their code.

Well-specified requirements mean the agent knows what to build.
If the requirements are changed, it can validate what's built and change accordingly.

Well-specified APIs make bridging between parts of the codebase easy;
each side undersands the contract
and doesn't need to care about implementation detauls on the other side.

(No duh, this is what I've been trying to explain to coworkers for 20 years,
but when bringing AI agents into the picture,
good de-coupling suddenly makes a qualitatively bigger impact.)

## Segregate code by owner

If there is a file with which you have strong personal connection,
it may be best to let others review and give suggestions,
but do all editing yourself,
exceptiong for small typo fixes that don't add to lines of code.

This was learned the hard way with
[the `tog-scad-directives` branch of OpenSCADDesigns](https://github.com/TOGoS/OpenSCADDesigns/commit/e8f974502ab4912ae748c00dbcb427117b988391),
where I had asked agents to make changes to the build system,
and I became *very picky* about changes made to `make.ts`,
but realized that I didn't feel as much need to micromanage libraries.

## Good specifications and fences make good neighbors

Separated ownership of different parts of the codebase ('different domains')
mean that you can worry less about agents (or other humans)
messing up the spirit of parts that you own.

Shared mental model makes it easy to talk about things.

Well-defined APIs streamline interoperation.

Keeping agents (and yourself) in their proper, well-defined place
can help make the experience plesant (sometimes even a fun adventure)
rather than a frustrating "I could have done this faster myself" slog.
