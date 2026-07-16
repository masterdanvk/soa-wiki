# Objectives & Event Hooks

Every objective type, what its `target` means, and the game event that drives its progress. Amount-based objectives (marked *total*) count toward `goal` as a running sum with throttled HUD refresh; the rest count discrete occurrences.

## Objective types

| Objective | `target` means | Progress source (event) | Notes |
|---|---|---|---|
| `kill_npc` | via `target_kind` (see below) | `EVENT_ON_KILLED` | Fires inside the anti-farm gate — farmed spawns don't count |
| `collect` | via `target_kind` | `EVENT_ON_KILLED` + quest's own drop roll | Gold killer-private drops; crumble once goal met |
| `kill_player` | village code (`""` = any enemy) | `EVENT_ON_PVP_KILL` | 30-min per-victim cooldown (anti kill-trading) |
| `knockout` | village code | `EVENT_ON_PVP_KO` | Attacker-side; same anti-trade cooldown, `ko_`-prefixed keys |
| `skirmish_win` | — | `EVENT_ON_SKIRMISH_WIN` | |
| `arena_win` | — | `EVENT_ON_ARENA_WIN` | Fires for 1v1 and both 2v2 blocks |
| `win_encounter` | encounter id | `EVENT_ON_ENCOUNTER_WIN` | Encounter id threaded through the shadow-realm start |
| `complete_mission` | mission rank filter | `EVENT_ON_MISSION_COMPLETE` | |
| `tower_waves` | — | `EVENT_ON_TOWER_WAVE` | Wave ≥ 2 = prior wave cleared; fired per player |
| `bounty_collect` *(total)* | — | `EVENT_ON_BOUNTY_COLLECTED` | Direct payout + bounty-hunter turn-in |
| `visit_area` | map name | `EVENT_ON_PLAYER_ENTERED_MAP` | Matches `mapinfo.map_name` |
| `capture` | — | `EVENT_ON_LAND_CAPTURE_ENDED` | Fires on the capturer |
| `gather` | `herb` or `ore` | `EVENT_ON_GATHERED` | Quest holders can pick herbs outside faction missions |
| `earn_ryo` *(total)* | — | `EVENT_ON_GAINED_RYO` | Hooks the actual money credit |
| `earn_fp` *(total)* | — | `EVENT_ON_GAINED_FP` | Faction points |
| `deal_damage` *(total)* | — | `EVENT_ON_DAMAGE_DONE` | |
| `use_skill` | skill name (`""` = any) | `EVENT_ON_USED_SKILL` | |
| `heal_allies` *(total)* | — | `EVENT_ON_HEAL` | Healer-side; self-heals excluded |
| `assists` | — | `EVENT_ON_ASSIST` | |

## NPC target matching (`kill_npc` / `collect`)

`target_kind` controls how a killed NPC is matched:

| `target_kind` | Matches on |
|---|---|
| `any` | Any NPC kill |
| `category` | See category table below |
| `type` | A compiled leaf type (picked in the editor; excludes shadow/maker/clone types) |
| `name` | Exact NPC name |
| `npc_id` | NPC Maker template id (`npc_template_id`, stamped on all maker spawn paths) |
| `icon` | NPC `icon_name` |

### Categories

| Category | Matching rule |
|---|---|
| `zetsu` | By icon_name |
| `bandit` | Types + name |
| `samurai` | **Name only** (no samurai type exists in code) |
| `<village>_ninja` | Checks both `faction_village` maker codes AND `locationdisc` full names. No compiled Rock/Cloud guard types exist — only maker NPCs |

## Gotchas for content authors

- **`EVENT_ON_KNOCKED_OUT` is a trap** — its string collides with `EVENT_ON_ATTACK`. KO quests use the dedicated `EVENT_ON_PVP_KO`. Don't "fix" this.
- Kill/KO **credit follows the killer** — party members and mercs without kill credit get no collect rolls (matches existing loot behavior).
- Anti-trade cooldowns are **per-victim mkey**, in the event handler, 30 minutes.
- Event defines live in `__define/datum/#event_listener.dm` — a `#`-prefixed file; if you must edit it, do so via shell tools, not editor tools (known cache-desync issue with `#` filenames).
