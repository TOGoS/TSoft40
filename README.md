# TSOFT-40: The TOGoS Software System

Specifications for data formats, RDF concepts, and such.

## Gernal approach

(untested advice to self as of 2024-10-19):


### Common data formats

[Turtle](https://www.w3.org/TR/turtle/)?
Though ContentCouch uses RDF+XML.  But XML is a pain to deal with.


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


### Where to put 'scratch' projects

Make a new sub-project of Scratch38.  Start here.

TScript34 has a lot of random sub-projects, too.

If something outgrows TScript34, it could get its own TOG Software Project number.

This is too ambiguous.  I might standardize on 'everything is a TOG
Software Project', but using high numbers with lots of digits
for probably-throwaway things.


### Naming things

Generate UUIDs or OIDs for new concepts
rather than coming up with a 'nice' name right away.
They can always be aliased together later.

Can use names like `http://ns.nuke24.net/X-2024/Whatever` as placeholders.


## Links

### RDF concepts

- [namespaces.tef](https://www.nuke24.net/docs/ns/namespaces.tef),
  which should probably be moved into this repository.
- [TOGVM](https://github.com/TOGoS/TOGVM-Spec)
  - RDF types and predicates for describing functional programs

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
