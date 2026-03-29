# Bouncing Pokémon Overlay

A browser source overlay for OBS. Displays bouncing Pokémon sprites when Pokémon Community Game spawns are detected in Twitch chat.

---

## Setup

1. Add `Bouncing-Pokemon.html` as a **Browser Source** in OBS.
2. When it loads for the first time, a setup screen will appear asking for your Twitch channel name.
3. Enter your channel name and click **START**. The channel is saved automatically — OBS will connect to it on every future launch without showing the setup screen again.

---

## Changing Your Channel

### `!clearchannel`
Clears the saved channel from the browser source cache. Type this command **in your own Twitch chat**. After sending it, right-click the browser source in OBS and select **Refresh** — the setup screen will appear again so you can enter a new channel.

> Only the streamer (the channel entered at setup) can use this command.

---

## Sprite Sources

When a Pokémon is looked up, sprites are pulled in order. The first working GIF is used, falling back to a static PNG if all fail.

| # | Source |
|---|--------|
| 1 | Showdown animated sprites (PokeAPI) |
| 2 | Gen V Black/White animated sprites (PokeAPI) |
| 3 | Project Pokémon |
| 4 | Static PNG (PokeAPI) — fallback only |

---

## Commands

### `!bouncy <pokemon>`
Manually triggers the bouncing sprite animation for any Pokémon. Open to everyone in chat.
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
Sets how many Pokémon sprites spawn per animation. Applies to automatic spawns and `!bouncy` only — Pokéballs are always 10. Resets to 10 on page reload. Open to everyone.
```
!bouncysprites 5
!bouncysprites 20
```
> Min: 1 — Max: 100

### `!bouncyvolume <number>`
Sets the volume of the Pokémon cry audio that plays when a Pokémon appears. Saved automatically — persists across OBS reloads. Open to everyone.
```
!bouncyvolume 0    → muted
!bouncyvolume 50   → 50% (default)
!bouncyvolume 100  → max
```
> Min: 0 — Max: 100

> **OBS note:** For the cry audio to play on stream, right-click the browser source in OBS → Properties → check **"Control audio via OBS"**. The browser source will then appear in your audio mixer.

### `!wilduser on` / `!wilduseroff`
Toggles the first-time chatter banner. When enabled, the first time a viewer sends a message this session, a *"A wild [Username] appeared!"* banner shows with bouncing Pokéballs. Toggling either way resets the seen-users list.

> Restricted to channels listed in `WILDUSER_AUTH` plus the streamer's own channel.

> Detection is based on **first message sent**, not channel join — this is a Twitch limitation. Silent lurkers will not trigger a banner.

---

## Pokémon Cries

When a Pokémon appears (via PCG spawn or `!bouncy`), its cry plays in sync with the banner. Cries are sourced from Pokémon Showdown and cover all Pokémon Gen 1–9 including special characters (Nidoran♀/♂, Mr. Mime, Farfetch'd, etc.).

Volume defaults to 50% and can be changed with `!bouncyvolume`. The setting persists across reloads.

---

## Banner Queue

All banners (Pokémon spawns, `!bouncy`, user joins) are queued and shown one at a time with a short gap between them. If multiple events happen simultaneously, each gets its own full banner — none are dropped.

Pokémon spawns detected across multiple channels within 10 seconds of each other are collapsed into a single banner to avoid duplicates.

---

## Sprite Counts

| Event | Sprites | Duration |
|-------|---------|----------|
| Pokémon spawn | `!bouncysprites` value (default 10) | 10s |
| `!bouncy` | `!bouncysprites` value (default 10) | 10s |
| First-time chatter | 10 Pokéballs (fixed) | 10s |
