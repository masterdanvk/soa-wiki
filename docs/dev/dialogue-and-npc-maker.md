# Dialogue & NPC Maker Workflow

## Dialogue is the only quest interface

Quests are **given and turned in exclusively through NPC dialogue choices**. There is no quest board. A `dialogue_choice` carries four quest fields:

| Field | Effect |
|---|---|
| `give_quest` | Choosing this gives the quest (only shown/valid if `can_take_npc_quest()` passes) |
| `complete_quest` | Choosing this turns the quest in (auto-hidden until the quest is READY) |
| `require_quest` | Choice only appears if the player has this quest… |
| `require_quest_state` | …in this state: `none` / `active` / `ready` / `done` / `cooldown`. Empty = active-or-ready |

The server **re-validates on pick** — a forged client pick is harmless. Prereq gating, the "!"/"?" overhead markers, and the Give choice all run through the same `can_take_npc_quest()` proc, so they can't drift apart.

### Dialogue patterns

- **Flavor branch while active:** a choice with `require_quest` + state `active` lets the giver comment on your progress.
- **Chains:** quest B's `prereq_quest = q_a`; the giver's dialogue offers B behind `require_quest = q_a`, state `done`.
- **Multiple givers:** any NPC's dialogue can `complete_quest` — turn-in doesn't have to be the giver (but the turn-in pin points at the giver, so note it on the wiki page if you diverge).

## Overhead markers & pins

- "!" (quest available) / "?" (ready to turn in) render via the quest-image engine; the NPC's dialogue is scanned and cached at build time (`quest_marker_register`).
- Markers respect faction/quest gates but **NOT affection/stat gates** — if your choice is gated on affection, the "!" may show before the choice does.
- Cooldown expiry doesn't push a marker refresh — the "!" reappears on the next natural refresh (login, quest add/remove, ready transition).
- Minimap: gold pins for the objective area (`pin_wx/wy`) and the giver on ready; giver location is captured at assignment.

## The authoring & review cycle

The NPC Maker is **deliberately ungated** — invited players author content. Review happens through packages:

1. **Author** builds NPC + dialogue + quests (Quests tab) + encounters in the NPC Maker.
2. **Export Package** (editor bottom bar) bundles the NPC entry, linked encounters, and every dialogue-referenced quest (including transitive `prereq_quest` chains) into one `goa_npc_package` JSON, delivered as a file download.
3. Author attaches the package JSON to their GitHub Issue / PR for review.
4. **Reviewer** uses **Import Package** (main-page toolbar) to load it into their own workshop: NPCs/encounters re-id on collision; quest ids already live in the registry are **kept** (live edits win) and reported.
5. After approval, the reviewer's normal **Export Code** bakes compiled `.dm` (quests self-register at boot; JSON same-id entries still override, so live tuning stays possible).
6. Author (or reviewer) writes the wiki pages: public quest page + [Quest Data Registry](quest-data-registry.md) entry.

!!! warning "Known sharp edges"
    - Compiled **encounter** exports do NOT self-register (`register_encounter()` is never called) — encounters must live in the JSON registry.
    - Quest ids must contain no `;` (the maker sanitizes, but don't fight it).
    - Generated-`.dm` string escaping is handled by the exporter — don't hand-edit baked quest strings containing `[`.

## Editor helpers worth knowing

- **Glyph picker:** file dropdown (gui/artifacts/weapons/scrolls) + clickable thumbnail grid for quest-item art.
- **Reward helper:** structured picker (None / Roll with rarity+luck / Specific item with rarity override + blueprint grid) that composes the `reward_loot` spec string for you.
- **Target picker:** for `type` matching, a curated list of viable compiled leaf types (shadow/maker/clone types excluded).
