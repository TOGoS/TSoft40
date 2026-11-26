# Deno / TypeScript Style

Applies to Deno-flavored TypeScript (ESM imports with `.ts`
extensions, `Deno.*` APIs, `deno test`); the language-level
guidance (types, naming, formatting) applies to TypeScript
anywhere, but the tooling and module rules assume Deno.

Identifier naming, casing (UpperCamelCase / lowerCamelCase /
UPPER_SNAKE constants), tabs, and blank-line indentation follow the
[general agent rules](general-agent-rules.md); this file covers
Deno/TypeScript-specific decisions.

## Types

- `type` when it's about the data, `interface` when it's about behavior.
  - Data-structurey shapes (fields, no methods) use `type`, which
    makes product types look like their sum-typed siblings:
    ```ts
    type ResourceRef =
    	{ kind: "content-urn", urn: string } |
    	{ kind: "remote-url", url: string } |
    	{ kind: "local-file", path: FilePath };
    ```
  - Class-like contracts -- things a caller implements to provide
    behavior -- are `interface`s: `HashAlgorithm`, `Logger`, and so on.
- No `I` prefix on interfaces, ever.
- Prefer small named aliases over bare primitives where the
  representation is ambiguous at a glance:
  `type FilePath = string` beats `string` in signatures like
  `(from: FilePath, to: FilePath)`.
- `as any` is permitted at foreign-interface seams with a comment
  when the types are *almost* right; silently lying about types
  across a real boundary is not.

## Module naming and files

- A module with a **default export** is named after the thing
  exported: `Builder.ts` exports default `Builder`; hypothetically,
  a module whose default export is `FilePath` would be `FilePath.ts`.
- A module **without a default export** -- a grab-bag of helpers --
  gets a lower-case name: `pathutil.ts`, `bufutil.ts`, `hash.ts`.
- Tests live next to the code they exercise: `<basename>.test.ts`.
- Tests that perform I/O (temp dirs, fixtures, the filesystem at
  large) go in `<basename>.io.test.ts` instead, so the plain test
  file runs under bare `deno test` with no permission flags, while
  the IO file announces the grants it needs.  Both suffixes match
  Deno's default discovery glob, so `deno test <dir>` runs
  everything without special runner support.
- Exported helpers belong in `src/lib/...`, not in build scripts.
  (Rationale: path helpers migrated out of OpenSCADDesigns'
  `make.ts` into `src/lib/ts/_util/pathutil.ts` with test coverage.)

## Deno and I/O in libraries

- Library code that is not intended to be Deno-specific
  should avoid Deno-specific calls (`Deno.*`, filesystem, env).
  Prefer:
  - dependency injection -- pass collaborators in as constructor
    or function arguments; or
  - the 'no I/O' approach -- return instructions or descriptions of work,
    and let the caller decode them and decide whether and how
    to act (`BuildRule.invoke` scripts handed to a runner that owns
    the process spawn, rather than a library that spawns itself).
- Application-level code (build scripts like `make.ts`, CLIs) is
  free to use `Deno.*` directly.

## Style details

- cuddled braces: the space, if any, goes *inside* the parens --
  `function( x : X )` and `if( x )` are both fine, `if (x)` is not.
  Rationale: in C-like syntax the parens belong to the function or
  keyword they apply to, not to their contents; contrast Lisp,
  where the space sits outside because the meaning is different.
  Same latitude for the space after a function name in its
  definition or call: `basename( path )` or `basename(path)`.
- `== null` / `== undefined` loosely: no strong rule, but when
  there's no reason to distinguish absent from null,
  `== null` catches both and that's fine.
  Use `===` when the distinction matters.
- parameter typing in camel: `opts:{...}` inline option objects
  are fine for internal APIs.

## Testing

- `assertEquals` from `std/testing/asserts.ts`; one behavior per `Deno.test`,
  names starting with the function under test:
  `Deno.test("relativePath: Windows drives", ...)`.
- Don't reimplement the logic under test inside the test.
  For path/string algebra, prefer a filesystem oracle (real temp tree,
  `realpath` adjudicates) or a property like
  `join(dirname(from), rel) === to` -- independently checkable --
  over hand-derived expected values, which are error-prone in
  exactly the interesting off-by-one cases.
- Run from the project root: `deno test src/lib/ts`
  (add `--allow-read --allow-write` when IO tests are in scope).
