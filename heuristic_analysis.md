# Written analysis for Planning project

TODO: Include the following in your written analysis.  
- Provide an optimal plan for Problems 1, 2, and 3.
- Compare and contrast non-heuristic search result metrics (optimality, time elapsed, number of node expansions) for Problems 1,2, and 3. Include breadth-first, depth-first, and at least one other uninformed non-heuristic search in your comparison; Your third choice of non-heuristic search may be skipped for Problem 3 if it takes longer than 10 minutes to run, but a note in this case should be included.
- Compare and contrast heuristic search result metrics using A* with the "ignore preconditions" and "level-sum" heuristics for Problems 1, 2, and 3.
- What was the best heuristic used in these problems?  Was it better than non-heuristic search planning methods for all problems?  Why or why not?
- Provide tables or other visual aids as needed for clarity in your discussion.

## Optimal plans for problems

In each case I provide optimal length and example of the plan.
There are several optimal plans for each problem of given length becaues of actions order.
Optimality is based on minimal required action for each cargo to be transported: Load, Fly, Unload.

Problem 1 has optimal plan with length 6 (2 cargos * 3 actions):
```
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P1, SFO, JFK)
Fly(P2, JFK, SFO)
Unload(C1, P1, JFK)
Unload(C2, P2, SFO)
```

Problem 2 has optimal plan with length 9 (3 cargos * 3 actions):
```
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Load(C3, P3, ATL)
Fly(P1, SFO, JFK)
Fly(P2, JFK, SFO)
Fly(P3, ATL, SFO)
Unload(C1, P1, JFK)
Unload(C2, P2, SFO)
Unload(C3, P3, SFO)
```

Problem 2 has optimal plan with length 12 (3 cargos * 3 actions).
In this problem there are only 2 planes and 4 cargos so optimal plan with given problem definition can
reach lenghth 12 only if one plane can take 2 cargos at the time.
Since our definition does not restrict this, planning alghoritm successfully finds such optimal path:
```
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P2, JFK, ORD)
Load(C4, P2, ORD)
Fly(P1, SFO, ATL)
Load(C3, P1, ATL)
Fly(P1, ATL, JFK)
Unload(C1, P1, JFK)
Unload(C3, P1, JFK)
Fly(P2, ORD, SFO)
Unload(C2, P2, SFO)
Unload(C4, P2, SFO)
```

## Non-heuristic results

#### Problem 1

| **Heuristic**                  | **Plan length** | **Expansions** | **Goal Tests** | **New Nodes** | **Time** |
|--------------------------------|-----------------|----------------|----------------|---------------|----------|
| 1. `breadth_first_search`      | 6               | 43             | 56             | 180           | 0.03s    |
| 2. `breadth_first_tree_search` | 6               | 1458           | 1459           | 5960          | 0.94s    |
| 3. `depth_first_graph_search`  | 20              | 21             | 22             | 84            | 0.01     |
| 4. `depth_limited_search`      | 50              | 101            | 271            | 414           | 0.09s    |
| 5. `uniform_cost_search`       | 6               | 55             | 57             | 224           | 0.04s    |

#### Problem 2

`breadth_first_tree_search` and `depth_limited_search` took even more than 30 mins my machine :(

| **Heuristic**                  | **Plan length** | **Expansions** | **Goal Tests** | **New Nodes** | **Time** |
|--------------------------------|-----------------|----------------|----------------|---------------|----------|
| 1. `breadth_first_search`      | 9               | 3251           | 4440           | 26941         | 11.90s   |
| 2. `breadth_first_tree_search` | timeout         | timeout        | timeout        | timeout       | timeout  |
| 3. `depth_first_graph_search`  | 80              | 85             | 86             | 547           | 0.17s    |
| 4. `depth_limited_search`      | timeout         | timeout        | timeout        | timeout       | timeout  |
| 5. `uniform_cost_search`       | 9               | 4521           | 4523           | 36997         | 35.73s   |

#### Problem 3

`breadth_first_tree_search` and `depth_limited_search` are hopeless in this case :(

| **Heuristic**                  | **Plan length** | **Expansions** | **Goal Tests** | **New Nodes** | **Time** |
|--------------------------------|-----------------|----------------|----------------|---------------|----------|
| 1. `breadth_first_search`      | 12              | 14663          | 18098          | 129631        | 109.82s  |
| 2. `breadth_first_tree_search` | timeout         | timeout        | timeout        | timeout       | timeout  |
| 3. `depth_first_graph_search`  | 392             | 408            | 409            | 3364          | 1.77     |
| 4. `depth_limited_search`      | timeout         | timeout        | timeout        | timeout       | timeout  |
| 5. `uniform_cost_search`       | 12              | 18223          | 18225          | 159618        | 414.13s  |

Breadth-first methods guarantee to find optimal solution but in cost of many expansions and processing time
(which is good to see for problem 2 and 3).

Depth-fist methods are nice for handling problems with limited amount of computational resources but they
are unoptimal

`uniform_cost_search` is optimal but performs worse than `breadth_first_search` in terms of resourses


## Heuristic-based results

#### Problem 1

| **Heuristic**                                 | **Plan length** | **Expansions** | **Goal Tests** | **New Nodes** | **Time** |
|-----------------------------------------------|-----------------|----------------|----------------|---------------|----------|
| 6. `recursive_best_first_search with h_1`     | 6               | 4229           | 4230           | 17023         | 2.72s    |
| 7. `greedy_best_first_graph_search with h_1`  | 6               | 7              | 9              | 28            | 0.01s    |
| 8. `astar_search with h_1`                    | 6               | 55             | 57             | 224           | 0.04s    |
| 9. `astar_search with h_ignore_preconditions` | 6               | 41             | 43             | 170           | 0.03s    |
| 10. `astar_search with h_pg_levelsum`         | 6               | 11             | 13             | 50            | 1.37s    |

#### Problem 2

`recursive_best_first_search` took more than 30 on my machine :(

| **Heuristic**                                 | **Plan length** | **Expansions** | **Goal Tests** | **New Nodes** | **Time** |
|-----------------------------------------------greedy_best_first_graph_search|-----------------|----------------|----------------|---------------|----------|
| 6. `recursive_best_first_search with h_1`     | timeout         | timeout        | timeout        | timeout       | timeout  |
| 7. `greedy_best_first_graph_search with h_1`  | 21              | 645            | 647            | 4771          | 2.67s    |
| 8. `astar_search with h_1`                    | 9               | 4521           | 4523           | 36997         | 36.30s   |
| 9. `astar_search with h_ignore_preconditions` | 9               | 1370           | 1372           | 11595         | 10.10s   |
| 10. `astar_search with h_pg_levelsum`         | 9               | 148            | 150            | 1152          | 144.87s  |

#### Problem 3

`recursive_best_first_search` is doomed

| **Heuristic**                                 | **Plan length** | **Expansions** | **Goal Tests** | **New Nodes** | **Time** |
|-----------------------------------------------|-----------------|----------------|----------------|---------------|----------|
| 6. `recursive_best_first_search with h_1`     | timeout         | timeout        | timeout        | timeout       | timeout  |
| 7. `greedy_best_first_graph_search with h_1`  | 22              | 5578           | 5580           | 49150         | 104.67s  |
| 8. `astar_search with h_1`                    | 12              | 18223          | 18225          | 159618        | 394.58s  |
| 9. `astar_search with h_ignore_preconditions` | 12              | 5118           | 5120           | 45650         | 81.09s   |
| 10. `astar_search with h_pg_levelsum`         | 12              | 414            | 416            | 3818          | 1045.90s |

A* is always optimal in these cases because of optimistic heuristics, which is a requirement for optimality.

`recursive_best_first_search` has also optimal solution for problem 1, but it is unappliable for bigger problems.

`greedy_best_first_graph_search` is unoptimal, but performs faster than A* with same heuristic which is just
uniform search in fact.

`A* with h_ignore_preconditions` is best choice in terms of speed, and `A* with h_pg_levelsum` is best choice
for limited memory cases.


## Overall experiments result

`A*` is best choice for search problems since it is complete and optimal alghoritm with given admissible heuristic.
`h_1` heuristic is actually a uniform_cost_search, but other 2 show big ourperformance over non-heuristic methods 
in terms of expansions, goal tests nodes.
`A* with h_ignore_preconditions` is fastest optimal alghoritm across all methods.

3 considered heuristics are admissible since they represent relaxed problems for this planning:
* `h_1` is not a true heuristic, but we can consider it as *how much steps we need to make if we can do anything*, i.e.
get the right state in once. It is nevertheless admissible.
* `h_ignore_preconditions` relaxes preconditions and therefore gives optimistic cost estimation and is admissible. As
we can see from results, this is fastest way to get optimal solution
* `h_pg_levelsum` considers levels of planning graph which are admissible estimates for A* (see Russel-Norvig). This
allows to get optimal solutions with most minimal amount of expansions and goal tests, as well as new nodes, but in
cost of much bigger computational time

The reason of such good results of last 2 heuristics is that they are good relaxations of a problem, but
`h_ignore_preconditions` does not require such amount of mutex checks and computations as planning graph building which
is the reason of long computations.

# APPENDIX A -- Raw results

```
(aind) [budmitr@localhost AIND-Planning]$ python run_search.py -p 1 -s 1 2 3 4 5 6 7 8 9 10

Solving Air Cargo Problem 1 using breadth_first_search...

Expansions   Goal Tests   New Nodes
    43          56         180    

Plan length: 6  Time elapsed in seconds: 0.030945955000788672
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P2, JFK, SFO)
Unload(C2, P2, SFO)
Fly(P1, SFO, JFK)
Unload(C1, P1, JFK)


Solving Air Cargo Problem 1 using breadth_first_tree_search...

Expansions   Goal Tests   New Nodes
   1458        1459        5960   

Plan length: 6  Time elapsed in seconds: 0.939643703000911
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P2, JFK, SFO)
Unload(C2, P2, SFO)
Fly(P1, SFO, JFK)
Unload(C1, P1, JFK)


Solving Air Cargo Problem 1 using depth_first_graph_search...

Expansions   Goal Tests   New Nodes
    21          22          84    

Plan length: 20  Time elapsed in seconds: 0.013864688999092323
Fly(P1, SFO, JFK)
Fly(P2, JFK, SFO)
Load(C2, P1, JFK)
Fly(P1, JFK, SFO)
Fly(P2, SFO, JFK)
Unload(C2, P1, SFO)
Fly(P1, SFO, JFK)
Fly(P2, JFK, SFO)
Load(C2, P2, SFO)
Fly(P1, JFK, SFO)
Load(C1, P2, SFO)
Fly(P2, SFO, JFK)
Fly(P1, SFO, JFK)
Unload(C2, P2, JFK)
Unload(C1, P2, JFK)
Fly(P2, JFK, SFO)
Load(C2, P1, JFK)
Fly(P1, JFK, SFO)
Fly(P2, SFO, JFK)
Unload(C2, P1, SFO)


Solving Air Cargo Problem 1 using depth_limited_search...

Expansions   Goal Tests   New Nodes
   101         271         414    

Plan length: 50  Time elapsed in seconds: 0.08865505500034487
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Unload(C1, P1, SFO)
Load(C1, P1, SFO)
Fly(P2, JFK, SFO)
Unload(C2, P2, SFO)
Fly(P1, SFO, JFK)
Unload(C1, P1, JFK)


Solving Air Cargo Problem 1 using uniform_cost_search...

Expansions   Goal Tests   New Nodes
    55          57         224    

Plan length: 6  Time elapsed in seconds: 0.0397011429995473
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P1, SFO, JFK)
Fly(P2, JFK, SFO)
Unload(C1, P1, JFK)
Unload(C2, P2, SFO)


Solving Air Cargo Problem 1 using recursive_best_first_search with h_1...

Expansions   Goal Tests   New Nodes
   4229        4230       17023   

Plan length: 6  Time elapsed in seconds: 2.7197766789995512
Load(C2, P2, JFK)
Load(C1, P1, SFO)
Fly(P2, JFK, SFO)
Unload(C2, P2, SFO)
Fly(P1, SFO, JFK)
Unload(C1, P1, JFK)


Solving Air Cargo Problem 1 using greedy_best_first_graph_search with h_1...

Expansions   Goal Tests   New Nodes
    7           9           28    

Plan length: 6  Time elapsed in seconds: 0.005073371999969822
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P1, SFO, JFK)
Fly(P2, JFK, SFO)
Unload(C1, P1, JFK)
Unload(C2, P2, SFO)


Solving Air Cargo Problem 1 using astar_search with h_1...

Expansions   Goal Tests   New Nodes
    55          57         224    

Plan length: 6  Time elapsed in seconds: 0.0427983919998951
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P1, SFO, JFK)
Fly(P2, JFK, SFO)
Unload(C1, P1, JFK)
Unload(C2, P2, SFO)


Solving Air Cargo Problem 1 using astar_search with h_ignore_preconditions...

Expansions   Goal Tests   New Nodes
    41          43         170    

Plan length: 6  Time elapsed in seconds: 0.031155677999777254
Load(C1, P1, SFO)
Fly(P1, SFO, JFK)
Unload(C1, P1, JFK)
Load(C2, P2, JFK)
Fly(P2, JFK, SFO)
Unload(C2, P2, SFO)


Solving Air Cargo Problem 1 using astar_search with h_pg_levelsum...

Expansions   Goal Tests   New Nodes
    11          13          50    

Plan length: 6  Time elapsed in seconds: 1.3685133409999253
Load(C1, P1, SFO)
Fly(P1, SFO, JFK)
Load(C2, P2, JFK)
Fly(P2, JFK, SFO)
Unload(C1, P1, JFK)
Unload(C2, P2, SFO)
```


```
(aind) [budmitr@localhost AIND-Planning]$ python run_search.py -p 2 -s 1 3 5 7 8 9 10

Solving Air Cargo Problem 2 using breadth_first_search...

Expansions   Goal Tests   New Nodes
   3251        4440       26941   

Plan length: 9  Time elapsed in seconds: 11.90309572499973
Load(C1, P1, SFO)
Load(C3, P3, ATL)
Fly(P1, SFO, JFK)
Load(C2, P1, JFK)
Unload(C1, P1, JFK)
Fly(P1, JFK, SFO)
Unload(C2, P1, SFO)
Fly(P3, ATL, SFO)
Unload(C3, P3, SFO)


Solving Air Cargo Problem 2 using depth_first_graph_search...

Expansions   Goal Tests   New Nodes
    85          86         547    

Plan length: 80  Time elapsed in seconds: 0.17217689899916877
Fly(P3, ATL, SFO)
Fly(P1, SFO, ATL)
Fly(P3, SFO, JFK)
Fly(P1, ATL, JFK)
Fly(P2, JFK, ATL)
Fly(P3, JFK, ATL)
Fly(P2, ATL, SFO)
Fly(P3, ATL, SFO)
Load(C2, P1, JFK)
Fly(P3, SFO, JFK)
Fly(P1, JFK, ATL)
Fly(P3, JFK, ATL)
Fly(P1, ATL, SFO)
Unload(C2, P1, SFO)
Fly(P3, ATL, SFO)
Fly(P1, SFO, ATL)
Fly(P3, SFO, JFK)
Load(C3, P1, ATL)
Fly(P1, ATL, SFO)
Fly(P3, JFK, ATL)
Fly(P1, SFO, JFK)
Fly(P3, ATL, SFO)
Unload(C3, P1, JFK)
Fly(P3, SFO, JFK)
Fly(P1, JFK, ATL)
Fly(P3, JFK, ATL)
Fly(P1, ATL, SFO)
Load(C2, P1, SFO)
Fly(P3, ATL, SFO)
Fly(P1, SFO, ATL)
Fly(P3, SFO, JFK)
Fly(P1, ATL, JFK)
Unload(C2, P1, JFK)
Fly(P3, JFK, ATL)
Fly(P1, JFK, ATL)
Fly(P3, ATL, SFO)
Fly(P1, ATL, SFO)
Load(C1, P3, SFO)
Fly(P3, SFO, ATL)
Fly(P1, SFO, ATL)
Fly(P3, ATL, JFK)
Fly(P1, ATL, JFK)
Unload(C1, P3, JFK)
Fly(P3, JFK, ATL)
Load(C3, P1, JFK)
Fly(P3, ATL, SFO)
Fly(P1, JFK, ATL)
Fly(P3, SFO, JFK)
Fly(P1, ATL, SFO)
Unload(C3, P1, SFO)
Fly(P1, SFO, ATL)
Fly(P3, JFK, ATL)
Fly(P1, ATL, JFK)
Fly(P3, ATL, SFO)
Load(C3, P3, SFO)
Fly(P3, SFO, ATL)
Fly(P1, JFK, ATL)
Fly(P3, ATL, JFK)
Fly(P1, ATL, SFO)
Load(C2, P3, JFK)
Fly(P1, SFO, JFK)
Fly(P3, JFK, ATL)
Fly(P1, JFK, ATL)
Fly(P3, ATL, SFO)
Unload(C3, P3, SFO)
Fly(P1, ATL, SFO)
Fly(P3, SFO, ATL)
Fly(P1, SFO, JFK)
Fly(P3, ATL, JFK)
Load(C1, P3, JFK)
Fly(P3, JFK, ATL)
Fly(P1, JFK, ATL)
Fly(P3, ATL, SFO)
Fly(P1, ATL, SFO)
Unload(C2, P3, SFO)
Fly(P3, SFO, ATL)
Fly(P1, SFO, ATL)
Fly(P3, ATL, JFK)
Fly(P1, ATL, JFK)
Unload(C1, P3, JFK)


Solving Air Cargo Problem 2 using uniform_cost_search...

Expansions   Goal Tests   New Nodes
   4521        4523       36997   

Plan length: 9  Time elapsed in seconds: 35.72609790599745
Load(C1, P1, SFO)
Load(C3, P3, ATL)
Fly(P3, ATL, JFK)
Load(C2, P3, JFK)
Fly(P1, SFO, JFK)
Fly(P3, JFK, SFO)
Unload(C3, P3, SFO)
Unload(C2, P3, SFO)
Unload(C1, P1, JFK)


Solving Air Cargo Problem 2 using greedy_best_first_graph_search with h_1...

Expansions   Goal Tests   New Nodes
   645         647         4771   

Plan length: 21  Time elapsed in seconds: 2.6740025380022416
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Load(C3, P3, ATL)
Fly(P1, SFO, ATL)
Fly(P2, JFK, ATL)
Fly(P3, ATL, SFO)
Fly(P1, ATL, JFK)
Unload(C1, P1, JFK)
Fly(P1, JFK, ATL)
Fly(P3, SFO, JFK)
Load(C1, P3, JFK)
Fly(P3, JFK, SFO)
Unload(C2, P2, ATL)
Fly(P2, ATL, SFO)
Fly(P3, SFO, ATL)
Load(C2, P3, ATL)
Fly(P3, ATL, JFK)
Unload(C1, P3, JFK)
Fly(P3, JFK, SFO)
Unload(C3, P3, SFO)
Unload(C2, P3, SFO)


Solving Air Cargo Problem 2 using astar_search with h_1...

Expansions   Goal Tests   New Nodes
   4521        4523       36997   

Plan length: 9  Time elapsed in seconds: 36.295926916998724
Load(C1, P1, SFO)
Load(C3, P3, ATL)
Fly(P3, ATL, JFK)
Load(C2, P3, JFK)
Fly(P1, SFO, JFK)
Fly(P3, JFK, SFO)
Unload(C3, P3, SFO)
Unload(C2, P3, SFO)
Unload(C1, P1, JFK)


Solving Air Cargo Problem 2 using astar_search with h_ignore_preconditions...

Expansions   Goal Tests   New Nodes
   1370        1372       11595   

Plan length: 9  Time elapsed in seconds: 10.09609821399863
Load(C3, P3, ATL)
Fly(P3, ATL, JFK)
Load(C2, P3, JFK)
Fly(P3, JFK, SFO)
Unload(C3, P3, SFO)
Unload(C2, P3, SFO)
Load(C1, P1, SFO)
Fly(P1, SFO, JFK)
Unload(C1, P1, JFK)


Solving Air Cargo Problem 2 using astar_search with h_pg_levelsum...

Expansions   Goal Tests   New Nodes
   148         150         1152   

Plan length: 9  Time elapsed in seconds: 144.86774064600104
Load(C3, P3, ATL)
Fly(P3, ATL, SFO)
Unload(C3, P3, SFO)
Load(C1, P3, SFO)
Fly(P3, SFO, JFK)
Unload(C1, P3, JFK)
Load(C2, P3, JFK)
Fly(P3, JFK, SFO)
Unload(C2, P3, SFO)
```

```
(aind) [budmitr@localhost AIND-Planning]$ python run_search.py -p 3 -s 1 3 5 7 8 9 10

Solving Air Cargo Problem 3 using breadth_first_search...

Expansions   Goal Tests   New Nodes
  14663       18098       129631  

Plan length: 12  Time elapsed in seconds: 109.8218858830005
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P2, JFK, ORD)
Load(C4, P2, ORD)
Fly(P1, SFO, ATL)
Load(C3, P1, ATL)
Fly(P1, ATL, JFK)
Unload(C1, P1, JFK)
Unload(C3, P1, JFK)
Fly(P2, ORD, SFO)
Unload(C2, P2, SFO)
Unload(C4, P2, SFO)


Solving Air Cargo Problem 3 using depth_first_graph_search...

Expansions   Goal Tests   New Nodes
   408         409         3364   

Plan length: 392  Time elapsed in seconds: 1.7869214359998296
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, JFK)
Fly(P2, ATL, SFO)
Load(C2, P1, JFK)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, SFO)
Unload(C2, P1, SFO)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, JFK)
Fly(P2, ATL, SFO)
Load(C2, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Load(C3, P1, ATL)
Fly(P1, ATL, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, SFO)
Fly(P2, ORD, ATL)
Fly(P1, SFO, JFK)
Fly(P2, ATL, SFO)
Unload(C3, P1, JFK)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, SFO)
Unload(C2, P2, JFK)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, JFK)
Fly(P2, ATL, SFO)
Load(C3, P1, JFK)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, SFO)
Fly(P2, ATL, JFK)
Unload(C3, P1, SFO)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, JFK)
Fly(P2, ATL, SFO)
Load(C3, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, SFO)
Load(C1, P1, SFO)
Fly(P2, ATL, ORD)
Fly(P1, SFO, ORD)
Fly(P2, ORD, SFO)
Fly(P1, ORD, ATL)
Fly(P2, SFO, JFK)
Fly(P1, ATL, JFK)
Unload(C3, P2, JFK)
Fly(P2, JFK, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, SFO)
Unload(C1, P1, ATL)
Fly(P1, ATL, ORD)
Fly(P2, SFO, ORD)
Fly(P1, ORD, SFO)
Fly(P2, ORD, ATL)
Fly(P1, SFO, JFK)
Fly(P2, ATL, JFK)
Load(C3, P2, JFK)
Fly(P2, JFK, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Unload(C3, P2, ATL)
Fly(P2, ATL, ORD)
Fly(P1, ATL, ORD)
Fly(P2, ORD, SFO)
Fly(P1, ORD, SFO)
Fly(P2, SFO, JFK)
Fly(P1, SFO, JFK)
Load(C2, P2, JFK)
Fly(P2, JFK, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, SFO)
Fly(P1, ATL, SFO)
Unload(C2, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, SFO, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Load(C3, P1, ATL)
Fly(P1, ATL, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, SFO)
Fly(P2, ORD, ATL)
Fly(P1, SFO, JFK)
Fly(P2, ATL, SFO)
Unload(C3, P1, JFK)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, SFO)
Load(C3, P2, JFK)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, SFO)
Fly(P1, ATL, JFK)
Fly(P2, SFO, ATL)
Load(C1, P2, ATL)
Fly(P2, ATL, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, SFO)
Fly(P1, ORD, ATL)
Fly(P2, SFO, JFK)
Fly(P1, ATL, SFO)
Unload(C3, P2, JFK)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, SFO)
Fly(P1, ATL, JFK)
Load(C3, P1, JFK)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, SFO)
Unload(C3, P1, SFO)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, JFK)
Fly(P2, ATL, SFO)
Unload(C1, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, SFO)
Load(C3, P1, SFO)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, JFK)
Fly(P2, ORD, ATL)
Unload(C3, P1, JFK)
Fly(P2, ATL, JFK)
Fly(P1, JFK, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Load(C4, P2, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ATL, ORD)
Fly(P2, ATL, SFO)
Fly(P1, ORD, SFO)
Fly(P2, SFO, JFK)
Fly(P1, SFO, JFK)
Unload(C4, P2, JFK)
Fly(P2, JFK, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, SFO)
Fly(P1, ATL, SFO)
Load(C2, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, SFO, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, JFK)
Unload(C2, P2, JFK)
Fly(P2, JFK, ORD)35.72
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, SFO)
Fly(P1, ATL, SFO)
Load(C1, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, SFO, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, JFK)
Unload(C1, P2, JFK)
Fly(P2, JFK, ORD)
Load(C4, P1, JFK)
Fly(P2, ORD, ATL)
Fly(P1, JFK, ORD)
Fly(P2, ATL, SFO)
Fly(P1, ORD, ATL)
Fly(P2, SFO, JFK)
Fly(P1, ATL, SFO)
Unload(C4, P1, SFO)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, JFK)
Fly(P2, ATL, SFO)
Load(C4, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, SFO)
Load(C3, P2, JFK)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, JFK)
Fly(P2, ATL, SFO)
Unload(C4, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Load(C2, P2, JFK)
Fly(P1, ATL, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, SFO)
Fly(P2, ORD, ATL)
Fly(P1, SFO, JFK)
Fly(P2, ATL, SFO)
Unload(C3, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, SFO)
Unload(C2, P2, JFK)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, JFK)
Load(C2, P1, JFK)
Fly(P2, ATL, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, SFO)
Fly(P1, ORD, ATL)
Fly(P2, SFO, JFK)
Fly(P1, ATL, SFO)
Unload(C2, P1, SFO)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, JFK)
Load(C1, P1, JFK)
Fly(P2, ATL, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, SFO)
Fly(P1, ORD, ATL)
Fly(P2, SFO, JFK)
Fly(P1, ATL, SFO)
Unload(C1, P1, SFO)
Fly(P1, SFO, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, JFK)
Fly(P2, ATL, SFO)
Load(C4, P2, SFO)
Fly(P2, SFO, ATL)
Fly(P1, JFK, ORD)
Fly(P2, ATL, JFK)
Fly(P1, ORD, ATL)
Fly(P2, JFK, ORD)
Fly(P1, ATL, SFO)
Load(C3, P1, SFO)
Fly(P2, ORD, ATL)
Fly(P1, SFO, ORD)
Fly(P2, ATL, SFO)
Fly(P1, ORD, ATL)
Fly(P2, SFO, JFK)
Unload(C4, P2, JFK)
Fly(P1, ATL, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, SFO)
Fly(P2, ORD, ATL)
Fly(P1, SFO, JFK)
Fly(P2, ATL, SFO)
Load(C4, P1, JFK)
Fly(P2, SFO, ORD)
Fly(P1, JFK, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, ORD)
Unload(C3, P1, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ATL, ORD)
Fly(P1, ATL, SFO)
Fly(P2, ORD, SFO)
Fly(P1, SFO, JFK)
Fly(P2, SFO, JFK)
Unload(C4, P1, JFK)
Fly(P2, JFK, ORD)
Fly(P1, JFK, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, ORD)
Fly(P2, ATL, SFO)
Fly(P1, ORD, SFO)
Load(C2, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, SFO, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, JFK)
Unload(C2, P2, JFK)
Fly(P2, JFK, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, SFO)
Fly(P1, ATL, SFO)
Load(C1, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, SFO, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, JFK)
Unload(C1, P2, JFK)
Fly(P2, JFK, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Load(C3, P1, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ATL, ORD)
Fly(P1, ATL, SFO)
Fly(P2, ORD, SFO)
Fly(P1, SFO, JFK)
Load(C4, P1, JFK)
Fly(P2, SFO, JFK)
Fly(P1, JFK, ORD)
Fly(P2, JFK, ORD)
Fly(P1, ORD, ATL)
Fly(P2, ORD, ATL)
Fly(P1, ATL, SFO)
Unload(C4, P1, SFO)
Fly(P2, ATL, ORD)
Fly(P1, SFO, ORD)
Fly(P2, ORD, SFO)
Fly(P1, ORD, ATL)
Fly(P2, SFO, JFK)
Fly(P1, ATL, JFK)
Load(C2, P2, JFK)
Fly(P2, JFK, ORD)
Fly(P1, JFK, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, SFO)
Fly(P1, ATL, SFO)
Unload(C2, P2, SFO)
Fly(P2, SFO, ORD)
Fly(P1, SFO, ORD)
Fly(P2, ORD, ATL)
Fly(P1, ORD, ATL)
Fly(P2, ATL, JFK)
Fly(P1, ATL, JFK)
Unload(C3, P1, JFK)


Solving Air Cargo Problem 3 using uniform_cost_search...

Expansions   Goal Tests   New Nodes
  18223       18225       159618  

Plan length: 12  Time elapsed in seconds: 414.12728583699936
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P1, SFO, ATL)
Load(C3, P1, ATL)
Fly(P2, JFK, ORD)
Load(C4, P2, ORD)
Fly(P2, ORD, SFO)
Fly(P1, ATL, JFK)
Unload(C4, P2, SFO)
Unload(C3, P1, JFK)
Unload(C2, P2, SFO)
Unload(C1, P1, JFK)


Solving Air Cargo Problem 3 using greedy_best_first_graph_search with h_1...

Expansions   Goal Tests   New Nodes
   5578        5580       49150   

Plan length: 22  Time elapsed in seconds: 104.67041494299701
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P1, SFO, ORD)
Load(C4, P1, ORD)
Fly(P2, JFK, ATL)
Load(C3, P2, ATL)
Fly(P2, ATL, ORD)
Fly(P1, ORD, ATL)
Unload(C4, P1, ATL)
Fly(P1, ATL, ORD)
Fly(P2, ORD, ATL)
Load(C4, P2, ATL)
Fly(P2, ATL, ORD)
Unload(C3, P2, ORD)
Load(C3, P1, ORD)
Fly(P1, ORD, JFK)
Unload(C3, P1, JFK)
Unload(C1, P1, JFK)
Fly(P1, JFK, ORD)
Fly(P2, ORD, SFO)
Unload(C4, P2, SFO)
Unload(C2, P2, SFO)


Solving Air Cargo Problem 3 using astar_search with h_1...

Expansions   Goal Tests   New Nodes
  18223       18225       159618  

Plan length: 12  Time elapsed in seconds: 394.5834422759981
Load(C1, P1, SFO)
Load(C2, P2, JFK)
Fly(P1, SFO, ATL)
Load(C3, P1, ATL)
Fly(P2, JFK, ORD)
Load(C4, P2, ORD)
Fly(P2, ORD, SFO)
Fly(P1, ATL, JFK)
Unload(C4, P2, SFO)
Unload(C3, P1, JFK)
Unload(C2, P2, SFO)
Unload(C1, P1, JFK)


Solving Air Cargo Problem 3 using astar_search with h_ignore_preconditions...

Expansions   Goal Tests   New Nodes
   5118        5120       45650   

Plan length: 12  Time elapsed in seconds: 81.08851811400018
Load(C2, P2, JFK)
Fly(P2, JFK, ORD)
Load(C4, P2, ORD)
Fly(P2, ORD, SFO)
Unload(C4, P2, SFO)
Load(C1, P1, SFO)
Fly(P1, SFO, ATL)
Load(C3, P1, ATL)
Fly(P1, ATL, JFK)
Unload(C3, P1, JFK)
Unload(C2, P2, SFO)
Unload(C1, P1, JFK)


Solving Air Cargo Problem 3 using astar_search with h_pg_levelsum...

Expansions   Goal Tests   New Nodes
   414         416         3818   

Plan length: 12  Time elapsed in seconds: 1045.9041954890017
Load(C2, P2, JFK)
Fly(P2, JFK, ORD)
Load(C4, P2, ORD)
Fly(P2, ORD, SFO)
Load(C1, P1, SFO)
Fly(P1, SFO, ATL)
Load(C3, P1, ATL)
Fly(P1, ATL, JFK)
Unload(C4, P2, SFO)
Unload(C3, P1, JFK)
Unload(C2, P2, SFO)
Unload(C1, P1, JFK)
```