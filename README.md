# xml-kern

XML parsing and decoding utilities for Kern.

The first layer is an allocation-free pull reader. It yields borrowed events and
keeps attribute iteration as a separate cursor so large documents can be scanned
without materializing a tree. Decoding helpers are available on encoded text
views and can write into caller-provided buffers or clone into a `String`.

Core APIs:

- `xml.reader(source)` creates a `Reader`.
- `reader.next()` yields borrowed `Event` values.
- `reader.next_significant()` skips whitespace text, comments, and processing instructions.
- `reader.expect_start/expect_empty/expect_end` provide small parser-combinator style building blocks.
- `reader.filter_significant()`, `reader.elements()`, and `reader.names()` provide small streaming adapters.
- `xml.drain_events(stream, sink)` drives trait-backed event sinks.
- `element.attributes()` creates an `AttributeCursor`.
- `xml.build_index(source, alloc)` builds a borrowed element index for repeated
  root/child/sibling lookup without owning or copying XML text.
- `xml.clone_owned_document(source, alloc)` builds an owned element tree with
  copied tag/name slices for use after the source buffer is no longer central.
- `name.qualified()`, `element.qualified_name()`, and `attribute.namespace_declaration()` provide borrowed namespace-aware lexical views.
- `DocumentIndex.resolved_name()`, `namespace_for()`, and `first_child_ns()`
  provide namespace-aware lookup over indexed documents.
- `EncodedText.decoded_size/write_decoded/clone_decoded` handle XML predefined entities and numeric character references.
- `reader.validate(alloc)` and `xml.validate(source, alloc)` check a single well-formed root and matching element nesting.

Benchmarking:

- `examples/bench_xml.rn` measures parse, validate, and decode paths over a
  generated Vulkan-like document or an external XML file such as `vk.xml`.
  See `bench/README.md`.

Implemented scope:

- XML declaration and processing instructions
- start, end, and empty element events
- attributes with single or double quoted values
- text, comments, CDATA, and doctype events
- BOM skipping
- entity and numeric character reference decoding
- well-formedness validation for a single root, matching element nesting,
  duplicate attributes, XML declaration placement, comments, and character data
- borrowed document indexing for repeated lookup without copying source text
- namespace-aware lexical views and URI resolution over indexed documents
- owned document cloning for callers that need to release the source buffer
- round-trip rendering for already parsed XML event streams

Release status:

`xml-kern` is intended to become a normal third-party ecosystem package. The
Kern package name remains `xml` so users can write `use xml...`; the repository
name carries the `-kern` ecosystem suffix. Keep `[package].publish = false`
until the remote repository URL and package ownership metadata are final.
