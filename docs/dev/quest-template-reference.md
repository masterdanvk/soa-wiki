# Quest Template Reference

A quest is a `datum/quest_template` — pure data in the global `quest_registry`, persisted to `savefiles/quest_templates.json`. These are the fields as defined in `code/game/npc/quest_system.dm`.

## Identity & text

| Field | Default | Meaning |
|---|---|---|
| `id` | `""` | Unique key, e.g. `q_zetsu_cull`. **No `;` allowed.** Never reuse or rename a live id — saves reference it. |
| `name` | `"New Quest"` | Player-visible name. Also the key used for completion tracking, so keep it unique too. |
| `desc` | `""` | Journal flavor text shown in the tracker overlay. |

## Objective

| Field | Default | Meaning |
|---|---|---|
| `objective` | `"kill_npc"` | One of the objective types — see [Objectives & Event Hooks](objectives-and-events.md). |
| `target_kind` | `"name"` | For `kill_npc`/`collect`: `any` \| `category` \| `type` \| `name` \| `npc_id` \| `icon`. |
| `target` | `""` | NPC name / maker id / icon_name / category; for `kill_player`/`knockout`: village code (`""` = any enemy); for `gather`: `herb` or `ore`; for `use_skill`: skill name; for `visit_area`: map name; for `complete_mission`: rank. |
| `goal` | `1` | Required count. Amount-based objectives (ryo, damage, healing…) treat this as a running total. |

## Collect drops (collect objective only)

The **quest** rolls the drop, not the NPC — the NPC knows nothing about the item.

| Field | Default | Meaning |
|---|---|---|
| `drop_chance` | `30` | % roll per qualifying kill while the quest is active and unmet. Drops appear as gold, killer-private ground items. |
| `item_name` | `""` | Display name of the quest item. |
| `item_icon` | `"gui"` | Glyph icon file key: `gui` (`icons/gui.dmi`) or `artifact` (`icons_new/artifacts.dmi`). |
| `item_state` | `""` | Glyph icon_state. Blank = beam/circle drop visual only. |

## Lifecycle

| Field | Default | Meaning |
|---|---|---|
| `repeatable` | `0` | 1 = can be retaken after cooldown. |
| `cooldown_hours` | `1` | Hours before a repeatable quest can be retaken (tracked by `world.realtime` per character). |
| `time_limit_min` | `0` | Minutes to finish once taken. **0 = no limit — the quest can never fail.** |

## Rewards (granted on turn-in)

| Field | Default | Meaning |
|---|---|---|
| `reward_xp` | `0` | Flat XP. |
| `reward_ryo` | `0` | Flat ryo. |
| `reward_loot` | `""` | Optional loot spec: `roll|<rarity>|<luck>` (random artifact roll) or `item|a:<blueprint_id>|<rarity>` (specific artifact). The NPC Maker's reward helper composes this string for you. |

## Minimap pins

| Field | Default | Meaning |
|---|---|---|
| `pin_wx`, `pin_wy` | `0,0` | Optional objective-area pin in world coords (`0,0` = none). |
| `turnin_pin` | `1` | Pin the giver's location when the quest is ready to turn in. Giver location is captured **at assignment**. |

## Prerequisites

All prereqs gate `can_take_npc_quest()` — the **single proc** that hides the giver's "!" marker, hides the dialogue Give choice, AND validates the server-side pick. They stay consistent automatically; never gate a quest anywhere else.

| Field | Default | Meaning |
|---|---|---|
| `prereq_min_level` | `0` | Minimum blevel (0 = none). |
| `prereq_max_level` | `0` | Maximum blevel (0 = no cap; use for newbie-only quests). |
| `prereq_rep` | `""` | Minimum reputation grade: `""` none, or `D`/`C`/`B`/`A`/`S`. |
| `prereq_quest` | `""` | Template id that must be **completed** first (builds chains; chains are followed transitively for package exports). |
| `prereq_flags` | `""` | Comma-separated PVEflags the player must ALL have. |
| `prereq_flags_not` | `""` | Comma-separated PVEflags that BLOCK the quest if present. |

!!! warning "Flags are global"
    PVEflags are a shared, game-wide namespace. Check the [Quest Data Registry](quest-data-registry.md) before naming a new flag, and prefix with your arc (`whitehunt_...`).

## Quest states

A player's quest instance moves through: **available** (prereqs met, giver shows "!") → **active** (progress counting) → **ready** (goal met, giver shows "?", turn-in pin appears) → **done** (rewards granted) → **cooldown** (repeatable only). Dialogue choices can require any of these states — see [Dialogue & NPC Maker](dialogue-and-npc-maker.md).

## Persistence notes

- Templates live in `savefiles/quest_templates.json`; baked `.dm` exports self-register at boot, but **JSON entries with the same id win** (live workshop edits override baked code).
- Player progress rides the existing quest save (`lst[9]/[10]`); quest item identity persists via the `S[8]` side-channel and is re-derived from the live template on load — so renaming an item on the template retroactively renames held items.
- Reward gating for repeatables uses the `quests_complete` status value as the retake timestamp.
