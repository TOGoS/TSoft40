# TSOFT-40: The TOGoS Software System

Specifications for data formats, RDF concepts, and such.

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

### Etc

- https://www.nuke24.net/docs/2023/Philosophy.html
