# msr-cli

The reference command-line tool for [MSR JSON](https://github.com/msr-standard/specification),
providing the `msr` command.

> **Status: not yet released.** This repository holds the structure and the
> rules the implementation will follow. Nothing is published yet, and there is
> no binary, package or install script. To validate a manifest today, see
> [AGENTS.md in the specification](https://github.com/msr-standard/specification/blob/main/AGENTS.md#validate).

## Role in the MSR JSON project

```
msr-standard/specification   source of truth: schemas, examples, RFCs
        ▲
        │  bundles the schema of a pinned release tag
msr-standard/msr-validator   the validation library
        ▲
        │  depends on
msr-standard/msr-cli         this repository: the `msr` command
```

Dependencies point one way only. Validation logic lives in `msr-validator`; this
tool calls it and never reimplements it, so the command line and every registry
that imports the library always agree on what is valid.

## Planned commands

The command surface is specified at <https://msr-standard.org/cli/>:

| Command | Purpose |
| --- | --- |
| `msr validate` | Validate a manifest — delegates to `msr-validator` |
| `msr generate` | Draft a manifest from a project's own metadata |
| `msr lint` | Style and completeness checks beyond schema validity |
| `msr convert` | Migrate legacy formats such as PAD XML |
| `msr sign` | Detached signatures over a manifest |

`sign` is the reason this is a separate package: it needs cryptographic
dependencies, and a registry importing only the validator should not inherit
them.

## Rules this implementation follows

- **Never publish a digest, a package or an install command before the artifact
  exists.** Release checksums are computed from the real published files.
- **Python**, published on PyPI as `msr-cli`. (The PyPI name `msr` belongs to an
  unrelated project; the installed command is still `msr`.)

## Author

MSR JSON was created by Antonio Santos. See the
[specification's AUTHORS](https://github.com/msr-standard/specification/blob/main/AUTHORS).

## License

[MIT](LICENSE).
