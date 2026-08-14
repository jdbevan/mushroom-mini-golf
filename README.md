# Mushroom Mini-Golf

A nine-hole top-down mini-golf game with booby traps, built for a 6-year-old.

**To play: double-click `index.html`.** That's it — no install, no build step, no
server, no internet connection. The whole game is one self-contained file.

## How to play

Press anywhere and **drag away from the ball**, like a catapult. A dotted line shows
where the ball will go, including its first bounce off a wall. Let go to shoot.

That is the entire control scheme. It works with a mouse or a finger, so it plays
fine on a tablet.

## Designed for small children

- **You cannot lose.** No timer, no lives, no game over. Unlimited shots.
- **Nothing on the course can cost you a shot.** Only your own swings count. Land in
  the water and the ball simply floats — you putt it back out.
- Every message is encouraging — even a very high score just says "You did it!".
- **Again** and **Skip** buttons are always on screen, so a stuck child can move on
  without needing a grown-up.
- Sound can be muted with the speaker button (or the `M` key).

## The nine holes

Each hole introduces one new idea, so nothing arrives unexplained.

| # | Hole | Par | Introduces |
|---|------|-----|------------|
| 1 | Warm-Up Green | 2 | just aiming and shooting |
| 2 | Doughnut Bend | 3 | bouncing off walls |
| 3 | Goomba Stroll | 3 | Goombas |
| 4 | Pipe Dreams | 3 | warp pipes |
| 5 | Sandy Shores | 3 | sand and water |
| 6 | Bumper Bash | 3 | ? blocks and speed arrows |
| 7 | Thwomp Alley | 4 | stomping stones |
| 8 | Slippery Slopes | 4 | ice and banana peels |
| 9 | Castle Finale | 4 | everything, plus a Chain Chomp |

## The booby traps

| Trap | What it does | Cost |
|------|--------------|------|
| Goomba | Bounces the ball back extra hard and stumbles about | none |
| Warp pipe | Rolls you in one end and out the other, at speed | none |
| Thwomp | Slams down. Caught underneath, the ball is squashed flat, then pops back | none |
| Piranha plant | Pops out of its own (olive-coloured) pipe and spits the ball somewhere silly | none |
| ? block | Springy bumper that pops out a coin | none |
| Banana peel | One-shot spin-out, then it vanishes | none |
| Speed arrows | Conveyor strip that pushes the ball along | none |
| Sand / mud | Bogs the ball down fast | none |
| Ice | Almost no friction — the ball keeps sliding | none |
| Water | Splash — then the ball floats and bobs on the surface. Heavy drag, so it takes a firm putt to get out | none |
| Chain Chomp | Lunges on its chain and swats the ball away | none |

Nothing in the table costs a shot. The only thing that increments the counter is the
player taking a swing.

## Keyboard shortcuts (handy for testing)

| Key | Does |
|-----|------|
| `1`–`9` | Jump straight to that hole |
| `D` | Toggle the debug overlay (collision shapes, FPS, ball speed) |
| `R` | Restart the current hole |
| `N` | Skip the current hole |
| `M` | Mute / unmute |
| `Space` / `Enter` | Advance the title and celebration screens |

## Adding a hole

Levels are plain data in the `LEVEL DATA` section of `index.html`. Add an object to
the `LEVELS` array and it just works — no engine changes needed:

```js
{
  name: 'My Hole', par: 3, wallStyle: 'brick',   // or 'stone'
  tee: { x: 120, y: 330 }, cup: { x: 840, y: 330 },
  walls:    [ { x: 400, y: 200, w: 120, h: 24 } ],
  surfaces: [ { type: 'sand', x: 300, y: 400, w: 200, h: 120 } ],
  traps:    [ { t: 'goomba', x: 500, y: 150, x2: 500, y2: 500, speed: 100 } ],
  decor:    [ { t: 'bush', x: 200, y: 150 } ],
  hint: 'Shown at the bottom for the first shot or two'
}
```

Coordinates live inside the play field: **x 16–944, y 72–584**. Solid boundary walls
are added automatically, so a level can never leak. `surfaces` accepts `sand`, `mud`,
`ice` and `water`; `traps` accepts any `t` listed in `TRAP_TYPES`.

Par is a rough guide, not a limit. Aim generous — a 6-year-old should beat it
sometimes.

## How it was tested

Beyond playing it, the physics were checked headlessly by loading the script into a
stubbed DOM under Node:

- **Completability** — a brute-force bot sweeps 64 angles × 5 power levels per shot
  and sinks every one of the nine holes.
- **Fuzzing** — 8,100 random shots (weighted toward full power, where tunnelling
  would show up) confirmed the ball never escapes the course, never comes to rest
  inside a wall, and always stops.
- **Trap coverage** — every trap type is confirmed to actually fire in play, and
  `Game.strokes++` exists in exactly one place: the player's own swing.
- **Pond escapes** — since the ball floats rather than being rescued, it is dropped at
  the centre and all four corners of every water body and putted out. Worst case is
  three shots to dry land; usually one.

## A note on the theme

Everything on screen is drawn from scratch with canvas shapes — the characters are
original homages to the genre, not Nintendo artwork, and no assets are downloaded.
That is what keeps the game a single portable file you can email or drop on a USB
stick, and it keeps it safe to share.
