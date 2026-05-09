# xml-kern Tasks

## P0 Correctness Core

- [x] Allocation-free pull reader over borrowed bytes.
- [x] Attribute cursor with quoted value handling.
- [x] XML declaration, processing instruction, comment, CDATA, doctype, text, start, end, and empty element events.
- [x] Entity and numeric character reference decoding.
- [x] Single-root and tag-stack validator.
- [x] Expect-style parser combinators for structured readers.
- [x] Enforce duplicate attribute errors in validation.
- [x] Reject `--` inside comments and invalid comment endings.
- [x] Reject `]]>` in ordinary character data.
- [x] Tighten XML name handling beyond ASCII-compatible fast path.
- [x] Validate XML declaration placement and declaration attributes.

## P1 API Shape

- [x] Keep stateful operations on `Reader` and `AttributeCursor` methods.
- [x] Keep query methods on shared receivers where useful.
- [x] Provide `Formatable` for public error types.
- [x] Add trait-backed event sinks for streaming transforms.
- [x] Add small composable filters/maps around `Reader`.
- [x] Add namespace-aware name and attribute views.

## P2 Testing

- [x] Smoke tests for all first-layer events.
- [x] Tests for entity decoding and UTF-8 char references.
- [x] Tests for validation failures and offsets.
- [x] Tests for expect combinators.
- [x] Add a broad malformed-input matrix.
- [x] Add generated split-position tests for EOF handling.
- [x] Add larger fixture tests.
- [ ] Add round-trip tests once writer support exists.

## P3 Performance

- [ ] Benchmark large `vk.xml`-sized documents.
- [x] Add fast byte-search helpers where they matter.
- [x] Avoid repeated scans in attribute-heavy start tags.
- [ ] Measure decode paths with and without references.

## P4 Higher Layers

- [ ] Borrowed document index for repeated lookup without full ownership.
- [ ] Owned document tree.
- [ ] XML writer/renderer.
- [ ] Namespace-aware validation and querying.
