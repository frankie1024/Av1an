# FlowEncode integration

This fork tracks [rust-av/Av1an](https://github.com/rust-av/Av1an). The current
integration is based on upstream v0.6.0, commit
`641ce62d70e34f438f3ee91ec50de77784a10705`.

## Maintained additions

- `--protocol-version` reports protocol 1 without starting VapourSynth.
- `--capabilities` reports supported FlowEncode metrics, encoders, interpolation
  methods and probing statistics as JSON.
- `--progress-format jsonl` emits lifecycle, input, chunk, progress, encoder-log
  and failure events on stdout. Human progress remains the default.
- `--version` provides a compact, dependency-free version probe.
- The package version identifies the maintained build as `0.6.0-flowencode.1`.

VapourSynth and VSScript use the unmodified registry dependencies from upstream:
`vapoursynth 0.5.6` and `vapoursynth-sys 0.6.0`. The old `third_party` copies,
Windows registry seeding and VSScript panic-recovery workarounds were removed.
Runtime discovery follows upstream, including `VSSCRIPT_PATH` and
`vapoursynth get-vsscript`. A caller selecting a portable runtime should expose
that runtime and its `vspipe` on PATH and may set `VSSCRIPT_PATH` explicitly.

## Updating upstream

Merge upstream master rather than resetting this fork, then review the remaining
diff against upstream for protocol and capability changes. Preserve the JSONL
schema used by FlowEncode. Update both package versions and `Cargo.lock` when
publishing a maintained build.

The FlowEncode compatibility workflow builds Windows x64 with the locked
dependencies, checks formatting, tests interpolation and capability parsing,
and validates the two protocol discovery commands. Its artifact is a candidate
binary; validate it with the intended VapourSynth runtime before publishing a
stable release or changing FlowEncode's pinned package and SHA256 digest.

VapourSynth R79 verification uses actual `.vpy` input, scene detection and
multi-chunk encoding at 8 and 10 bits, followed by decoded-frame counting.
This is a short-sample compatibility check, not a long-form, HDR/DV or GPU-plugin
qualification.
