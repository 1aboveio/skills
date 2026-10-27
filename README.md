# 1aboveio Skills

Public Codex plugin catalog for the
[`@1aboveio/skills` npm bundle](https://www.npmjs.com/package/@1aboveio/skills).
Plugin code ships through npm. This repository contains only public catalog
metadata and documentation. The planned installation path will not require access
to the private implementation repository.

## Launch Status

The Codex plugin is not available yet. The published bundle checked during setup,
`@1aboveio/skills@1.0.8`, does not contain the native Codex metadata. No installable
marketplace entry is published here until a compatible release passes certification.

The first target is Codex CLI 0.164.0 and the plugin identity
`1aboveio-skills@1aboveio`. Other client versions and desktop support are not
certified.

## Release Requirements

Before adding `.agents/plugins/marketplace.json`:

1. Publish a native-compatible `@1aboveio/skills` bundle to
   `https://registry.npmjs.org/` and verify its exact version and integrity.
2. Certify that published artifact on the target client, including discovery,
   writing, engineering workers, compaction/resume, overlap selection, refresh,
   rollback, disablement, removal, and preservation of existing shared skills.
3. Review and deliver a catalog entry pinning the exact certified npm version.
   Do not use `latest`, a version range, a private Git source, or local paths.
4. Verify anonymous access to this repository and native installation from the
   delivered public catalog in a new isolated client session.

Package publication alone does not activate this catalog. A failed first launch
leaves the marketplace file absent. A failed later release retains the previous
verified pin. Catalog withdrawal does not disable or uninstall cached plugins.

Installation commands will be added when the first certified pin is delivered.
