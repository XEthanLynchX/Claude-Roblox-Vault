# Hotbar inventory

Built 2026-10-19. Server-authoritative 3-slot hotbar.

## How it plays
- **1 / 2 / 3** (or the mouse wheel, gamepad D-pad left/right) pick a slot. **G** (D-pad down) drops what you hold.
- Items lying in the world have a **Pick up** prompt. Picking up puts it in the first free slot and in your hand.
- **The flashlight takes a slot** and starts in slot 1. It only shines while it's the selected item. **F** with something else in hand takes the flashlight out (switched on).
- One of each "Unique" item (flashlight, wing key). Full hands: "Drop something first [G]".
- Trash bags and the wing key are hotbar items (no more Carry in hand).
- New wing: everyone is reset to just the flashlight. Taken/left: items go home (bag back in its can, key back on the shelf, tools to their spot in the custodian room).

## Code
- `src/shared/Items.luau`: item definitions (name, how it's held: `Hold` relative to RightHand, `View` relative to the camera at `ViewScale`, `Floor` when dropped). `Items.make(kind)` builds the model (from `ReplicatedStorage.Assets`, except the light tube, which is built in code).
- `src/server/Services/InventoryService.luau`: slots, Give/Take/Has/Equipped, `Spawn` (world item with pickup prompt), `Drop`, `OnLost(kind, fn)`, `OnChanged(fn)`, `LoseItems`, `NewWing`, remote `Inventory`.
- Replicates through player attributes: `Inv1..Inv3`, `Equipped`, `EquippedKind`.
- `src/client/Controllers/HotbarController.luau`: the UI (3D previews in ViewportFrames), input, the first-person view model of the held item, and tool hold times on prompts tagged `ToolHold` (vacuum fast, broom slow).
- `FlashlightService` hides the hand flashlight and kills its beam unless `EquippedKind == "Flashlight"`.
- Dev command: `give <Kind>` (Broom, Vacuum, Plunger, Bulb, BatteryBox, Flashlight).

## Item kinds
Flashlight, Broom, Vacuum, Plunger, Bulb (fluorescent tube), BatteryBox, TrashBag, WingKey.

## Not verified yet
- How the held items look in third person and in your own view (View/Hold offsets are first guesses; Studio screenshots don't work during a playtest). Tune in `Items.Defs`.
