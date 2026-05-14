---
name: aspid-profiler-marker
description: Use when the user asks to instrument code with a Unity ProfilerMarker via Aspid.FastTools' `this.Marker()` extension. Triggers on phrases like "profile this method", "add a profiler marker", "instrument with ProfilerMarker", "wrap this in this.Marker()", or any reference to `this.Marker()`/`WithName(...)`. Inserts the correct `using (this.Marker()) { ... }` scope at the call site, picks a stable line position so the source generator emits a meaningful per-call-site marker, and warns about anti-patterns (loops, lambdas, helper wrappers) that defeat the generator's identity model.
---

# aspid-profiler-marker

Instrument code with a `ProfilerMarker` using `this.Marker()`. The Aspid.FastTools
source generator replaces every `this.Marker()` call site with a unique
`ProfilerMarker.Auto` scope keyed by `(EnclosingType, Method, Line)`.

## When this skill applies

Trigger when the user asks for any of:

- "add a profiler marker to X"
- "profile this method" / "instrument this hot path"
- references to `this.Marker()` or `.WithName(...)`
- "show this in the Unity Profiler"

Do **not** trigger for `Profiler.BeginSample` / `EndSample` — that's the lower-level
Unity API and not what this package provides.

## How `this.Marker()` works

The extension method lives in the **global namespace** (intentional — no `using`
required at the call site):

```csharp
public static ProfilerMarker.AutoScope Marker(this object _);
public static ProfilerMarker.AutoScope WithName(this in ProfilerMarker.AutoScope marker, string name);
```

At runtime the global-namespace methods above are never actually called. For every
enclosing type that contains `this.Marker()` call sites, the source generator emits a
companion `internal static class __TypeNameProfilerMarkerExtensions` with:

- a nested `Markers` class holding a `static readonly ProfilerMarker` per call site,
  named `"TypeName.MethodName (Line)"` (for properties — `TypeName.PropertyName`,
  for constructors — `TypeName.Ctor`);
- a more specific `Marker(this TypeName _, [CallerLineNumber] int line = -1)` overload
  that dispatches on the line number:

```csharp
internal static class __FooBarProfilerMarkerExtensions
{
    private static class Markers
    {
        public static readonly ProfilerMarker UpdateGrid_42 = new("FooBar.UpdateGrid (42)");
    }

    public static ProfilerMarker.AutoScope Marker(this FooBar _, [CallerLineNumber] int line = -1)
    {
#if ENABLE_PROFILER
        if (line is 42) return Markers.UpdateGrid_42.Auto();
#endif
        return default;
    }
}
```

Because the overload is more specific than the global one, the compiler picks it
whenever `this` is of the right enclosing type. The marker is unique per
`(EnclosingType, Method, Line)`. **Identity is line-based.**

## Steps

### 1. Locate the target method

The method must be:

- An instance method (or any context where `this` is in scope). For static methods,
  `this.Marker()` is not available — use a regular `ProfilerMarker` field instead.
- Inside a `class` or `struct` you control. The generator runs on the user's
  compilation, so the file must be in their project, not in a referenced DLL.

### 2. Wrap the body

Default form (marker name = method name):

```csharp
public void UpdateGrid()
{
    using (this.Marker())
    {
        // hot work
    }
}
```

Custom name (useful when one method has several distinct phases):

```csharp
public void Tick()
{
    using (this.Marker().WithName("Tick.Pre"))
    {
        Prepare();
    }

    using (this.Marker().WithName("Tick.Solve"))
    {
        Solve();
    }
}
```

### 3. Confirm the namespace context

The user does **not** need to add a `using` for `this.Marker()` itself — the extension
is in the global namespace by design.

If they want the `ProfilerMarker` *type* anywhere else, add:

```csharp
using Unity.Profiling;
```

### 4. Conditional compilation (optional)

The package's own runtime files use this guard so consumers can strip markers:

```csharp
#if !ASPID_FAST_TOOLS_UNITY_PROFILER_DISABLED
            using (this.Marker())
#endif
            {
                // body
            }
```

Suggest this only if the user explicitly asks for a kill-switch — for most projects
the unconditional form is correct.

## Anti-patterns to avoid

The generator's identity is `(Type, Method, Line)`. Anything that breaks the 1:1
mapping between a call site and a runtime occurrence makes the marker meaningless:

| Anti-pattern | Why it's bad | Fix |
|---|---|---|
| `this.Marker()` inside a `for`/`while` body | Same call site fires N times — looks like one slow scope, not per-iteration data. | Wrap the **outer** loop, not the body. If you need per-iteration timing, give each iteration its own named marker via `.WithName($"…")` outside the call-site rule (and accept the GC cost). |
| `private void Profile(Action a) { using (this.Marker()) a(); }` | Every caller resolves to the SAME line — all callers collapse into one marker. | Inline `using (this.Marker())` at each call site. The whole point is per-call-site identity. |
| `this.Marker()` inside a lambda / local function defined inside a loop | Same problem as the helper wrapper — the call site is one line, executed many times. | Move the `using` outside the lambda, or pass a different `WithName` per invocation. |
| `this.Marker()` in a generic method shared across types | Marker's enclosing-type metadata may be ambiguous depending on instantiation. | Inline at concrete call sites, or use a regular `ProfilerMarker` field with an explicit name. |
| `this.Marker()` in a `static` method | `this` is not in scope; will not compile. | Use `new ProfilerMarker("…").Auto()` directly, or refactor to an instance method. |

## Quick template

```csharp
// Method-level scope
public void DoWork()
{
    using (this.Marker())
    {
        // work
    }
}

// Multi-phase
public void Tick()
{
    using (this.Marker().WithName("Tick.Phase1")) { /* ... */ }
    using (this.Marker().WithName("Tick.Phase2")) { /* ... */ }
}
```

## Verification

After inserting `this.Marker()`, remind the user to:

1. Let Unity recompile (the generator runs as a Roslyn analyzer step).
2. Open **Window → Analysis → Profiler**, switch to Hierarchy or Timeline view.
3. Filter by the type/method name — markers appear as `TypeName.MethodName (Line)`
   or the custom name from `.WithName(...)`.
