# Loss Landscape Golf

**You are the optimizer.** The curve is a loss function, the ball is your parameter. Sink it into the **global minimum** in as few steps as you can.

▶ **Play: https://tianqifu380.github.io/loss-landscape-golf/**

![campaign](screenshot.png)

## Controls

- Drag away from the ball like a slingshot and release to shoot.
- Or hold `←` / `→` to charge and release. `R` restarts.
- Charge power *is* your **learning rate**: too small won't clear the ridge, too large overshoots the hole.

## Terrain

| Feature | Physics | What it feels like in training |
| --- | --- | --- |
| Local minimum | ordinary basin | Comfortable, but wrong — you need momentum to escape |
| Saddle sand | damping ×2.7, near-flat slope | Vanishing gradient: get stuck and you barely move |
| Ice | damping ×0.36 | Learning rate too high: the ball oscillates and won't settle |
| lr decay | damping ramps up after 2.4 s of rolling | Annealing, so every shot is guaranteed to converge |

## Modes

**Campaign** — six hand-built holes.

| # | Name | Par | Theme |
| --- | --- | --- | --- |
| 1 | Warm-up | 2 | feel the gradient |
| 2 | Local Minima | 3 | three basins, one answer |
| 3 | Saddle Sand | 3 | cross the flat plain |
| 4 | Ice Rink | 3 | learning rate too high |
| 5 | Noisy Gradients | 3 | tiny traps everywhere |
| 6 | The Boss | 4 | sand, ice, noise, decoys |

**Endless** — procedurally generated holes, forever. You start with 8 strokes; sinking a hole pays back `par − 1`, so par play breaks even and every miss drains the budget. Run out and the run ends. Score is holes cleared.

As the hole number climbs, decoy basins get deeper, gradient noise rises, sand and ice show up more often, the cup shrinks from 42 to 22 units, and roughly a third of holes tee off from the right instead. Every generated hole is verified solvable in at most two shots before you see it, so a run never ends on an impossible layout.

![endless](endless.png)

## Deep links

- `#lv=3` jumps straight to campaign level 3
- `#endless` starts a random endless run, `#endless=61109` replays a specific seed

## Technical notes

One `index.html`, ~35 KB, no dependencies, no build step, no network calls: Canvas 2D rendering, Web Audio blips, `localStorage` for progress (degrades gracefully in private mode).

The ball is constrained to the curve, so the tangential acceleration is `a = -g·sin θ - k·v` where `θ = arctan(dL/dθ)` and `k` comes from the terrain zone, integrated with four substeps per frame. The same physics runs headless as a small solver: campaign pars were tuned against it, and Endless uses it at generation time to rate each hole and reject unsolvable ones.

## License

MIT
