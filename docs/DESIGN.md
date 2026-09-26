# Sanctify Tower Defense — Game Design

Items marked **Decided** are established. Items marked **Idea — don't implement unless asked**
are future possibilities, not tasks. Numbers (costs, health, damage, wave contents) are balanced
through playtesting and live in the data modules.

---

## 1. Overview

**Decided**

The player defends a sacred forest sanctuary near a mountain and waterfall from waves of invading
creatures. The structure is inspired by games like Bloons TD6: place troops, survive waves, earn
currency, upgrade. BTD6 is only a structural reference. Sanctify must not reproduce another game's
characters, art, maps, names, or exact mechanics.

## 2. World and Visual Style

**Decided**

- Setting: dense forest, mountains, a large waterfall, streams and pools, moss-covered rocks,
  vines, ancient ruins, shrines, statues, wooden bridges.
- The sanctuary feels peaceful and mystical before enemies arrive; the contrast with the invasion
  matters.
- Style: stylized fantasy, nature-focused, colorful but not overly cartoonish, readable from a
  tower-defense camera, Roblox-appropriate.
- Troops and enemies have recognizable silhouettes. Upgraded troops look visibly stronger.
- Prefer simple, maintainable Roblox-compatible models.

## 3. Core Loop

**Decided**

```text
Wave starts → enemies spawn and follow the path → player places troops →
troops attack automatically → enemies die → player earns Sanctity →
player places or upgrades troops → next, harder wave
```

- The sanctuary has health (starting value in GameConfig, e.g. 100).
- Enemies that reach the end damage it; each enemy type has its own sanctuary damage
  (weak ~1, strong 5–20, bosses much more).
- Health reaches 0 → "SANCTUARY FALLEN" → loss.
- **Prototype win condition:** survive wave 10.

## 4. Camera

**Decided**

Top-down tower-defense camera: rotate, zoom, clearly show the path and troop ranges, select troops.

## 5. Currency

**Decided**

- Currency: **Sanctity** (placeholder name).
- Earned by defeating enemies; spent on placing and upgrading troops.
- Selling refunds 70% of the total Sanctity spent on a troop.

## 6. Troops

Roster: Monk, Stone Guardian, Elf, Owl Sky Warden, Jungle Cat, Armadillo Ronin, Ant Soldier.
Each must have a distinct role — not the same tower with different stats.

### Monk
**Decided:** spiritual ranged attacker with possible support role. Sacred warrior theme.
Visibly grows more powerful through upgrades (robes → prayer beads → glowing markings → aura).
**Idea — don't implement unless asked:** spiritual projectiles, holy damage, enemy debuffs,
buffing nearby troops, cleansing effects, enlightened final form.

### Stone Guardian
**Decided:** heavy, slow, high-damage defensive troop. Ancient stone guardian statue. Grows larger
and more elaborate (runes, armor, crystals) through upgrades.
**Idea — don't implement unless asked:** stuns, area attacks, stone barriers, Earthquake ability.

### Elf
**Decided:** fast ranged attacker with good range. Forest archer with a bow; visual upgrades
(better bow, forest armor, magical arrows).
**Idea — don't implement unless asked:** piercing, poison, magic arrows, multi-shot, branching paths.

### Owl Sky Warden
**Decided:** long-range aerial specialist, strong against flying enemies. Giant mystical owl with
a distinctive silhouette.
**Idea — don't implement unless asked:** revealing hidden enemies, wind attacks, dive attacks,
Storm Dive ability.

### Jungle Cat
**Decided:** fast, aggressive close-range attacker. Jungle predator.
**Idea — don't implement unless asked:** crits, bleed, leap attacks, Predator ability.

### Armadillo Ronin
**Decided:** defensive melee troop. Armadillo wandering warrior with a sword; gains armor, helmet,
larger sword, banners through upgrades.
**Idea — don't implement unless asked:** blocking, rolling attacks, knockback, spin attacks,
Rolling Slash ability.

### Ant Soldier
**Decided:** cheap, fast-attacking, swarm-style troop — a different strategy from expensive single
troops.
**Idea — don't implement unless asked:** summoning extra ants, temporary swarms, multiple soldiers
per placement.

### Flying enemy targeting
**Not decided yet.** Which troops can hit flying enemies is decided in Phase 2, when the Vulture
Witch is added. Owl Sky Warden will be one of them.

## 7. Upgrades

**Decided**

- Every troop has upgrades that change more than numbers at higher tiers: model, armor, weapon,
  size, effects, animations, projectiles.
- A player should recognize an upgraded troop by sight.
- Build one complete upgrade path per troop first.

**Idea — don't implement unless asked:** multiple upgrade paths per troop
(e.g. Elf: Ranger / Poisoner / Spirit Archer).

## 8. Enemies

Roster: Goblin, Vulture Witch, Obsidian Mole, Mandrill Mage, Yaga (boss).
Each has health, speed, sanctuary damage, reward, and a visual identity. Variants communicate
strength through appearance, not just health bars.

### Goblin
**Decided:** basic, weak, frequent enemy. Variants:
- Basic Goblin
- Copper Goblin (copper armor, more health)
- Chain Goblin (chain armor, tougher)
- Heavy Goblin (large, slow, durable)

**Idea — don't implement unless asked:** Goblin Warrior (armed variant).

### Vulture Witch
**Decided:** flying enemy; vulture + witch + dark magic. Only certain troops can target it.
**Idea — don't implement unless asked:** curses, troop debuffs, variants (Hooded, Dark, Elder).

### Obsidian Mole
**Decided:** heavily armored, slow, high-health enemy with dark obsidian/volcanic body elements.
**Idea — don't implement unless asked:** burrowing, temporary untargetability.

### Mandrill Mage
**Decided:** magical enemy, visually distinct; should push players to prioritize targets.
**Idea — don't implement unless asked:** buffing enemies, slowing or attacking troops, summoning,
magical shields.

### Yaga (boss)
**Decided:** major boss inspired by Baba Yaga mythology — an original interpretation, not a copy of
any modern character design. Much stronger than normal enemies; should feel like an event.
**Idea — don't implement unless asked:** multiple phases, summoning, disabling troops, area magic.

## 9. Waves

**Decided**

- Wave compositions are defined in `WaveData`.
- Early waves: basic Goblins. Later: variants, flying, armored, magical enemies, mixed groups, bosses.
- Exact counts are balanced by playtesting. Rough shape for reference only:

```text
Wave 1   10 Basic Goblins
Wave 5   15 Goblins, 3 Copper Goblins
Wave 10  20 Goblins, 5 Chain Goblins (+ Vulture Witches once added)
```

## 10. Path and Map

**Decided**

- Enemies follow a predefined, readable path with curves and several strategic placement areas.
- Conceptual route: mountain entrance → forest → waterfall → ancient ruins → sanctuary.
  Not a final layout.

## 11. Placement and Troop UI

**Decided**

- Free placement; invalid on/near the path, in blocked terrain, inside other troops, outside the map.
- Placement preview shows range and validity; player can cancel.
- Selecting a troop shows: name, level, damage, range, attack speed, upgrade cost, sell value,
  Upgrade and Sell buttons.
- Main HUD: wave, sanctuary health, Sanctity, troop bar. Keep it uncluttered.

**Idea — don't implement unless asked:** targeting modes (First, Last, Strong, Close, Random).

## 12. Abilities

**Idea — don't implement unless asked**

Active abilities with cooldowns: Monk "Sanctify", Stone Guardian "Earthquake", Elf "Rain of Arrows",
Owl "Storm Dive", Jungle Cat "Predator", Armadillo Ronin "Rolling Slash", Ant Soldier "Swarm".

## 13. Modes, Multiplayer, Audio, Progression, Monetization

**Decided:** first mode is Classic. Single-player first.

**Idea — don't implement unless asked:**
- Modes: Endless, Challenge, Boss Rush, Hard Mode, Daily Challenges.
- Co-op for 1–4 players (don't complicate the early architecture for it).
- Audio: forest ambience, waterfall, birds, attack/upgrade/wave sounds, boss music,
  intensity rising with danger.
- Permanent progression: unlocks, skins, sanctuary customization, levels, achievements, maps.
- Monetization: cosmetics and optional convenience only, never pay-to-win.

## 14. Development Phases

### Phase 1 — Prototype (current)
Placeholder map and path, Goblin, server-side movement, sanctuary health, Monk, free placement,
targeting and attacks, enemy health and death, Sanctity rewards, one basic Monk upgrade,
selling, waves 1–10, win/lose screens, basic HUD. Placeholder assets are fine.

### Phase 2 — Core Game
Remaining troops and enemies, Goblin variants, flying-target rules, fuller upgrade paths,
better UI, balancing.

### Phase 3 — Visual Identity
Environment, models, upgrade visuals, animations, VFX, audio.

### Phase 4 — Advanced Gameplay
Yaga and bosses, advanced enemy/troop mechanics, multiple upgrade paths, abilities, modes, co-op.

### Phase 5 — Polish
Performance, balancing, tutorials, menus, error handling.

## 15. Core Principle

Sanctify Tower Defense must feel like its own game: its own sanctuary world, creatures, upgrade
transformations, atmosphere, and progression. Never sacrifice a working gameplay foundation for
extra features.
