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
- `name.qualified()`, `element.qualified_name()`, and `attribute.namespace_declaration()` provide borrowed namespace-aware lexical views.
- `EncodedText.decoded_size/write_decoded/clone_decoded` handle XML predefined entities and numeric character references.
- `reader.validate(alloc)` and `xml.validate(source, alloc)` check a single well-formed root and matching element nesting.

Benchmarking:

- `examples/bench_xml.rn` measures parse, validate, and decode paths over a
  generated Vulkan-like document or an external XML file such as `vk.xml`.
  See `bench/README.md`.

Current scope:

- XML declaration and processing instructions
- start, end, and empty element events
- attributes with single or double quoted values
- text, comments, CDATA, and doctype events
- BOM skipping
- entity and numeric character reference decoding

Planned higher layers include namespace-aware validation, indexed borrowed views,
owned document models, writer/renderer support, and larger conformance fixtures.
The current namespace API is a borrowed lexical view over names and namespace
declaration attributes; URI scope resolution belongs to the later validation and
query layers.
