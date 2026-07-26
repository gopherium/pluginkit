# Changelog

## 0.1.0

Initial release:

- Lifecycle contract: `Plugin`, `Migrator`, `RouteProvider`,
  `PublicPathProvider`.
- `Host`: migrate-before-start, in-order start with reverse-order stop,
  rollback on failed start, panic isolation per plugin call.
- `Protect`: exact-match public-path passthrough around caller-supplied
  middleware.
- `wire`: plugin manifest loading/validation and Go + TypeScript wiring
  generation, parameterized by the consuming application.
