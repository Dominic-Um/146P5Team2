# P5: Writeup

**Team:** Oliver Stephenson, Dominic Umbrasas

---

## 1. What we changed and why

**Selection (`generate_successors`):** Two methods. Elitism passes on the top 5% of the population unchanged to the next generation, so that best fitness does not decrease. Tournament selection (size 5) selects the rest, taking 5 random individuals and mating the fittest of each, favoring reproductive opportunities for fitter individuals (exploitation) but still allowing weaker ones (exploration/diversity).

**Crossover (`generate_children`):** One-point crossover by column, not by tile. A randomly selected x-coordinate divides the level; Child 1 gets the columns up to that point from Parent A, and those beyond that point from Parent B, Child 2 the reverse division. We chose to do this by column since Mario levels often have vertical structure (pipes, columnar blocks, staircases), which one-point crossover by tile would make impossible.

**Mutation (`mutate`):** Every interior tile (not including the edges or the bottom row) has a 2% probability of being mutated, and with heavy weight toward empty tiles, not uniformly random choices. A previous version using uniform random mutation created very chaotic, noisy levels. Weighting toward empty tiles makes for more playable levels, while still providing diversity.

**Stopping condition:** Stops automatically after 500 generations or 40 generations without fitness improvement (whichever first). Ctrl-C still works manually.

**Fitness function (`calculate_fitness`):** : We increased the solvability weight from 2.0 to 4.0 so the algorithm would make levels with better playability. We also increased meaningfulJumpVariance (0.5 to 0.8) and pathPercentage (0.5 to 0.7) to encourage variable challenges and clear walkable paths. These changes were made so the algorithm generates playable, engaging levels that improve over generations.

**Fix (`metrics.py`):** Fixed an issue where running `python3 metrics.py <file>` from the command line kept each line's trailing `\n`, which the pathfinder misread as an extra gap tile. It made it that sometimes, making a solvable level looked unsolvable.

---

## 2. Favorite levels

**Grid encoding — `Umbrasas_Stephenson.txt`:** 291 generations, ~759 seconds (pop. 480, width 200), fitness 17.92. Confirmed solvable (`solvability: 1.0`) with a fairly direct path (`pathPercentage: 0.245`) and a good mix of jumps/obstacles (`meaningfulJumps: 25`) rather than a flat corridor. It's more dense than a hand-designed level (`decorationPercentage: 0.60`) since our fitness function didn't weight decoration/leniency but I think that gives it a unique character while still being playable.
