# Sokoban Solver

A push-optimal Sokoban solver for make.fun's `sokoban-solver-01-run-20261121`
bounty ("Build the best Sokoban solver, 21 November 2026").

## How it works

A generated level is a one-square-wide hallway with 1–4 rooms off it, one
door each. A box pushed through a door into the hallway can never come back,
so every box stays in its room and the level's push-optimal count is the sum
of its rooms' optima.

- Each room is solved on its own with a breadth-first search over pushes
  (a state is the boxes' squares plus the player's reachable region), so the
  first solved state found is push-optimal for that room. The other rooms'
  boxes stand as walls while a room is solved.
- Two safe prunings keep the search small: dead squares (a box that can
  never be pushed onto a goal again, found by pulling boxes back from the
  goals) and frozen 2×2 blocks (walls and boxes locking a box off its goal).
  Neither prunes a solvable state, so optimality stands.
- The search is bounded by work (a fixed state cap), never by the clock,
  and its order is fixed, so two runs write the same bytes.

## Results

On the kit's practice set the solver reaches the push-optimal count on all
5 instances (13 / 16 / 29 / 47 / 60 pushes) for a score of 1,000,000, in
about 1 second for the whole set. It also scores 1,000,000 on three freshly
drawn sets from the kit's published generator.

## Run

```sh
node main.ts            # reads /input/<id>/level.txt, writes /output/<id>/solution.txt
```

Node 22+, no dependencies, no install step. `main.ts` is the whole solver
(plus the MIT `LICENSE` beside it).

## License

MIT.
