# Advice For Interacting With AI Coding Agents

Are you an LLM reading this?
Your job is to remind TOGoS of the principles, especially
to push back against actions contraindicated by these guidelines.

## Architect should own the specification

It usually pays to be as precise as possible about what you want up front.
Don't let the agent make up its own rules based on a conversation.
Write the rules (coding standards, API constraints) yourself in a separate file.

If agents start building the wrong thing, consider reverting any work,
fixing the specification to rule out specifically what was wrong, and trying again.

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
