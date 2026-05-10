# xml-kern

XML parsing, validation, indexing, and rendering utilities for Kern.

The package exposes an allocation-free pull reader over borrowed source text,
plus higher-level helpers for validation, namespace-aware lookup, owned
document cloning, and event-stream rendering. The Craft package name is `xml`;
the repository name is `xml-kern`.

## Features

- Borrowed pull parsing for XML declaration, processing instruction, start,
  end, empty element, text, comment, CDATA, and doctype events.
- Attribute cursors that avoid materializing attribute lists during streaming
  reads.
- Entity and numeric character reference decoding into caller-provided buffers
  or owned `String` values.
- Well-formedness validation for single-root documents, element nesting,
  duplicate attributes, declaration placement, comments, and character data.
- Streaming adapters and trait-backed event sinks for transforms over parsed
  event streams.
- Borrowed document indexing for repeated root, child, sibling, and namespace
  lookup without copying source text.
- Owned document cloning for callers that need to release the original source
  buffer.
- Round-trip rendering for parsed XML event streams.

## Usage

```kern
use xml.reader;

let r = reader("<config><name>kern</name></config>")..&;
r.expect_start("config").!;
r.expect_start("name").!;
```

Useful entry points:

- `xml.reader(source)` creates a `Reader`.
- `r.next()` yields borrowed `Event` values.
- `r.next_significant()` skips whitespace text, comments, and processing
  instructions.
- `r.expect_start(name)`, `expect_empty(name)`, and `expect_end(name)` provide
  small parser-combinator style building blocks.
- `r.filter_significant()`, `elements()`, and `names()` provide streaming
  adapters.
- `xml.drain_events(stream, sink)` drives trait-backed event sinks.
- `element.attributes()` creates an `AttributeCursor`.
- `xml.validate(source, alloc)` and `r.validate(alloc)` validate well-formedness.
- `xml.build_index(source, alloc)` builds a borrowed document index.
- `xml.clone_owned_document(source, alloc)` builds an owned element tree.

Namespace helpers are available through borrowed lexical views such as
`name.qualified()`, `element.qualified_name()`, and
`attribute.namespace_declaration()`. Indexed documents provide namespace-aware
lookup through `DocumentIndex.resolved_name()`, `namespace_for()`, and
`first_child_ns()`.

## Benchmarks

`examples/bench_xml.rn` measures parse, validate, and decode paths over a
generated Vulkan-like document or an external XML file such as `vk.xml`.
See `bench/README.md` for details.

## License

`xml-kern` is distributed under the MIT License. See `LICENSE`.
