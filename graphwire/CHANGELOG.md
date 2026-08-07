# Changelog

All notable changes to the `graphwire` module are documented in this
file. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the module
follows [Semantic Versioning](https://semver.org/). While at v0.x, minor
releases may contain breaking changes.

Releases of this module are tagged `graphwire/vX.Y.Z`. The module lives
beside the stdlib-only root so its gqlparser dependency never enters
`pluginkit` itself.

## [Unreleased]

### Added

- `Run` and `Config`, generating an application's graph resolver root
  from the plugin manifests and SDL under plugins/. Plugins opt in with
  `"graphql": true` in plugin.json and export one `<Type>Resolvers` set
  per GraphQL type they define or extend. The generator parses SDL with
  gqlparser, derives the resolver bearing types the same way gqlgen does
  (root types, argumented fields, and goField forceResolver fields), and
  emits one composite struct per shared type plus the ResolverRoot
  accessors.
