# `assets/` — drop the PNGs here

The loader tries `assets/<file>` **first** for every sheet (`candidates()` in the
game source), so this folder is the intended home for the artwork on a hosted
deploy. Filenames are matched exactly and are case-sensitive on Vercel's
filesystem even though they are not on Windows — copy them across without
renaming.

## Required sheets

| File | Grid | Notes |
|---|---|---|
| `Dogelon_backwalk.png` | 2 x 2 | |
| `Dogelon_frontwalk.png` | 2 x 2 | |
| `Dogelon_Stand.png` | 1 x 1 | |
| `Dogelon_Blast.png` | 2 x 2 | firing pose |
| `Dogelon_Walk.png` | 2 x 2 | |
| `Dogelon_walk_Nogun.png` | 2 x 2 | lowercase `walk`, uppercase `Nogun` |
| `Dogelon_Run.png` | 2 x 2 | |
| `Dogelon_Slide.png` | 2 x 2 | |
| `Dogelon_duck.png` | 2 x 2 | lowercase `duck` |
| `Dogelon_Hit.png` | 2 x 2 | |
| `Dogelon_Dead.png` | 4 x 5 | |
| `Dogelon_spawn.png` | 2 x 2 | lowercase `spawn` |
| `Dogelon_respawn.png` | 2 x 2 | lowercase `respawn` |
| `Dogelon_explode.png` | 2 x 3 | lowercase `explode` |
| `Dogelon_enemyfly.png` | 3 x 3 | drone |
| `Dogelon_enemywalk.png` | 3 x 4 | walker |
| `Kaiju_Boss.png` | 4 x 3 | black background is keyed out by the loader |
| `Dogelon_Rover.png` | 4 x 5 | the mount that replaced the retired bull |

The three casing oddities above (`walk_Nogun`, `duck`, `spawn`, `respawn`,
`explode`) are the names as they exist in the manifest — they are not typos, and
"fixing" them will silently break those animations on a case-sensitive host.

## Optional

| File | Grid | Notes |
|---|---|---|
| `Dogelon_Ship.png` | 4 x 5 | white background, keyed out by `keyWhiteSheet()` |

**`Dogelon_Ship.png` was never supplied.** It is the jet pickup's sheet, and
because the file has never been on disk the game has always drawn the jet with
`fallbackJet()` — a hand-drawn vector ship. Nothing is broken and nothing
throws; the jet simply looks drawn rather than painted. If you want the sprite
version, drop a 4-column x 5-row sheet named exactly `Dogelon_Ship.png` in here
and it will be picked up on the next load with no code change.

## `Dogelon_Rover.png` aliases

If your rover file still has its generated name, the loader will also try, in
order: `Gemini_Generated_Image_7hmpue7hmpue7hmp-removebg-preview.png`, then
`Dogelon_rover.png`, then `Rover.png`. Renaming it to `Dogelon_Rover.png` is
cleaner but not required.

## Missing sheets are not fatal

Every sheet has a hand-drawn vector fallback. A sheet that fails to resolve
prints nothing to the loading screen (the diagnostics strip was removed on
request) and the fallback draws in its place. So a deploy with an empty
`assets/` folder still boots, still plays, and still passes the F2 self-test
suite — it just looks like a wireframe of itself.

## Music (not in this folder)

Soundtrack probing is relative to the **page**, not to `assets/`. Drop any of
these next to `index.html` at the deploy root and they will be found
automatically, in this priority order:

```
music1.mp3 music2.mp3 music3.mp3 music4.mp3 music5.mp3
track1.mp3 track2.mp3 track3.mp3
dogelon1.mp3 dogelon2.mp3 dogelon3.mp3
soundtrack.mp3 theme.mp3 boss.mp3
music1.ogg music2.ogg music1.wav music2.wav music1.m4a
```

If you ship none of them, all nineteen probes 404 and the built-in WebAudio
synth score plays instead. Those 404s are expected and appear in the browser
console on first load — they are the probe, not a failure.
