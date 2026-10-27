# Catalog Scope

This is the public catalog repository, not the implementation repository.

- Publish only public catalog metadata and documentation. Never copy private
  source history, credentials, internal logs, or operator evidence here.
- Keep `.agents/plugins/marketplace.json` absent before the first certified
  native-compatible npm release. Do not invent certification or activate an
  incompatible version to populate the catalog.
- Use marketplace name `1aboveio`, plugin name `1aboveio-skills`, npm package
  `@1aboveio/skills`, an exact certified version, and explicit registry
  `https://registry.npmjs.org/`.
- Preserve the previous verified pin when publication or certification fails.
  Catalog withdrawal does not remove installed copies.
- Verify public access and real native installation after catalog delivery.
