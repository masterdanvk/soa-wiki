# Page Templates

Copy-paste skeletons. Replace everything in `<angle brackets>`, delete unused sections.

## Quest page (public)

Create as `docs/quests/<slug>.md`, add a row to the [Quest Index](../quests/index.md), and add it to `nav` in `mkdocs.yml`.

```markdown
# <Quest Name>

| | |
|---|---|
| **Giver** | <NPC>, <location> |
| **Village** | <Leaf/Sand/Mist/Neutral> |
| **Type** | <Kill/Collect/Gather/PvP/...> |
| **Requirements** | <Level X+ / completes "<Quest>" / reputation D+> |
| **Repeatable** | <No / Yes — Xh cooldown> |
| **Time limit** | <None / X minutes> |

## Story

<1-2 paragraphs of flavor — why the giver needs this done.>

## Objectives

1. <Do the thing> (<count>)
2. Return to <giver>.

## Rewards

- <XP>
- <ryo>
- <item, if any>

## Walkthrough & tips

<Where to go, what to watch for, minimap pin behavior.>

## Lore notes

<How this ties into the wider story. Link related pages.>

---

*Developer data for this quest lives in the [Quest Data Registry](../dev/quest-data-registry.md).*
```

## Quest registry entry (dev)

Append to [Quest Data Registry](quest-data-registry.md) under **Registry entries**, and add the quest to the chain map if it's part of an arc.

```markdown
### <template_id> — <Quest Name>

| Field | Value |
|---|---|
| **Template id** | `<q_arc_short>` |
| **Public page** | [<Quest Name>](../quests/<slug>.md) |
| **Giver NPC** | <name> (`npc_template_id`: `<id>`) |
| **Objective** | `<objective>`, `target_kind=<kind>`, `target=<target>`, `goal=<n>` |
| **Collect item** | `<item_name>`, `drop_chance=<n>%`, icon `<file>:<state>` *(collect only)* |
| **Lifecycle** | `repeatable=<0/1>`, `cooldown_hours=<n>`, `time_limit_min=<n>` |
| **Rewards** | `reward_xp=<n>`, `reward_ryo=<n>`, `reward_loot=<spec or "">` |
| **Prereqs** | `prereq_min_level=<n>`, `prereq_max_level=<n>`, `prereq_rep=<grade>`, `prereq_quest=<id>` |
| **Flags set/read** | `prereq_flags=<...>`, `prereq_flags_not=<...>` |
| **Pins** | `pin_wx/wy=<x,y>`, `turnin_pin=<0/1>` |
| **Dialogue nodes** | Give: <node/choice>; Turn-in: <node/choice>; extras: <flavor branches> |
| **Status** | <🟢 Live / 🔍 In review / 📝 Draft> |
```

## NPC page (public)

Create as `docs/npcs/<slug>.md`, add a row to the [NPC Index](../npcs/index.md).

```markdown
# <NPC Name>

| | |
|---|---|
| **Village** | <Leaf/Sand/Mist/Neutral> |
| **Location** | <map / landmark> |
| **Role** | <Quest giver / Merchant / Story> |

## Description

<Appearance, personality, one-paragraph backstory consistent with the Canon Policy.>

## Quests

| Quest | Role |
|---|---|
| [<Quest>](../quests/<slug>.md) | <Giver / Turn-in> |

## Dev notes

`npc_template_id`: `<id>` · Author: <who> · Package: <issue/PR link>
<Encounter links, dialogue tree summary, flags this NPC's dialogue reads.>
```

## Item entry

Add a row to the [Item Index](../items/index.md); give complex artifacts their own page using the same header-table pattern.

```markdown
| <Item Name> | <slot/quest> | <theme/source> | <notes> |
```

## Lore article

Create under `docs/lore/`, link it from the [Lore Overview](../lore/index.md).

```markdown
# <Topic>

<Body. Every claim must be consistent with the Canon Policy and the Timeline.
Link generously — orphan lore pages get lost.>

## Related

- [<Related page>](<path>)
```
