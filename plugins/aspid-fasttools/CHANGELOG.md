# Changelog

All notable changes to the `aspid-fasttools` plugin are tracked here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and
the version numbers follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

While the plugin is in `0.x`, skill triggers, descriptions and structure may
change between minor versions as real-world usage feedback comes in. Pin a
specific commit or tag if you need stability.

## [Unreleased]

## [0.2.0-alpha] — 2026-05-14

### Fixed
- `aspid-visual-element-fluent`: removed nonexistent `params string[]` overloads of
  `AddClass` / `RemoveClass` / `AddStyleSheets`; references to a `ToggleClass`
  method that does not exist are replaced with the real `ToggleInClass` /
  `EnableInClass` / `ClearClasses`. Multi-class examples now chain
  `.AddClass(...).AddClass(...)` instead of producing code that would not compile.
- `aspid-visual-element-fluent`: removed the inaccurate mention of an
  `Aspid.FastTools.Unity.VisualElements.Math` satellite namespace. The Mathematics
  overloads live in the same `Aspid.FastTools.UIElements` namespace and are gated
  by the `ASPID_FASTTOOLS_UNITY_MATHEMATICS_INTEGRATION` define.
- `aspid-profiler-marker`: the "How `this.Marker()` works" section now matches the
  actual generator output — a companion `__TypeNameProfilerMarkerExtensions`
  class with a nested `Markers` table and a `[CallerLineNumber]` overload that
  dispatches on line. Marker name format corrected to `TypeName.Method (Line)`.

### Changed
- Plugin README "Versioning" section now lists only the public types actually
  touched by the current skill set. `SerializableType` and `EnumValues<TValue>`
  are explicitly called out as not yet covered.
- Root README plugin entry now describes `aspid-id-struct` as scaffolding
  "`IId` structs and `[UniqueId]` fields" and broadens
  `aspid-visual-element-fluent` to "editor or runtime UI".
- Marketplace description softened — no longer promises general-purpose plugins
  that have not shipped yet.
- Plugin manifest aligned with the marketplace entry: author email added,
  `homepage` repointed at the plugin folder in this repo, keywords extended with
  `ids` / `profiler` / `profiler-marker`.
- Marketplace plugin entry now carries an explicit `version` and `tags`.

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
