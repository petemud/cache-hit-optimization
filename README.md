This report is written by Petro Mudrievskyj to describe the algorithm used in his solution to the problem of the 1st round of [Huawei Optimization Tournament](https://algotester.com/hot/en).
Check out [the statement](statement.pdf "Cache Hit Optimization") if you've missed out on the competition.

### Report

The idea behind solution is following:

Firstly, build a graph with vertices being functions and edges `(v -> u)` - the [expected](https://en.wikipedia.org/wiki/Expected_value) number of times function `u` gets called at any point in time after function `v` during simulation run.  
And secondly, from this graph build a [maximum weight spanning tree](https://en.wikipedia.org/wiki/Minimum_spanning_tree) (except not actually a tree but a [Hamiltonian path](https://en.wikipedia.org/wiki/Hamiltonian_path)) using [Kruskal's algorithm](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm).

The second step is quite easy: in addition to Kruskal's algorithm we also don't add edges `(x -> u)` and `(v -> x)` to solution if `(v -> u)` is already present in it. And then output the generated result.

As for the first step, we could run full simulation, follow all of the branches, calculate all of the probabilities for all of the parent nodes, and then stop when we hit some error margin. And it works fine for small cases like one in the sample, but it's too slow for those with `N=10⁴`. So what can we do? - Run a couple of [Monte Carlo](https://en.wikipedia.org/wiki/Monte_Carlo_algorithm) (random) simulations and take an average (the same as a sum) over all runs.
