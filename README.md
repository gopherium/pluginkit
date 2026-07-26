# pluginkit

Compile-time plugin infrastructure for Go applications: a lifecycle host,
plugin wiring generation, and route guarding. Framework-free and
storage-agnostic. The application owns its plugin contract and its SDK
surface, and pluginkit owns the mechanism underneath.

## Design

pluginkit is a set of primitives, not a framework. It grows by adding small
independent building blocks, each usable on its own.

- `Plugin` is the lifecycle contract, with the optional `Migrator`,
  `RouteProvider`, and `PublicPathProvider` capabilities.
- `Host` migrates, starts, and stops a fixed set of plugins with rollback
  and panic isolation.
- `Protect` guards a plugin's mounted routes with caller-supplied
  middleware while letting its declared public paths through untouched.
- `wire` generates the Go and TypeScript plugin wiring files from
  `plugins/*/plugin.json` manifests.

Applications re-export the lifecycle types from their own SDK package as
type aliases, so plugins depend only on the application's contract.

## Usage

```go
host := pluginkit.NewHost(registeredPlugins...)
if err := host.Start(ctx); err != nil {
    // every migrator ran first and a failed start rolled back cleanly
}
defer host.Stop(ctx)

for id, handler := range host.Routes() {
    mux.Handle("/api/plugins/"+id+"/", http.StripPrefix("/api/plugins/"+id,
        pluginkit.Protect(handler, host.PublicPaths()[id], requireSession)))
}
```

## License

Copyright 2026 Manuel 'SirLouen' Camargo

Apache License 2.0 ([LICENSE](LICENSE)). Every file carries an
`SPDX-License-Identifier: Apache-2.0` header.
