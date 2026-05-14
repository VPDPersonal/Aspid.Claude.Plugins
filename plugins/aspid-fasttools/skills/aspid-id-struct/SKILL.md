---
name: aspid-id-struct
description: Use when the user asks to create an "id struct", "unique id", "[UniqueId]"-based identifier, or wants a stable int↔string id system tied to an Aspid.FastTools IdRegistry. Triggers on phrases like "create an id struct for X", "add a unique id", "make this a project id", "register Y in IdRegistry". Generates a `partial struct : IId` in the right runtime assembly, explains the 1:1 binding to a registry asset, and reminds the user to create the registry via Assets → Create → Aspid/Id Registry.
---

# aspid-id-struct

Generate the boilerplate consumers need to add a new project-wide id type backed by
`Aspid.FastTools.Ids`.

## When this skill applies

Trigger when the user asks for any of:

- "create an id struct for X" / "add a unique id for X"
- "make X a project-wide id" / "stable int id for X"
- "register X in `IdRegistry`"
- references to `[UniqueId]`, `IId`, `IdRegistry`, or `IdStructGenerator`

Do **not** trigger for plain integer enums, `Guid`, or non-Aspid id systems.

## What the user gets

The package's `IdStructGenerator` does the heavy lifting. The user only writes a
**partial struct that implements `IId`**. The generator emits:

- `private int _id;` (serialized field)
- `public int Id => _id;`
- editor-only `__stringId` (drives the registry-aware drawer)

Generated file naming: `{StructName}.IId.g.cs`.

## Steps

### 1. Pick a name and namespace

If the user did not give a name, suggest one based on the domain (`ItemId`, `EnemyId`,
`AbilityId`, `SfxId`). Confirm before writing.

The struct must live in a **runtime** assembly (player-build), i.e. *not* under
`Editor/` and *not* in an `Aspid.FastTools.Unity.Editor`-style asmdef.

### 2. Write the struct

```csharp
using Aspid.FastTools.Ids;

namespace Game.Items
{
    public partial struct ItemId : IId { }
}
```

Required properties:

- `partial` — without it the generator emits nothing.
- Implements `IId` — the generator checks `AllInterfaces` for `Aspid.FastTools.Ids.IId`.
- Public visibility is recommended; `internal` works if the consumer is the same assembly.

Do **not** add `_id` field, `Id` property, or any other body member — they are generated.

### 3. Create the registry asset

Tell the user to create the asset in the editor:

> **Assets → Create → Aspid → Id Registry**

Each `IId` struct binds to **exactly one** `IdRegistry` asset. The package's
`IdRegistryResolver` enforces uniqueness at lookup time. If the user already has a
registry for this struct, they should reuse it; the editor drawer will warn on duplicates.

For type-safe lookups, suggest a strongly-typed wrapper:

```csharp
using Aspid.FastTools.Ids;
using UnityEngine;

[CreateAssetMenu(fileName = "ItemRegistry", menuName = "Game/Item Registry")]
public sealed class ItemRegistry : IdRegistry<ItemId> { }
```

`IdRegistry<T>` adds `TryGetName(T id, out string)` and `Contains(T id)` overloads.

### 4. Use `[UniqueId]` on inspector fields (optional)

When a field stores an id picked in the editor, mark it with `[UniqueId]` for the
registry-aware picker:

```csharp
using Aspid.FastTools.Ids;
using UnityEngine;

public class ItemDefinition : ScriptableObject
{
    [SerializeField, UniqueId] private int _itemId;
}
```

`UniqueIdAttribute` is `[Conditional("UNITY_EDITOR")]`, so its usages are stripped from
player builds. Runtime code reads the resolved `int` directly.

### 5. Look up at runtime

```csharp
using Aspid.FastTools.Ids;
using UnityEngine;

public class ItemLookup : MonoBehaviour
{
    [SerializeField] private ItemRegistry _registry;

    private void Start()
    {
        if (_registry.TryGetId("Sword", out int id))
            Debug.Log($"Sword id = {id}");

        if (_registry.TryGetName(42, out string name))
            Debug.Log($"42 = {name}");
    }
}
```

Available on `IdRegistry`:

- `TryGetId(string name, out int id)`
- `TryGetName(int id, out string name)`
- `Contains(int id)`, `Contains(string name)`
- `Count`, `Ids`, `IdNames`
- `IEnumerable<KeyValuePair<int, string>>`

Available on `IdRegistry<T>` additionally:

- `TryGetName(T id, out string name)`
- `Contains(T id)`

## Anti-patterns to avoid

- **Multiple registries for the same struct.** The resolver picks one; the others are
  silently ignored. One struct ↔ one registry.
- **Manually writing `_id` / `Id`.** The generator already emits them; conflicts will
  break the build.
- **Forgetting `partial`.** Without it, no code is generated — the user will see
  "missing `IId.Id`".
- **Putting the struct under `Editor/`** — the registry needs the type in a runtime
  assembly so player builds can reference ids.
- **Using `[UniqueId]` on fields that aren't of type `int`.** The drawer expects a
  serialized integer.

## Verification checklist

After generating files for the user, remind them to:

1. Let Unity reimport (the source generator runs on compile).
2. Create the registry asset via **Assets → Create → Aspid → Id Registry**
   (or the strongly-typed wrapper's own `[CreateAssetMenu]` path).
3. Open the registry inspector and add named entries.
4. Assign the registry asset to any field that needs runtime lookup.
