# TransformSnapshot Tool
 ***Tool Version — v1.0.0***


A Unity Editor tool for capturing, managing, and restoring transform configurations in your scenes. Works in Edit Mode, Play Mode and Runtime.
 

---

## Table of Contents


- [Installation](#installation)
- [Quick Start](#quick-start)
- [Config Portability — Capture Relative, Place Anywhere](#config-portability)
- [Capturing Transforms](#capturing-transforms)
- [Applying a Configuration](#applying-a-configuration)
- [Apply Modes](#apply-modes)
- [Transform Components](#transform-components)
- [Categories](#categories)
- [Managing Configurations](#managing-configurations)
- [Filtering and Search](#filtering-and-search)
- [Multi-Select / Bulk Actions](#multi-select--bulk-actions)
- [Sending a Config to a Script](#sending-a-config-to-a-script)
- [Cross-Scene Config Import](#cross-scene-config-import)
- [Pasting Transforms Between Configs](#pasting-transforms-between-configs)
- [Resolution Dialog](#resolution-dialog)
- [Scene Operations](#scene-operations)
- [Options Menu](#options-menu)
- [Maintenance](#maintenance)
- [Real World Use Cases](#real-world-use-cases)
- [The Power of Config Stacking](#the-power-of-config-stacking)
- [Runtime API](#runtime-api)
  - [SimpleRuntimeApplier](#simpleruntimeapplier)
  - [RuntimeCapturer](#runtimecapturer)
  - [RuntimeCaptureApplier](#runtimecaptureapplier)
  - [RuntimeApplyOptions](#runtimeapplyoptions)
  - [RuntimeCaptureWriter](#runtimecapturewriter)
- [Data Storage](#data-storage)
- [FAQ](#faq)

---

## Installation

1. Import the package into your Unity project via the Package Manager or by placing the folder inside `Assets/`.
2. No additional setup needed — the tool activates automatically once imported.
3. Launch it from Tools / TransformSnapshot / Main Window

---
## Quick Start

Your first two snapshots in 60 seconds. The most common workflow: capture a layout, rearrange objects, then save a second snapshot so you can switch between both with one click.

| Step | Action |
|---|---|
| 1 — Capture | Select objects in the Hierarchy → click **Capture** → name it `Layout_A` → OK |
| 2 — Duplicate | Click the **Duplicate** button beside Apply. You now have `Layout_A` and `Layout_A (1)` |
| 3 — Rearrange | Move, rotate, or scale your objects in the scene however you like |
| 4 — Update | Click **Update** on `Layout_A (1)` to re-capture transforms at their new values |
| 5 — Done | Click **Apply** on either config to instantly switch between both layouts |

> Rename `Layout_A (1)` to `Layout_B` — right-click the card → **Rename**.

---

## Config Portability

**Capture relative, place anywhere.**

When you capture a config, exclude the topmost parent object and capture only its children. The children's transforms are stored in local space relative to their parent — so when you move, rotate, or reposition the parent anywhere in the scene and apply the config, all children restore exactly where they were relative to that parent, not to the world. The parent becomes a portable container: place it anywhere and the config follows.

> If objects logically belong together and move as a group, parent them under an empty GameObject, capture everything except that empty, and the config becomes position-independent.

---

## Capturing Transforms

### Basic Capture

1. Select one or more GameObjects in the **Hierarchy**.
2. Click **Capture (N)** — the button shows the selection count.
3. Name the configuration and choose a category.
4. Click **OK**.

### Capture Scope

Choose what gets included in the Capture dialog:

| Option | What is captured |
|---|---|
| Selected only *(default)* | Exactly the selected GameObjects |
| Selected + all children | Selected objects and their entire subtrees |
| All children only | Every descendant, but not the selected roots |
| Direct children only | One level of children only |

**Config Name** — set in the capture dialog or rename later by right-clicking the card.
**Category** — assign to an existing category or create one with the **+** button. The last used category is remembered.

### The Config List

Each configuration appears as a card showing the config name, transform count (roots = top-most captured parents, children = total children of all roots), and a status colour:

- **Grey** — all transforms valid
- **Yellow** — some transforms missing
- **Red** — all transforms missing

---

## Applying a Configuration

**Collapsed card (quick apply):**
Click the green **Apply** button on the right side of any config card.

**Expanded card (with settings):**
Click a config card to expand it, adjust the apply settings, then click the large **Apply** button.

The apply button label shows how many transforms will be affected, e.g. **Apply (5)**.

---

## Apply Modes

Each configuration has an independent apply mode:

| Mode | Behaviour |
|---|---|
| **Auto** *(recommended)* | Root objects use world space; children use local space. Handles mixed hierarchies correctly out of the box. |
| **World Space** | Objects are placed at their exact captured world positions. Best for room layouts and absolute prop placement. |
| **Local Space** | Transforms are applied relative to the current parent. Best for character poses and reusable setups. |
| **Selected Only** | Editor only — applies only to objects currently selected in the scene. Best for partial updates. |

---

## Transform Components

Toggle **Position**, **Rotation**, and **Scale** independently inside an expanded card. The apply button label reflects active components — **Apply PR** means position and rotation only.

When fewer than all three are active the apply button turns amber and shows **Apply Partial**.

---

## Categories

Categories are coloured header bars that group configs visually.

**Creating a category:**
- Use **Options (⚙) → Create New Category** from the toolbar, or
- Choose **Create New Category** when capturing or moving a config.

**Category context menu (⚙ button on header):**

| Action | Description |
|---|---|
| Isolate | Show only this category's configs |
| Show All Categories | Exit isolation mode |
| Rename Category | Rename. Merges into the target name if it already exists. |
| Move Category Up / Down | Reorder categories |
| Apply All Configs | Apply every config in this category at once |
| Delete All Configs | Remove all configs but keep the category |
| Delete Category | Remove the category; configs move to **Uncategorised** |
| Delete Category and Configs | Remove both the category and all its configs |
| Import from Export Queue | Import configs queued from another scene into this category |
| Change Color | Pick a custom header colour |
| Reset to Default Color | Revert to the auto-assigned colour |

> Configs without a category are grouped under **Uncategorised**, which always appears last.

---

## Managing Configurations

**Expanding a card** reveals all settings and actions. Click the card again to collapse it.

**Card buttons:**

| Button | Action |
|---|---|
| Apply | Apply all transforms instantly |
| Duplicate | Create an identical copy |
| Update | Re-capture all transforms at their current scene values |
| + | Add selected Hierarchy objects to this config |
| − | Remove selected Hierarchy objects from this config |

**Config right-click context menu:**

| Item | Action |
|---|---|
| Rename | Rename the configuration |
| Move Up / Down in Category | Reorder within the category |
| Move to Category | Move to a different category |
| Remove Orphaned Transforms | Delete entries for deleted objects |
| Add to Export Queue | Queue for cross-scene import |
| Paste Transforms | Paste pose data from another config onto this config's objects |
| Delete | Permanently delete the config |

**Clicking the info line** (e.g. *2 transforms — 2 roots, 0 children*) selects all captured objects in the scene and frames them in the Scene View.

> Never rename or move config asset files manually — always use the tool's Rename action to keep internal references intact.

---

## Filtering and Search

**Search bar** — type to filter configs by name in real time.

**Category dropdown** — filter to show only one category at a time.

**Selection filter** — select objects in the Hierarchy, then toggle **Filter configs for (N objects selected)** to show only configs that contain those objects.

| Mode | Shows configs that… |
|---|---|
| Any | contain at least one selected object |
| All | contain every selected object |
| Only | contain exactly the selected objects and nothing else |

---

## Multi-Select / Bulk Actions

1. Enable **Multi-Select Mode** from the **Options (⚙)** menu.
2. Check the configs you want to act on.
3. Click **Bulk Actions**.

| Action | Description |
|---|---|
| Select All Visible | Check all currently visible configs |
| Deselect All | Uncheck everything |
| Move to Category | Move all selected to a category |
| Export : Add Selected to Queue | Queue selected configs for cross-scene import |
| Delete Selected | Delete all checked configs permanently |

---

## Cross-Scene Config Import

Every transform is stored using Unity's `GlobalObjectId` — an identifier tied to the specific scene file. To use a config in another scene, export it through the queue and import it into the destination scene. The resolve dialog remaps each stored hierarchy path to the matching object in the new scene, handling renamed or restructured hierarchies gracefully. Once imported, the config is fully native to the destination scene with fresh `GlobalObjectId` references — the original is untouched.

### Exporting (source scene)

1. Enable **Multi-Select Mode** through the Options menu.
2. Check the configs to export.
3. **Bulk Actions → Export : Add Selected to Queue**.
4. A red badge on the Options button shows the number of queued configs.

> Alternatively, right-click any config card → **Export : Add to Queue**.

### Importing (destination scene)

1. Open the destination scene.
2. **Options menu (⚙) → Import from Export Queue (N pending)**.
3. The queue dialog lists all pending configs grouped by source scene.
4. Click **Import** on each config to open the Resolution Dialog.
5. Walk through any renamed or ambiguous object mappings.

The more your scenes share a common hierarchy structure, the faster the import resolves with no manual intervention needed.

---

## Pasting Transforms Between Configs

The Paste Transforms workflow copies the pose data from one config onto the live objects tracked by another config — without requiring the two configs to have identically named objects.
1. Right-click any config card and click **Copy Transforms**.
1. Right-click any other config card and click **Paste Transforms**.

3. The **Resolution Dialog** opens up to map source entries to target objects.
4. Click **Paste** to apply. The target config's stored pose is updated with the pasted values so re-applying it later uses the new pose.

---

## Resolution Dialog

The Resolution Dialog appears during both **Import** and **Paste** workflows to handle cases where object names differ between source and destination.

### Import mode

- The left column lists the source config's transforms.
- The right column shows an **ObjectField** for the currently active row — drag a GameObject from the Hierarchy to resolve it instantly.
- Navigate between rows using **↑ Prev** and **↓ Next**.
- Resolved rows show green with a ✕ button to clear and re-pick.
- Resolving one segment automatically propagates to all configs sharing that ancestor — you rarely need to resolve more than one or two entries per import.
- Unresolved rows at **Done** time are skipped and not imported.

### Paste mode

- The left column lists the source config's transforms.
- The right column lists the target config's live objects with **OK** and drag-to-resolve support.
- Suggested matches (same name, same hierarchy depth) are marked **★** in green.
- **Map All by Order** — maps each source row to the target row at the same position. Use this when configs have the same structure but different names (e.g. pasting a 60-object rig onto a differently-named variant in one click).
- **Clear All** — removes all current mappings to start over.
- Unresolved entries are skipped on **Done**.

---

## Scene Operations

### Scene duplication

When you duplicate a scene that has capture data, the tool detects it and offers to copy the configs to the new scene:

```
'SceneName 1' is a duplicate of 'SceneName'.
Copy its N configuration(s) to the new scene?
```

Choose **Yes, copy configs** or **No thanks**.

### Scene deletion

When you delete a scene that has capture data, a dialog appears:

```
Deleting 'SceneName' will delete N captured configuration(s).
Delete all capture data for this scene too?
```

Choose **Yes, delete all** or **Keep capture data**.
Keep Capture data options is there for the recovery of accidental scene deletion.

---

## Options Menu

| Option | Description |
|---|---|
| Multi-Select Mode | Toggle bulk selection |
| Create New Category | Add a new category |
| Import from Export Queue | Import configs queued from another scene |
| Delete Empty Configs | Remove all configs with zero valid transforms |
| Delete Empty Categories | Remove all categories with no configs |
| Clean Orphaned Transforms (All Configs) | Remove missing object references from every config |
| New Config Position → Add at Top | New captures appear at the top of their category |
| New Config Position → Add at Bottom | New captures appear at the bottom *(default)* |
| Ping Scene Capture Data | Select the `SceneConfigLibrary` asset in the Project window |

---

## Maintenance

**Orange warning icon on a config** — one or more captured objects were added before the scene was saved. Save the scene to lock in their references.

**Red text on the info line** — all captured objects are missing from the scene (fully orphaned config).

**Yellow text on the info line** — some captured objects are missing.

**Asterisk (*) on a category header** — at least one config in that category has missing transforms.

**Remove Orphaned Transforms** — right-click a config card to remove only missing transforms from that config.

**Clean Orphaned Transforms (All Configs)** — available in the Options menu; cleans every config at once.

**Delete Empty Configs** — removes configs that have zero valid transforms remaining after cleanup.

---

## Real World Use Cases

### Furniture room kit

Capture chairs, cushions, and lamps as children of a `Room` empty. Move the entire room to a different position or duplicate it across the level. Apply the config in each instance and every piece snaps back to its correct position relative to that room's origin.

### Character rig poses

Capture all bones except the root. The character can be placed anywhere on the stage, facing any direction. Apply the pose config and the rig reconstructs correctly regardless of where the root is standing.

### Modular environment pieces

A `ShopFront` parent with sign, awning, crates, and barrels as children. Capture the arrangement without the parent. Duplicate the shop across the scene at different positions and rotations — one Apply restores the exact dressing in each instance.

### Stage lighting rig

A `LightingRig` parent with spotlights as children. Capture multiple lighting setups without the rig root. Move the rig to illuminate different areas of the level and switch between captured setups freely.

---

## The Power of Config Stacking

The tool respects Unity's hierarchy naturally, which means you can layer configs at every level of depth independently. Each object in your scene can simultaneously be a child in one config and a parent container for another.

Consider a room setup. The `Room` parent has three layout configs — `Layout_Open`, `Layout_Dining`, `Layout_Lounge` — each capturing where the table, chairs, and props sit within the room. Move the room anywhere on the level and apply any layout config: everything repositions correctly relative to the room's origin.

Now go one level deeper. The `Table` itself is a parent with its own portable configs — `Table_Clear`, `Table_Working`, `Table_Cluttered` — capturing the arrangement of books, cups, and objects sitting on top. These configs are independent of where the table is in the room. Switch the room to `Layout_Dining`, then apply `Table_Working` — both levels resolve correctly, simultaneously, without interfering with each other.

Go deeper still. On that table sits a `Chessboard` with its own configs — `Chess_Opening`, `Chess_Midgame`, `Chess_Endgame` — each capturing a different piece arrangement relative to the board. Move the board to any table in any room, apply a chess config, and the pieces snap to the correct positions on that board regardless of where it ended up.

| Level | Parent | Configs | Count |
|---|---|---|---|
| Room | Room (empty) | Layout_Open, Layout_Dining, Layout_Lounge | 3 |
| Table | Table mesh | Table_Clear, Table_Working, Table_Cluttered | 3 |
| Chessboard | Board mesh | Chess_Opening, Chess_Midgame, Chess_Endgame | 3 |
| **Total combinations** | | **3 × 3 × 3** | **27 distinct scene states** |

You are not managing one flat list of world positions. You are building a tree of composable states where each node in the hierarchy can independently switch between multiple captured arrangements. This scales to any depth: a city block containing buildings, buildings containing rooms, rooms containing furniture, furniture containing objects. Each level manages only its own immediate children — configs stay small, focused, and reusable across any instance of that parent anywhere in the scene.

## Runtime API
 
The runtime classes have **no UnityEditor dependencies** and are fully safe in builds.
 
There are two layers:
 
- **Core API** — the static classes `RuntimeCaptureApplier`, `RuntimeCaptureWriter`, and the `RuntimeApplyOptions` data class. These live in the `TransformSnapshotTool` namespace and are the reusable, general-purpose runtime API. Use these in your own scripts.
- **Example components** — `RuntimeApplyExample` and `RuntimeCaptureExample` are ready-made MonoBehaviours that demonstrate the core API. They are fully functional and can be used directly or copied as a starting point. `RuntimeCaptureExample` lives in the `TransformSnapshotTool.Example` namespace and contains demo-specific fields (ragdoll, furniture).
---
 
### RuntimeApplyExample
 
An example MonoBehaviour for applying editor-captured configs by name at runtime. Assign the `SceneConfigLibrary` asset in the Inspector, then call `Apply` from any script, button, or UnityEvent. It can also trigger configs from keyboard shortcuts.
 
**Inspector fields:**
 
| Field | Description |
|---|---|
| Scene Config Library | The `SceneConfigLibrary` asset for this scene (drag from Project window) |
| Configs | Array of `KeyedCaptureEntry` items — each pairs a config with a keyboard shortcut and a per-key Snap/Lerp transition. Right-click a config card in the tool window and choose **Feed Config** to populate an entry. |
 
**Apply API:**
 
Every `Apply` overload accepts an optional `lerp` argument. Pass `lerp: true` to interpolate smoothly using the entry's duration and easing curve; omit it (or pass `false`) to snap instantly.
 
```csharp
// Snap using the settings stored on the config
_applier.Apply("Idle");
 
// Lerp using the entry's inspector duration + easing curve
_applier.Apply("Idle", lerp: true);
 
// Explicit space mode (snap)
_applier.Apply("Idle", RuntimeApplyMode.LocalSpace);
 
// Explicit space mode (lerp)
_applier.Apply("Idle", RuntimeApplyMode.LocalSpace, lerp: true);
 
// Inline component toggles (snap)
_applier.Apply("Idle", position: true, rotation: true, scale: false);
 
// Inline component toggles (lerp)
_applier.Apply("Idle", position: true, rotation: true, scale: false, lerp: true);
 
// Explicit mode and component toggles — full override, always snaps
_applier.Apply("Idle", RuntimeApplyMode.WorldSpace, true, false, true);
 
// Cancel any lerp currently running on this component
_applier.StopLerp();
```
 
**Overload summary:**
 
| Overload | Mode | Components | Lerp |
|---|---|---|---|
| `Apply(name, lerp = false)` | from asset/entry | from asset/entry | optional |
| `Apply(name, mode, lerp = false)` | caller picks | from asset/entry | optional |
| `Apply(name, pos, rot, scl, lerp = false)` | from asset/entry | caller picks | optional |
| `Apply(name, mode, pos, rot, scl)` | caller picks | caller picks | always snaps |
 
**Other members:**
 
| Member | Description |
|---|---|
| `StopLerp()` | Cancels any lerp currently running on this component. |
| `IsLerping` | `bool` property — `true` while a lerp is in progress. |
 
> `RuntimeApplyExample` applies configs created in the editor tool and stored in a `SceneConfigLibrary`. For capturing snapshots during gameplay, use `RuntimeCaptureWriter` (or the `RuntimeCaptureExample` component).
 
---
 
### RuntimeCaptureExample
 
An example MonoBehaviour demonstrating runtime capture, apply, and JSON persistence. Snapshots live in memory and can optionally be persisted to JSON in `Application.persistentDataPath`. Namespace: `TransformSnapshotTool.Example`.
 
This component includes demo-specific fields (a ragdoll-recovery example and a furniture-placer example) to show realistic usage. For your own projects, you can use it directly, copy the parts you need, or call the core `RuntimeCaptureWriter` / `RuntimeCaptureApplier` API instead.
 
**Inspector fields:**
 
| Field | Description |
|---|---|
| Ragdoll Bones | *(demo)* Bone GameObjects used by the ragdoll-recovery example |
| Furniture | *(demo)* Furniture GameObjects used by the furniture-placer example |
 
**Capture API:**
 
```csharp
// Snapshot specific objects to memory (no disk write)
capturer.Capture("MyPose", objA, objB, objC);
capturer.Capture("MyPose", myGameObjectArray);
```
 
**Apply API:**
 
```csharp
// Restore with default options
capturer.Apply("MyPose");
 
// Restore with explicit space mode
capturer.Apply("MyPose", RuntimeApplyMode.LocalSpace);
 
// Restore with inline component toggles
capturer.Apply("MyPose", position: true, rotation: false, scale: false);
 
// Restore with explicit mode and component toggles
capturer.Apply("MyPose", RuntimeApplyMode.LocalSpace, false, true, false);
 
// Restore with fully custom options
capturer.Apply("MyPose", new RuntimeApplyOptions
{
    mode          = RuntimeApplyMode.LocalSpace,
    applyPosition = false,
    applyRotation = true,
    applyScale    = false,
});
```
 
**Persistence API:**
 
```csharp
// Write a memory snapshot to JSON
capturer.Save("MyPose");
 
// Load a JSON snapshot into memory (does not apply it)
capturer.Load("MyPose");
 
// Load from JSON and apply immediately
capturer.LoadAndApply("MyPose");
 
// Delete a JSON file from disk
capturer.Delete("MyPose");
 
// Query
bool inMemory = capturer.HasSnapshot("MyPose");
bool onDisk   = capturer.HasSave("MyPose");
```
 
> Snapshots are temporary by default — `Capture` stores to memory only. Call `Save` explicitly to persist to disk.
 
---
 
### RuntimeCaptureApplier
 
Static class. Instantly applies a `CaptureConfiguration` to live scene objects. Resolves GameObjects by stored hierarchy path, with a name-fallback scoring strategy if the path walk fails. This is the core apply API — used internally by the example components.
 
```csharp
// Apply all objects in a config (options is optional; null uses Default)
RuntimeCaptureApplier.ApplyConfiguration(config);
 
// Apply with custom options
RuntimeCaptureApplier.ApplyConfiguration(config, new RuntimeApplyOptions
{
    mode          = RuntimeApplyMode.WorldSpace,
    applyPosition = true,
    applyRotation = false,
    applyScale    = false,
});
 
// Apply only objects under a specific subtree
RuntimeCaptureApplier.ApplyToSubtree(config, rootTransform);
```
 
Both methods return `int` — the number of objects successfully updated.
 
---
 
### RuntimeApplyOptions
 
Data class controlling coordinate space and which transform components are applied.
 
```csharp
// Built-in presets
RuntimeApplyOptions.Default             // Auto mode, all components on
RuntimeApplyOptions.PositionOnly        // position only
RuntimeApplyOptions.RotationOnly        // rotation only
RuntimeApplyOptions.ScaleOnly           // scale only
RuntimeApplyOptions.PositionAndRotation // position + rotation, no scale
RuntimeApplyOptions.WorldSpace          // world space, all components
RuntimeApplyOptions.LocalSpace          // local space, all components
 
// Custom
var options = new RuntimeApplyOptions
{
    mode               = RuntimeApplyMode.Auto,
    applyPosition      = true,
    applyRotation      = true,
    applyScale         = false,
    includeObjectPaths = new[] { "Root/Arm/Hand" } // whitelist; null = apply all
};
```
 
**RuntimeApplyMode values:**
 
| Value | Behaviour |
|---|---|
| `Auto` | Root objects use world space, children use local space *(recommended)* |
| `WorldSpace` | Always apply world position, rotation, and lossy scale |
| `LocalSpace` | Always apply local position, rotation, and local scale |
 
---
 
### RuntimeCaptureWriter
 
Static class for capturing live transforms and persisting them as JSON. This is the core capture + persistence API.
 
**Save location:**
```
Application.persistentDataPath/TransformSnapshotTool/{configName}.json
 
Windows : %AppData%\..\LocalLow\<Company>\<Product>\TransformSnapshotTool\
macOS   : ~/Library/Application Support/<Company>/<Product>/TransformSnapshotTool/
Android : /data/data/<packagename>/files/TransformSnapshotTool/
iOS     : <AppHome>/Documents/TransformSnapshotTool/
```
 
```csharp
// Capture objects to memory (no disk write)
CaptureConfiguration capture = RuntimeCaptureWriter.Capture("MyPose", objectArray);
 
// Capture and immediately save to JSON
RuntimeCaptureWriter.CaptureAndSave("MyPose", objectArray);
 
// Save an in-memory config to JSON
RuntimeCaptureWriter.Save(capture);
 
// Load a config from JSON by name
CaptureConfiguration loaded = RuntimeCaptureWriter.Load("MyPose");
 
// Load every saved JSON config from disk
List<CaptureConfiguration> all = RuntimeCaptureWriter.LoadAll();
 
// Apply a loaded config
RuntimeCaptureApplier.ApplyConfiguration(loaded);
 
// Query
bool         exists = RuntimeCaptureWriter.Exists("MyPose");
List<string> names  = RuntimeCaptureWriter.GetSavedConfigNames();
 
// Delete
RuntimeCaptureWriter.Delete("MyPose");
```
 
**`MaxSavedConfigs`** — optional static field. When set above `0`, the oldest JSON files are deleted automatically once the saved count exceeds the limit. Defaults to `0` (no limit).
 
```csharp
RuntimeCaptureWriter.MaxSavedConfigs = 20; // keep only the 20 most recent
```
 
> In the Editor, JSON files survive Play Mode exit, making them useful for iterating on runtime-captured poses between sessions.

## Data Storage

Config data is stored as ScriptableObject assets inside your project, organised by scene:

```
Assets/
  TransformSnapshotTool/
    CaptureData/
      {SceneName}_{SceneGUID}/
        {SceneName}_ConfigLibrary.asset   ← SceneConfigLibrary (index + category list)
        {ConfigName}_{ShortGUID}.asset    ← CaptureConfigurationAsset (one per config)
```

Each scene has its own folder. The `ConfigLibrary` asset holds the ordered list of all configs and categories for that scene.

These files are plain Unity assets — safe to commit to version control and will survive reloads, reimports, and Unity upgrades.

> Never rename or move these files manually — always use the tool's **Rename** action to keep internal references intact.

---

## FAQ


**I deleted an object but the config still references it.**

Configs retain transforms for deleted objects so they can be restored if the user performs an Undo operation. To permanently remove these orphaned transforms, use **Remove Orphaned Transforms** on the config card context menu, or Clean Orphaned Transforms (All Configs) from the Options menu.


**Can I share configs between team members?**

Yes — commit the `Assets/TransformSnapshotTool/CaptureData/` folder to source control. All team members on the same project will see the same configs automatically.


**Can I use the runtime classes in a build?**

Yes — `RuntimeCaptureApplier`, `RuntimeCaptureWriter`, `RuntimeCapturer`, and `SimpleRuntimeApplier` have no UnityEditor dependencies and are fully safe in builds. Only the editor window and editor-side scripts are Editor-only.


**What is the difference between SimpleRuntimeApplier and RuntimeCapturer?**

`SimpleRuntimeApplier` applies configs created in the editor tool. `RuntimeCapturer` captures and restores transforms created at runtime during gameplay. They use separate storage and cannot conflict with each other.


**The import or paste dialog shows all rows unresolved.**

The object names in the source config do not match those in the current scene. Use the ObjectField on each active row to drag the correct object from the Hierarchy, or use **Map All** button in paste mode if the two configs have the same structure but different names.



**Can I apply only part of a config?**

Yes — use **Selected Only** mode to apply only the objects currently selected in the Hierarchy that also appear in the config. You can also toggle Position, Rotation, and Scale independently to apply partial transforms.


 
