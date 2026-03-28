# Bouncing Pokémon Overlay

A browser source overlay for OBS. Displays bouncing Pokémon sprites when Pokémon Community Game spawns are detected in Twitch chat, with additional commands for manual control.

---

## Setup

1. Add `Bouncing-Pokemon.html` as a **Browser Source** in OBS.
2. Open the file and set the channels you want to monitor:
   ```js
   var CHANNELS = ['yourchannel', 'secondchannel'];
   ```
3. The overlay connects automatically when OBS loads the source.

---

## Sprite Sources

When a Pokémon is looked up, sprites are pulled from the following sources in order. The first working GIF is used, falling back to a static PNG if all fail.

| # | Source |
|---|--------|
| 1 | Showdown animated sprites (PokeAPI) |
| 2 | Gen V Black/White animated sprites (PokeAPI) |
| 3 | Project Pokémon |
| 4 | Static PNG (PokeAPI) — fallback only |

---

## Commands

All commands can be typed by anyone in chat.

### `!bouncy <pokemon>`
Manually triggers the bouncing sprite animation for any Pokémon.
```
!bouncy Pikachu
!bouncy Maushold (Family of Four)
!bouncy Great Tusk
```

### `!bouncy <pokemon> <1-4>`
Forces a specific sprite source for testing. No fallback — if the source doesn't have it, nothing shows.
```
!bouncy Pikachu 1   → Showdown GIF
!bouncy Pikachu 2   → Gen V B/W GIF
!bouncy Pikachu 3   → Project Pokémon GIF
!bouncy Pikachu 4   → Static PNG
```

### `!bouncysprites <number>`
Sets how many Pokémon sprites spawn per animation. Applies to both automatic spawns and `!bouncy` commands. Resets to 10 on page reload.
```
!bouncysprites 5
!bouncysprites 20
```
> Min: 1 — Max: 100

### `!wilduser on` / `!wilduseroff`
Toggles the first-time chatter banner. When enabled, the first time a viewer sends a message in chat this session, a *"A wild [Username] appeared!"* banner shows with bouncing Pokéballs. Resets the seen-users list when toggled either way.

> Detection is based on **first message sent**, not channel join — this is a Twitch limitation. Silent lurkers will not trigger a banner.

---

## Banner Queue

All banners (Pokémon spawns, `!bouncy`, user joins) are queued and shown one at a time with a short gap between them. If multiple events happen simultaneously, each gets its own full banner — none are dropped.

---

## Sprite Counts

| Event | Sprites | Duration |
|-------|---------|----------|
| Pokémon spawn | `!bouncysprites` value (default 10) | 10s |
| `!bouncy` | `!bouncysprites` value (default 10) | 10s |
| First-time chatter | 10 Pokéballs (fixed) | 10s |
