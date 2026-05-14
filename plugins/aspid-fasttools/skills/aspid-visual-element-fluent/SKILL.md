---
name: aspid-visual-element-fluent
description: Use when the user is building or modifying UIToolkit code (editor or runtime) and wants to use Aspid.FastTools' fluent VisualElement extensions instead of imperative `element.style.X = …` / `RegisterValueChangedCallback` / `AddToClassList` calls. Triggers on phrases like "build editor UI", "use fluent VisualElement", "style this VisualElement", "chainable UIToolkit", "add a button with click handler", or any C# touching UnityEngine.UIElements types. Provides a catalog of available fluent methods grouped by domain and shows the canonical chain-building pattern.
---

# aspid-visual-element-fluent

Use the fluent `VisualElement` extensions shipped by Aspid.FastTools to build
UIToolkit trees with a single chained expression instead of multi-line imperative
property setters.

## When this skill applies

Trigger when the user:

- writes new UIToolkit code (editor windows, inspectors, runtime UI)
- modifies an existing `VisualElement` configuration with property assignments
- asks for "fluent" / "chainable" / "builder" UIToolkit code
- references `style.X = Y`, `AddToClassList`, `RegisterValueChangedCallback`, `clicked +=`,
  `style.flexDirection = …`, etc., where a fluent equivalent exists

Do **not** trigger for non-UIToolkit UI (uGUI, IMGUI), or for UXML/USS authoring (those
have their own files). Mixed UXML + C# is fine — the C# part still uses these extensions.

## Namespace

All extensions live in:

```csharp
using Aspid.FastTools.UIElements;
```

Note: `UIElements`, **not** `VisualElements`. A common mistake is to assume the
namespace mirrors the folder path — it does not.

## Core convention

Every extension is generic in the receiver type:

```csharp
public static T SetXxx<T>(this T element, …) where T : VisualElement
{
    // mutate
    return element;
}
```

The return type preserves the most-derived static type. So:

```csharp
Button btn = new Button()
    .SetName("submit")          // returns Button (not VisualElement)
    .AddClicked(OnSubmit)       // Button-specific, only available because T is Button
    .SetWidth(120);             // returns Button
```

This means **subtype-specific methods are available mid-chain** as long as you start
the chain with the subtype.

## Catalog (by domain)

### Identity & state — `VisualElementExtensions`

`SetName`, `SetVisible`, `SetEnabled`, `SetTooltip`, `SetUserData`, `SetPickingMode`,
`SetFocusable` (where applicable), `SetTabIndex`.

### USS classes & stylesheets — `VisualElementExtensions.Uss`

`AddClass(string)`, `RemoveClass(string)`, `ToggleInClass(string)`,
`EnableInClass(string className, bool enable)`, `ClearClasses()`,
`AddStyleSheets(StyleSheet)`, `RemoveStyleSheets(StyleSheet)`,
`AddStyleSheetsFromResource(string path)`, `RemoveStyleSheetsFromResource(string path)`.

All take a **single** value — chain calls to apply multiples:

```csharp
panel
    .AddClass("my-panel")
    .AddClass("my-panel--compact")
    .AddStyleSheetsFromResource("UI/MyPanel/Styles");
```

### Children — `VisualElementExtensions.Child`

`AddChild(VisualElement)`, `AddChildren(params VisualElement[])`,
`AddChildren(IEnumerable<VisualElement>)`.

The chain pattern for tree construction:

```csharp
return new VisualElement()
    .AddClass("toolbar")
    .AddChildren(
        new Button().SetName("undo").AddClicked(Undo),
        new Button().SetName("redo").AddClicked(Redo),
        new ToolbarSpacer(),
        new TextField().SetName("search"));
```

### Layout & sizing — `VisualElementExtensions.Style`, `IStyleExtensions`

| Group | Methods |
|---|---|
| Flex | `SetFlexDirection`, `SetFlexGrow`, `SetFlexShrink`, `SetFlexBasis`, `SetFlexWrap` |
| Alignment | `SetAlignItems`, `SetAlignSelf`, `SetAlignContent`, `SetJustifyContent` |
| Size | `SetWidth`, `SetHeight`, `SetMinWidth`, `SetMaxWidth`, `SetMinHeight`, `SetMaxHeight`, `SetAspectRatio` |
| Position | `SetPosition`, `SetLeft`, `SetRight`, `SetTop`, `SetBottom` |
| Spacing | `SetMargin`, `SetMarginX`, `SetMarginY`, `SetMarginLeft/Right/Top/Bottom`, same family for `SetPadding*` |

All sizing/spacing methods accept either `float` (pixels) or `Length` (e.g. `Length.Percent(50)`).

### Color & background

`SetColor`, `SetBackgroundColor`, `SetBackgroundImage(Texture2D | Sprite | VectorImage | StyleBackground)`,
`SetBackgroundImageFromResource(string)`, `SetBackgroundSize`, `SetBackgroundPosition`,
`SetBackgroundPositionX`, `SetBackgroundPositionY`, `SetBackgroundRepeat`.

### Borders

`SetBorderColor`, `SetBorderColorX`, `SetBorderColorY`, `SetBorderColorTop/Bottom/Left/Right`;
`SetBorderWidth`, same family per side; `SetBorderRadius`,
`SetBorderRadiusTop/Bottom`, `SetBorderRadiusTopLeft/TopRight/BottomLeft/BottomRight`.

### Text

`SetFontSize`, `SetLetterSpacing`, `SetUnityFont`, `SetUnityFontDefinition`,
`SetUnityFontStyle`, `SetUnityTextAlign`, `SetTextOverflow`, `SetWhiteSpace`,
`SetTextShadow` (where supported by the Unity version).

### Visual effects

`SetOpacity`, `SetCursor`, `SetDisplay`, `SetVisibility`, `SetFilter`, `SetOverflow`.

### Transitions

`SetTransitionProperty`, `SetTransitionDuration`, `SetTransitionTimingFunction`,
`SetTransitionDelay`. All accept arrays for multi-property transitions.

### Style presets — `VisualElementExtensions.Style.Preset`, `IStyleExtensions.Preset`

Higher-level helpers like centering, full-stretch, single-axis fill, etc. Browse the
file when a multi-property combination is repeated three times in the same project.

### Per-element domains

| Element | Methods (selection) |
|---|---|
| `Button` | `AddClicked(Action)`, `RemoveClicked(Action)`, `SetClickable`, `SetIconImage`, `SetText` |
| `BaseField<T>` | `SetLabel`, `SetValue`, `SetShowMixedValue`, `SetBindingPath` |
| `BaseBoolField` | `SetText`, `SetValue` |
| `EnumField` | `Init(Enum)`, `SetValue` |
| `INotifyValueChanged<T>` | `AddValueChanged(EventCallback<ChangeEvent<T>>)`, `RemoveValueChanged`, `SetValueWithoutNotify`, `SetValue` (overloads for primitives, `string`, `Color`, `Vector2/3/4`, `Object`; Mathematics types — `float2/3/4`, `int2/3/4`, `bool2/3/4` etc. — live in the same `Aspid.FastTools.UIElements` namespace, gated by the `ASPID_FASTTOOLS_UNITY_MATHEMATICS_INTEGRATION` define) |
| `Slider` / `SliderInt` | `SetLowValue`, `SetHighValue`, `SetDirection`, `SetPageSize`, `SetShowInputField` |
| `ProgressBar` | `SetLowValue`, `SetHighValue`, `SetTitle`, `SetValue` |
| `Foldout` | `SetText`, `SetValue` |
| `HelpBox` | `SetText`, `SetMessageType` |
| `Image` | `SetImage`, `SetSprite`, `SetVectorImage`, `SetTintColor`, `SetScaleMode` |
| `IMGUIContainer` | `SetOnGUIHandler`, `SetCullingEnabled` |
| `BaseListView` / `ListView` | `SetItemsSource`, `SetMakeItem`, `SetBindItem`, `SetUnbindItem`, `SetDestroyItem`, `SetFixedItemHeight`, `SetSelectionType`, etc. |
| `TextElement` | `SetText`, `SetEnableRichText`, `SetSelection*` |
| `IMixedValueSupport` | `SetShowMixedValue` |
| `Manipulator`s | `AddManipulator`, `RemoveManipulator`, `WithManipulator` |
| `CallbackEventHandler` | `AddCallback<TEvent>(EventCallback<TEvent>)`, `RemoveCallback<TEvent>` |
| `Focusable` | `SetTabIndex`, `SetFocusable`, `SetDelegatesFocus` |

When in doubt, look in `Unity/Runtime/VisualElements/Extensions/{Folder}/`.

## Canonical patterns

### Building a tree top-down

```csharp
using Aspid.FastTools.UIElements;
using UnityEngine.UIElements;

public class MyEditorWindow : EditorWindow
{
    private void CreateGUI()
    {
        rootVisualElement.AddChild(new VisualElement()
            .AddClass("aspid-window")
            .AddStyleSheetsFromResource("UI/MyEditorWindow/Styles")
            .SetFlexDirection(FlexDirection.Column)
            .AddChildren(
                new Label("Settings").AddClass("aspid-window__title"),
                new TextField()
                    .SetLabel("Name")
                    .AddValueChanged(OnNameChanged),
                new Toggle()
                    .SetLabel("Enabled")
                    .AddValueChanged(OnEnabledChanged),
                new Button()
                    .SetText("Apply")
                    .AddClass("aspid-button--primary")
                    .AddClicked(Apply)));
    }
}
```

### Imperative → fluent rewrites

| Imperative | Fluent |
|---|---|
| `el.style.flexDirection = FlexDirection.Row;` | `el.SetFlexDirection(FlexDirection.Row);` |
| `el.style.backgroundColor = Color.red;` | `el.SetBackgroundColor(Color.red);` |
| `el.AddToClassList("foo"); el.AddToClassList("bar");` | `el.AddClass("foo").AddClass("bar");` |
| `btn.clicked += OnClick;` | `btn.AddClicked(OnClick);` |
| `field.RegisterValueChangedCallback(OnChange);` | `field.AddValueChanged(OnChange);` |
| `el.style.marginLeft = 4; el.style.marginRight = 4;` | `el.SetMarginX(4);` |
| `parent.Add(child1); parent.Add(child2);` | `parent.AddChildren(child1, child2);` |

### Mixed UXML + fluent

When the tree comes from UXML and you only need to wire callbacks / dynamic styling:

```csharp
var tree = uxml.CloneTree();
rootVisualElement.AddChild(tree);

tree.Q<Button>("submit").AddClicked(Submit);
tree.Q<TextField>("name").AddValueChanged(OnNameChanged);
tree.Q<VisualElement>("status").AddClass("status").AddClass("status--idle");
```

`Q<>` is Unity's API; everything after it is fluent.

## Anti-patterns to avoid

- **Mixing styles.** Don't write half the chain fluently and the other half via
  `element.style.X = Y` — pick one. The fluent form is preferred for new code.
- **`AddToClassList` when `AddClass` exists.** `AddClass` returns the element, so it
  composes; `AddToClassList` does not. For multiple classes, chain `.AddClass("a").AddClass("b")`.
- **Building a parent then mutating children separately when a chain is shorter.**
  ```csharp
  // ❌
  var row = new VisualElement();
  row.style.flexDirection = FlexDirection.Row;
  var btn = new Button();
  btn.text = "Go";
  btn.clicked += Go;
  row.Add(btn);

  // ✅
  var row = new VisualElement()
      .SetFlexDirection(FlexDirection.Row)
      .AddChild(new Button().SetText("Go").AddClicked(Go));
  ```
- **Forgetting the `Aspid.FastTools.UIElements` using.** Without it, the IDE will
  fall back to imperative property assignments and the chain won't compile.

## Verification

After applying fluent changes, confirm:

1. Code compiles (chain types resolve correctly — most issues come from receiver type
   narrowing, e.g. assigning the result of `.AddClass(...)` to `Button` when the chain
   started from `VisualElement`).
2. The visual result matches the imperative version (open the window / inspector).
3. UI Toolkit Debugger (**Window → UI Toolkit → Debugger**) shows the same hierarchy
   and class list.
