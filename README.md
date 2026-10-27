# 1aboveio Skills

Public installation guide and Codex plugin catalog for the
[`@1aboveio/skills` npm bundle](https://www.npmjs.com/package/@1aboveio/skills).
Skills ship through the public npm registry. This repository contains only catalog
metadata and documentation, not skill source files.

| Harness | Public installation method | Status |
|---|---|---|
| Pi | Pi's npm package installer | Available now |
| Codex CLI | Native plugin from this catalog | Certified for CLI 0.164.0; available when the 1.0.10 catalog entry reaches `main` |

Neither path requires access to the implementation repository. Do not use this
catalog as a Skills CLI source: it contains no skills to install.

## Launch Status

`@1aboveio/skills@1.0.10` is certified for Codex CLI **0.164.0** with plugin
identity `1aboveio-skills@1aboveio`. Other CLI versions and desktop support are
not certified.

Once the matching catalog entry is added to `main`, this catalog selects exactly
`@1aboveio/skills@1.0.10` from `https://registry.npmjs.org/`. The installation
commands below use the public Git catalog for discovery and npm for the payload.
Package publication and certification alone do not make the catalog installable.

## Install in Pi

Install and configure [Pi](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/README.md)
first, with Node.js and npm available. Pi reads the bundle's `pi` manifest; this is
not a generic npm plugin installation command for other harnesses.

```sh
pi install npm:@1aboveio/skills
```

Pi initially loads all first-party packages. To inspect the available packages and
choose which ones Pi loads:

```sh
npx @1aboveio/skills list
npx @1aboveio/skills                 # interactive package selection
# Or choose packages by ID:
npx @1aboveio/skills engineering cicd
```

The selector adds `core` when a chosen package needs it. To preview a selection
without writing settings or downloading third-party sources:

```sh
npx @1aboveio/skills engineering cicd --dry-run
```

Use `pi config` to enable or disable individual skills instead of whole packages.
Install either the bundle or the individual `@1aboveio/<id>-skills` packages, not
both, because they provide the same skill names.

Personal installation and selection use `~/.pi/agent/settings.json`. For a
project-scoped installation, use matching scopes on both commands:

```sh
pi install npm:@1aboveio/skills --local
npx @1aboveio/skills engineering cicd --local
```

Project settings use `.pi/settings.json` and require Pi's project trust approval.
The selector is a Pi configuration tool, not a Codex or Claude Code installer.

### Update and Remove in Pi

For the unpinned installation above, update only this package with:

```sh
pi update npm:@1aboveio/skills
```

Current Pi also supports `pi update --extensions` to update all installed packages;
plain `pi update` updates Pi itself. Check `pi update --help` on older versions.
Package selection remains in settings; rerun the selector only to change it.
An exact version, such as `pi install npm:@1aboveio/skills@1.0.10`, stays pinned;
install a different explicit version to change that pin. Pi version pinning is
separate from native Codex certification.

Start a new Pi session after installation, updates, or selection changes, or use
`/reload` in an existing session. At the first startup, the bundle may ask for
`/reload` after hiding duplicate shared skills from Pi.

To remove the unpinned bundle:

```sh
pi remove npm:@1aboveio/skills
```

Use the configured source if pinned, and add `--local` for a project installation.
Start a new session or run `/reload` after removal. Third-party sources selected
through the picker have separate local package entries; removing the bundle does
not remove those entries. Use `pi list` and `pi remove <source>` to manage them.

The bundle preserves existing shared skill files. In personal settings, its
extension and selector can hide matching `~/.agents/skills` copies from Pi using
negative path entries; they do not delete the copies used by Claude Code or Codex.
Project-scoped selection does not hide personal copies. Removing the package is
not a cleanup of those shared files or of previously written Pi path exclusions.

## Install in Codex CLI

Use the public catalog on `main` after its matching **1.0.10** entry is present.
The entry must pin exactly `@1aboveio/skills@1.0.10` from
`https://registry.npmjs.org/`, not `latest` or a version range. The certified client
is Codex CLI **0.164.0**, not a minimum supported version. Install that client
separately, with Git, Node.js, npm, and tar available.

```sh
codex plugin marketplace add https://github.com/1aboveio/skills.git --ref main --json
codex plugin add 1aboveio-skills@1aboveio --json
codex plugin list --marketplace 1aboveio --json
```

Check that the returned installed version is **1.0.10**, then start a **new Codex
session** before using the skills. The public repository is the marketplace
source; the plugin payload comes from npm. Codex loads the plugin's skills, not
Pi's extension or package selector.

Compaction and recovery require explicit skill instructions. After compaction or
a thread restart, reload the relevant skill and resume from its saved work state.
Certification does not guarantee automatic global hooks or automatic recovery.

### Update and Remove in Codex

To update an older installation to the catalog's certified **1.0.10** pin,
refresh the catalog, explicitly re-add the plugin, and check the installed version:

```sh
codex plugin marketplace upgrade 1aboveio --json
codex plugin add 1aboveio-skills@1aboveio --json
codex plugin list --marketplace 1aboveio --json
```

Start a new session afterward and confirm the installed version is **1.0.10**.
Refreshing the marketplace alone is not proof that the installed plugin changed.
If an update fails, do not treat it as installed; check the reported installed
version before continuing.

To uninstall the plugin, then optionally stop tracking the marketplace:

```sh
codex plugin remove 1aboveio-skills@1aboveio --json
codex plugin marketplace remove 1aboveio --json
```

Start a new session after removal. Catalog withdrawal does not remove installed
or cached copies; use native plugin removal for those copies. Plugin removal does
not delete separately installed shared skills or Pi and Claude Code installations.
Overlapping shared skills can still exist; do not assume the plugin automatically
takes precedence over a skill with the same name.

## Other Harnesses and Prerequisites

No public installation method for this bundle in Claude Code, Cursor, or other
harnesses is verified here. Pi's npm support does not imply those clients accept
the package as a native plugin. No private-repository installation is required or
offered by this guide.

The bundle does not include third-party skill bodies, external services, client
binaries, or credentials. Workflows that need these prerequisites require separate
setup. Engineering workflows expect the
[Matt Pocock toolkit](https://github.com/mattpocock/skills); frontend work may use
[Impeccable](https://github.com/pbakaus/impeccable). Pi's selector can download
these public sources separately at the revisions recorded in the npm manifest.
Selecting all entries in the picker, including `--all`, includes third-party
sources; a plain Pi bundle install does not download them. Codex plugin
installation does not provision them. A missing prerequisite blocks only the
workflows that need it.

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

## Sources

- [Public npm package](https://www.npmjs.com/package/@1aboveio/skills) and
  [1.0.10 registry manifest](https://registry.npmjs.org/@1aboveio%2fskills/1.0.10).
- [Pi package installation and resource selection](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/packages.md).
- [Pi package command reference](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/cli.md#package-commands).
- [Codex plugin use and new-session requirement](https://developers.openai.com/codex/plugins).
- [Codex CLI 0.164.0 marketplace commands](https://github.com/openai/codex/blob/rust-v0.164.0/codex-rs/cli/src/marketplace_cmd.rs) and
  [plugin add/list/remove commands](https://github.com/openai/codex/blob/rust-v0.164.0/codex-rs/cli/src/plugin_cmd.rs).
