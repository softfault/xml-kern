# xml-kern Benchmarks

`examples/bench_xml.rn` measures the allocation-free reader, validation, and
decode paths. By default it builds a generated Vulkan-like XML document in
memory so benchmark runs stay self-contained. Pass a path to use a real corpus
such as `vk.xml`.

Usage:

```text
bench_xml [iterations] [mode] [path|-]
```

Modes:

- `parse`: scan events with the borrowed pull reader.
- `validate`: parse and check root/tag/duplicate-attribute well-formedness.
- `decode_refs`: repeatedly decode text containing XML references.
- `decode_plain`: repeatedly run the same decode API over reference-free text.

Examples:

```text
bench_xml
bench_xml 5000 parse
bench_xml 1000 validate /path/to/vk.xml
cat /path/to/vk.xml | bench_xml 1000 parse -
```

Each line reports operations per second and bytes per second. The `sink` value is
included so benchmark loops keep observable work.
