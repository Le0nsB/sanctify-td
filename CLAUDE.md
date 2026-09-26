# Sanctify Tower Defense — AI Development Guide

Roblox tower defense game set in a fantasy forest sanctuary. Built with Rojo.
Full game design: `docs/DESIGN.md`. Read it when a task involves troops, enemies,
waves, upgrades, abilities, the map, or visuals. Only implement items marked
**Decided** or explicitly requested. Items marked **Idea** are never tasks.

## Current Status
**Phase:** 1 — Prototype
**Done:** nothing yet
**Current task:** enemy path + server-side Goblin movement
**Next:** sanctuary damage → Monk placement → targeting/attacks → enemy death + Sanctity →
Monk upgrade → selling → waves 1–10 → win/lose screens

Update this section at the end of every completed task. Don't start a later phase
unless the current one works or the user asks.

## Decisions
Don't change these without discussing it with the user first.

- **Currency:** "Sanctity" (placeholder name).
- **Win/lose (prototype):** win by surviving wave 10; lose when sanctuary health hits 0.
- **Placement:** free placement. Server rejects spots on or too close to the path, in
  blocked terrain, overlapping other troops, or outside the map. Client shows a preview
  and range but the server decides.
- **Selling:** refunds 70% of total Sanctity spent on the troop. Included in Phase 1.
- **Enemy simulation:** enemies are server-side data (id, type, health, path distance,
  speed, status effects), updated by ONE loop in EnemyManager. No physics, Humanoids,
  or PathfindingService for enemies.
- **Enemy replication:** the server fires events, not positions:
  `EnemySpawned(id, type, speed, spawnTime)`, `EnemyUpdated(id, changes)` for things
  like slows or knockback, and `EnemyRemoved(id, reason)`. Each client computes enemy
  positions along the path itself and renders the models. The server's copy is the
  truth for targeting and damage.
- **Game state for UI:** wave number and sanctuary health are Attributes on
  `Workspace`; Sanctity is an Attribute on each Player. Server writes, client reads.
- **Flying enemies:** which troops can target flying enemies is not decided yet (Phase 2).

## Architecture
```text
src/shared → ReplicatedStorage.Shared
  TroopData, EnemyData, WaveData, GameConfig, Types      (ModuleScripts)
src/server → ServerScriptService
  Main.server.luau   ← the ONLY server Script; requires and inits managers in order
  GameManager, WaveManager, EnemyManager, TroopManager, EconomyManager  (ModuleScripts)
src/client → StarterPlayer.StarterPlayerScripts
  Main.client.luau   ← the ONLY LocalScript; requires and inits controllers
  PlacementController, CameraController, UIController, EnemyRenderer   (ModuleScripts)
```
Rojo rule: plain `.luau` = ModuleScript and does nothing unless required. Only the two
`Main` files run on their own. Each manager/controller exposes `init()` (and `start()` if needed).

**Remotes** are created by `Main.server.luau` in `ReplicatedStorage.Remotes`:
- Client → server (RemoteFunction, returns success + reason):
  `PlaceTroop(troopType, position)`, `UpgradeTroop(troopId)`, `SellTroop(troopId)`
- Server → client (RemoteEvent): `EnemySpawned`, `EnemyUpdated`, `EnemyRemoved`

Don't add remotes elsewhere. Add new ones here and in this list.

**Responsibilities:** GameManager = match flow, win/lose. WaveManager = wave timing and
composition from WaveData. EnemyManager = enemy data, movement, death, rewards,
sanctuary damage. TroopManager = placement validation, ownership, targeting, attacks,
upgrades, selling. EconomyManager = all Sanctity changes and costs. EnemyRenderer =
client-side enemy models only.

All gameplay numbers (costs, damage, health, speeds, rewards, wave contents) live in the
data modules, never in manager code.

## Rules
1. **Server is authoritative.** Never trust client values for damage, prices, currency,
   health, placement, ownership, or wave state. Validate every remote: types, ownership,
   cost, and whether the action is legal.
2. `--!strict` in every file. Typed tables for shared data (put types in `Types`).
   Clear names, one system per module, no globals.
3. Small incremental changes. Don't rewrite working systems. If an architecture change
   is needed, explain the problem, the change, and what it could break before doing it.
4. Build only what the current phase needs.
5. Don't assume assets exist. Use placeholder Parts, list the missing assets in your
   report, and keep building the system.
6. When debugging, state the likely cause, then make the smallest fix.
7. Performance: one update loop per system, not per object. Pool enemy models on the
   client once enemy counts get large.

## Workflow (every task)
1. Read Current Status; read DESIGN.md if relevant; inspect existing code.
2. Plan the smallest change, then implement it.
3. Playtest via MCP, read the console output, and fix errors and warnings. Check that
   earlier features still work. Never report something as working just because the
   code looks right.
4. Update Current Status, commit at each working milestone with a descriptive message,
   and report what changed plus any placeholder assets needed.
