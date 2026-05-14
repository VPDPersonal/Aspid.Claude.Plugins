# Changelog

All notable changes to the `aspid-fasttools` plugin are tracked here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and
the version numbers follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

While the plugin is in `0.x`, skill triggers, descriptions and structure may
change between minor versions as real-world usage feedback comes in. Pin a
specific commit or tag if you need stability.

## [Unreleased]

## [0.1.0] — 2026-05-14

### Added
- Initial alpha release.
- `aspid-id-struct` — scaffolds `partial struct : IId` declarations and the
  matching `IdRegistry` / `IdRegistry<T>` asset workflow.
- `aspid-profiler-marker` — inserts `this.Marker()` / `.WithName(...)` call
  sites and warns about call-site identity anti-patterns (loops, helper
  wrappers, lambdas, generic methods, `static` methods).
- `aspid-visual-element-fluent` — catalogs the fluent `VisualElement` extensions
  from the `Aspid.FastTools.UIElements` namespace and provides imperative →
  fluent rewrite patterns.
