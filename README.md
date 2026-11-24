# TOGSOFT-40: The TOGoS Software System

Specifications for data formats, RDF concepts,
conventions for starting projects, and so on.

- README.md (this file) :: General high-level description of my approach
  and the overall trans-project architecture I have in mind.
- [AGENT-STARTER.md](./AGENT-STARTER.md) ::
  A starting point at which to point coding agents to
  get them started either on an existing project or to bootstrap a new one.

## General approach

### Common data formats

[Turtle](https://www.w3.org/TR/turtle/)?
Though ContentCouch uses RDF+XML.  But XML is a pain to deal with.

[TOGETLTSV](https://www.nuke24.net/docs/2026/TOGETLTSV202601.html) is intended
as a very simple-to-parse RDF syntax that includes 'provenance' information for each statement
(i.e. what system derived this, and from what sources, which themselves may have
derived it from something else).

[TEF](https://github.com/TOGoS/TEF) is a multipart MIME-inspired format with relatively
well defined syntax that leaves higher-level structures and semantics to the application
(as opposed to something like YAML, where Norway can become a boolean,
TEF treats everything as literal strings, and the application needs to
decide how to interpret them based on header names or other context).

[SharpFormats](https://www.nuke24.net/docs/2025/SharpFormat.html) is a family of
text-based formats sharing some characteristics, such as "`#`"-prefixed directives,
"`# `"-prefixed (note the space) comments, and a `"#format " + URI identifier of the file's format` line at the top.

[TSVFileManifestV1](https://www.nuke24.net/docs/2024/TSVFileManifestV1.html) is a lightweight
SharpFormat/TSV-based format for recording filesystem or directory snapshots
(it could be used as an alternative to CCouch's RDF+XML representation of `Directory` objects).

### Process concepts

`InputType ->{effects} OutputType` can be used as a
lowest-common-denominator to describe everything else.

- Functions with side-effects
- Pure functions
  - A special case of function-with-side-effects whose list of
    possible effects is empty
  - Single argument, single return value
  - Multiple, possibly named arguments
  - Multiple, possibly named return values
    (think continuation passing style; returning and calling are the same kind of thing)
- Stack operations
  - Which can be thought of as functions that take a stack and return a stack,
    with or without side-effects
- Stream processes
  - `ProtoProcess = () ->{Read,Write,...} Int32`
  - Those that eat bytes, emit bytes, and finally exit with some error code
    - Like an OS process, but without side-effects
  - Those that do all that but can also have other effects
    - Basically like an OS process
- Commands
  - In the spirit of shell or Tcl commands
  - `List String -> Map String String -> ProtoProcess`,
    i.e. a function that takes a list of arguments (`argv`)
    and environment variable values and returns
    some representation of a a process
    that can be executed any number of times.


### Choice of programming language

Former favorite language for getting things done: Ruby.

More recently, TypeScript, via Deno.

I think that [Unison](https://www.unison-lang.org/) is a good idea,
though I find it cumbersome to program in, but its notation for
indicating effects is useful for formalizing ideas outside of Unison.

[Factor](https://factorcode.org/) seems very close to the idea
of a Forthlike languages with high-level constructs that I keep
trying to build.

As of 2024-10-19 I am on a [Raku](https://raku.org/) (formerly 'Perl 6') kick.
If nothing else, Perl is *interesting*.

As much as I like the idea of custom languages and Unison-like guarantees,
I am, as of 2026-11, following the general rule of choosing either:
- Java [1.8] for maximum portability
- TypeScript (via Deno) for maximum development speed, and because
  Deno's explicit permissions are quite useful

I sometimes use Idris to flesh out the ontology of a system
since it has such an expressive type system, and also just
because I like the idea of dependent types and want to master thinking in those terms.


### Where to put 'scratch' projects

A good default: Make a new sub-project of Scratch38.

TScript34 has a lot of random sub-projects, too,
and is appropriate for fleshing out ideas about interpreter design,
especially for Forth or PostScript-like languages,
and especially for TScript34.2-based languages!

If something outgrows TScript34, it could get its own TOG Software Project number.

I may eventually standardize on 'everything gets a TOG Software Project number'
(i.e. `"TOGSOFT-" + some number`), possibly with dedicated sub-ranges
for numbered subprojects of SynthGen2100, Scratch38, and TScript34,
and using high numbers with lots of digits for other probably-throwaway things.


### Projbase Metadata

An RDF+XML file called ".projbase.rdf.xml" in a project directory
can hold some metadata about the project.  The root element
of that file should describe a `http://ns.nuke24.net/X-2024/Project/Projbase`,
and might include attributes like `http://ns.nuke24.net/X-2024/Project/projbaseName`,
`http://purl.org/dc/terms/title`, and `http://purl.org/dc/terms/description`

See https://www.nuke24.net/docs/ns/namespaces.tef for more terms.


### Naming Things

Generate UUID, OID, or tag URIs for new concepts
rather than coming up with a 'nice' name right away.
They can always be aliased together later.

Current recommendation is when you need a quick name,
make up a tag like `tag:nuke24.net,2026-08-23:somequickname`
(or `tag:nuke24.net,2026-08:thisonemightoutlastthemonth:` for slightly less-quick ones)
using colons as a 'path separator'.

I no longer recommend names like `http://ns.nuke24.net/X-2024/Whatever`
for experimental namespaces.
I'm not sure whether it makes sense to 'graduate' names, either,
on the basis that it's easier and simpler to just make up a tag
and stick with it forever.  Don't over-think this.

#### Software projects

Projects with a 'TOGSOFT' number can be referred to by OID,
like `urn:oid:1.3.6.1.4.1.44868.261.40`,
or by `TOGSOFT-` + number, like `TOGSOFT-40`.

Sometimes I tack the TOGSOFT project number onto a different name,
e.g. `TScript34` could alternatively be referred to as `TOGSOFT-34`.
Beware that not all numbers following project names are TOGSOFT numbers,
e.g. the '2021' in `TOGoSArduinoLibs2021` is just the year I started the project.

In some contexts I will arbitrarily decide to use a different format.
e.g. [tsoft1008](https://jsr.io/@togos/tsoft1008) (formerly 'tdxml' package within SynthGen2100).
So that may be the convention for JSR libraries that I don't have a better name for.


### Backups

Want to back up a bunch of data from friends/family/yourself?

First of all, *don't* spend too much time trying to curate what gets archived or not.
This will result in failing to actually do the backups.

*Do* use a content-addressing datastore for storing files.
This way you can back up a file any number of times without using up additional disk space.

*Do* buy some large hard drives and maybe some M-discs and a writer.

Procedure:
- Give the original media/partition a name.
  - e.g. WSITEM-3334.5 is a partition on WSITEM-3334, an SSD I bought in 2024.
  - Set the partition label to that name, or some shorthand,
    and possibly add a .partition-info.txt or something in the root to help identify it
    or provide other information 10-years-from-now you might wwant to know.
- Generate a manifest of the files using `ccouch3 walk-fs`.
  - I like to store these on the partition as `/manifests/<name>-<subdir>-<date>.manifest`,
    e.g. `WSITEM-202003.1-DCIM-20260207T1336.manifest`.
  - Feel free to only include certain files/directories, but it's also fine to go ahead
    and include everything, including earlier manifests
- Store the manifests in CCouch.
  - I use the `file-manifests` sector for this.
- Reference manifests in a central location.
  - I have a `FileManifests` Git repo, but these files can be so large that they cause problems for Git.
    _It might be better to store them in CCouch and just reference them by hash from a central Git repo._
    i.e. _I don't yet have a specific way to do this._
- Back up individual files to CCouch.
  - Don't worry about cleaning up the directory structure, first
  - If it's too much trouble, don't even worry about storing the directory structure;
    you've already recorded exactly what was on this in the manifest.
  - _todo: improve CCouch tooling to be able to, like, weak-ignore some files; reference them but don't copy._
- Write collections of files to M-disc or other ROM.
  - If not too inconvenient, include the manifest (under `/manifests/`),
    the CCouch tree or directory URI (in `/.ccouch-uris` or `/.commit-uris`, respectively),
    any CCouch directory data (under `/.ccouch/data/`).
  - Feel free to include *other* manifests and other data in those directories, too.
    An updated manifest for this partition should include all of it.
- Store the manifest for your M-discs in your central manifest repository, also.

Then, later on, you can:
- Write a tool to index all the manifests and CCouch directory data
  and help you find out what files are available where
  - _As of 2026-02-10, I have been meaning to build this tool for a few years, but have not yet done so.
    Presumably it will tie into TOGETL somehow._
- In lieu of nice tool, grep through manifests.


_TODO: how to back up Git repos or similar structures that do their own content addressing/deduplication?_


## Terminology

- project (task) :: a set of tasks to be completed
- projbase, project (codebase) :: a versioned set of files, usually stored in one or more Git repositories
- TEF :: [TOGoS's Entry Format](https://github.com/TOGoS/TEF), a relatively human and machine-readable/writable
  format for storing arbitrary information in a loosely-structured way
- TSV :: Tab-separated values; I have my own dialect that I follow, which is
  itself a line-oriented 'hash-format'


## Links

### RDF concepts

- [namespaces.tef](https://www.nuke24.net/docs/ns/namespaces.tef),
  which should probably be moved into this repository.
- [TOGVM](https://github.com/TOGoS/TOGVM-Spec)
  - RDF types and predicates for describing functional programs

TODO: Consolidate all of them!


### File formats

- [M3U Extensions](https://github.com/TOGoS/M3UExtensions)
- [TOGoS Binary Blocks](https://www.nuke24.net/docs/2012/TOGoSBinaryBlocks.html) and [TOGoS Text Blocks](https://www.nuke24.net/docs/2012/TOGoSBinaryBlocks.html#TTB)
- [TSVFileManifestV1](https://www.nuke24.net/docs/2024/TSVFileManifestV1.html)
  - one `#format` ('hash-format') format

I try to stick to a relatively consistent convention
for text-based formats, which is that `#` can be used to indicate
line comments or half-out-of-band data.  If followed by whitespace,
it's a comment.  If followed by a word, it may have special meaning.
`#!` is also treated as comment to allow for shebang lines.

### XML datatypes

i.e. lexical <-> value encodings

- https://www.nuke24.net/docs/2023/SubjectDatatype.html
- [TS34Encoded Datatype](https://www.nuke24.net/docs/2023/TS34EncodedDatatype.html)
  - Sort of a flexible meta-datatype that can be used to indicate
    a series of encodings

### Libraries

- [TOGoS Java Libraries](https://www.nuke24.net/docs/2024/TOGoSJavaLibs.html)

### URI schemes

- [active:](https://en.wikipedia.org/wiki/NetKernel#Active_URI_Scheme)
  - For representing function applications as URIs
- [urn:bitprint:](https://www.nuke24.net/docs/2015/HashURNs.html)
- [x-git-object:](https://www.nuke24.net/docs/2015/HashURNs.html)
  - For referencing blobs, directories, and commits by their Git hash
    (which is based on, but isn't exactly, SHA-1)
- [x-rdf-subject:](https://www.nuke24.net/docs/2015/HashURNs.html)
  - Prefix to indicate the concept described by the document
    identified by the rest of the URI.
- [urn:oid:](https://en.wikipedia.org/wiki/Uniform_Resource_Name)
  - Identify things by [OID](https://en.wikipedia.org/wiki/Object_identifier)

### Etc

- https://www.nuke24.net/docs/2023/Philosophy.html
