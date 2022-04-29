#### <u>Lecture 01: Intelligent Agents & Problem Environments</u>

**Types of AI**
Strong: dynamic, solves many problems
Weak/Narrow: less dynamic, solves fewer problems
**§ Intelligent Agents**
**Agent Components**
Sensors $\to$ Function $\to$ Actuators
•  Sensors: what can/should be captured
   Percept data at time $t$: $p_t$	Percept history $P = \{p_1, p_2, \dots, p_t\}$
•  Function:
   Maps any given percept sequence to an action, $f: P \to a$
•  Actuators: how will agent affect change
   Set of actions $A$
Goal in AI: determine $f:P \to a, a\in A$
Given available data, rational agent optimises performance measure
**Types of Agents**
•  Reflex: Use if-statements, direct mapping
•  Model-based reflex: Based on maintained internal state
•  Goal-based and utility-based: Achieves goals
•  Learning: Agents that learn how to optimise performance
**§ Problem Environments**
Fully/Partially observable	Deterministic/Stochastic
Episodic/Sequential	Discrete/Continuous
Single/Multi agent	Known/Unknown	Static/Dynamic

#### <u>Lecture 02: Uninformed Search</u>

Path planning problem properties: fully observable, deterministic, discrete, episodic
**§ Search Spaces**

| Term             | Symbol                               | Meaning                                                      |
| ---------------- | ------------------------------------ | ------------------------------------------------------------ |
| State            | $s_i$                                | ADT describing an instance of the environment                |
| Initial state    | $s_0$                                | State that agent starts in                                   |
| Goal test        | $\text{isGoal}:s_i \to \{0, 1\}$     | Checks if $s_i$ is a goal state                              |
| Action           | $\text{actions}:s_i \to A$           | Returns possible actions at given state                      |
| Action cost      | $\text{cost}:(s_i, a_j, s_i') \to v$ | Returns cost of taking $a_j$ at $s_i$ to reach $s_i'$, generally assume $v \geq 0$ |
| Transition model | $\text{T}:(s_i, a_j) \to s_i'$       | Returns state transitioned to when $a_j$ applied to $s_i$    |

**Performance Criteria**
Time complexity	Space complexity
Completeness	Optimality
**Redundant Path**
More expensive path from $s_0$ to $s1$, cycle is a special case of redundant path
Affects optimality of the algorithm
To rectify, use graph search instead of tree search:
•  Maintain reached hash table
•  Add nodes to state reached
•  Add new node to frontier if state not reached or path cheaper than stored
**§ Uninformed Search Algorithms**
No domain knowledge beyond search space formulation, no clue on how close a state is to goal state(s)
**General Algorithm**

```python
frontier = {initial state} # data structure
while frontier:
  current = frontier.pop()
  if isGoal(current): return True
  for a in actions(current): frontier.push(T(current, a))
return False
```

**Algorithm Implementations**
$b$: branching factor	$d$: depth for optimal solution	$m$: max depth	$\ell$: depth limit
$e$: $1 + \lfloor C^* / \epsilon \rfloor$, where $C^*$ is the optimal path, $\epsilon $ is a small positive constant

| Algo | DS             | Time                            | Space                         | Notes                                                    |
| ---- | -------------- | ------------------------------- | ----------------------------- | -------------------------------------------------------- |
| BFS  | Queue          | $O(b^d)$                        | $O(b^d)$                      |                                                          |
| UCS  | Priority queue | $O(b^e)$                        | $O(b^e)$                      | Increments of $\epsilon$                                 |
| DFS  | Stack          | $O(b^m)$                        | $O(bm)$                       | Memory efficient                                         |
| DLS  | Stack          | $O(b^\ell)$                     | $O(b\ell)$                    | Search up to $\ell$                                      |
| IDS  | Stack          | $O(b^d)$ / $O(b^m)$ no solution | $O(bd)$ / $O(bm)$ no solution | Increment $\ell$, BFS completeness, DFS space complexity |

| Algo |                           Complete                           |           Optimal           | Improvement                                |
| :--: | :----------------------------------------------------------: | :-------------------------: | ------------------------------------------ |
| BFS  | ✅<br />if $b$ finite AND<br />($d$ finite OR contain solution) | ❎<br />unless costs uniform | Early goal test before pushing to frontier |
| UCS  |                              ✅                               |              ✅              | Early goal test not optimal                |
| DFS  |                ✅<br />if finite search space                 |              ❎              | Backtracking, DLS                          |
| DLS  |                              ❎                               |              ❎              | IDS                                        |
| IDS  |                         ✅<br />(BFS)                         |        ❎<br />(BFS)         | Overhead not by a lot                      |

#### <u>Lecture 03: Informed Search</u>

**§ Informed Search Algorithms**
Use domain knowledge to estimate cost to nearest goal
**Heuristic Function**
$h(n)$ efficiently uses domain knowledge to approximate path cost to nearest goal
•  Admissibility
   $h(n)$ never overestimates the cost
   $\forall n, h(n) \leq h^*(n), \, h(G) = 0$
•  Consistency
   $f(n)$ to be monotonically increasing along path
   $\forall n, h(n) \leq \text{cost}(n, a, n') + h(n')$
   Consistency $\to$ admissibiilty
•  Dominance
   $h_1(n)$ dominates $h_2$ if $\forall n, h_1(n) \geq h_2(n)$
**Greedy Best-First Search**
$f(n) = h(n)$
Never exploits information on path already travelled, worst-case time and space complexity $O(b^m)$

| Tree search                              | Graph search                        |
| ---------------------------------------- | ----------------------------------- |
| NOT complete, can get stuck in the group | Complete, if search space is finite |
| NOT optimal                              | NOT optimal                         |

**A* Search**
$f(n) = g(n) + h(n)$
$g(n)$: actual path cost from $\text{Start}$ to $n$
$h(n)$: estimated cheapest path cost from $n$ to $\text{Goal}$

| Tree search                   | Graph search                  |
| ----------------------------- | ----------------------------- |
| Optimal, if $h(n)$ admissible | Optimal, if $h(n)$ consistent |

To prove optimality of admissible A* tree search:
$s \to t$, $s \to n \to t^*$, $t^*$ is optimal path

1) Assume $t$ expanded before $n$, $f(t) < f(n)$
2) For a non-goal node $m$, $f(m) \leq g(m) + h^*(m)$
3) For a goal node $m^*$ on path from $m$, $f(m) \leq f(m^*)$
4) $f(n) \leq f(t^*)$ from $3.$, $f(t^*) < f(t)$ since $t^*$ optimal, hence $f(n) < f(t)$
5) Contradicts $1.$

Updating prevents skipping of non-redundant paths for graph search
Updates are expensive, must link to ancestors and descendants
Remedy: add state to reached on pop

#### <u>Lecture 04: Heuristics</u>

Heuristics domination translates directly to efficiency
Identify $h$ that approximates $h^*(n)$, ideally $O(1)$ time complexity
Heuristics from relaxed problems for 8-puzzle
•  Number of misplaced tiles
•  Sum of Manhattan distances of tiles from goal positions

#### <u>Lecture 05: Local Search</u>

Only interested about goal state, not the path and cost to get there
**Pros**:
•  $O(b)$ space complexity, only store current and immediate successor states
•  Applicable to large or infinite state spaces
**Cons**:
•  Incomplete
**§ Hill-Climbing Search**
**Algorithm (Greedy Local Search)**

```python
current = initial state
while True:
	neighbour = highest-valued successor of current
	if value(neighbour) <= value(current):
	  return current
	current = neighbour
```

`value`: a way to evaluate each state e.g. $f(n) = -h(n)$
Can use $f(n) = h(n)$ by replacing `<=` with `>=`
**Example: n-queens problem**
Complete-state formulation: every state has all components of a solution, but might not be in the right place
$h(n)$: number of pairs of queens threatening each other
**Issues**
Hill-climbing may not return a solution and can get stuck at:
•  Local maxima
•  Ridges
•  Plateaus / Shoulders
**Variants**
•  Sideways move
   `value(neighbour) < value(current)` in hope that plateau is a shoulder
•  Stochastic hill climbing
   Choose among uphill moves at random
•  First-choice hill climbing
   Generate successors randomly until one has better value than current
•  Random restart hill climbing
   Keep attempting from randomly generated initial states until goal is found
**Expected Computation**
$\displaystyle \text{steps for success} + \frac{1-p}{p} \times \text{steps for failure}$
**§ Local Beam Search**
Keep track of $k$ states rather than just one
•  Begin with $k$ randomly generated states
•  Generate all successors for all $k$ states
•  Selects $k$ best successors and repeat
Performs better than $k$ random restarts in parallel
**Variants**
•  Stochastic beam search
   Choose successors with probability proportional to successor's value
**§ Constraint Satisfaction Problems**
Use systematic search to reduce search space, prune invalid subtrees as early as possible
**Formulation**

1) $\mathcal{X}$: set of variables $\{X_1, \dots, X_n\}$
2) $\mathcal{D}$: set of domains $\{D_1, \dots, D_n\}$
$D_i = \{v_1, \dots, v_k\}$
3) $\mathcal{C}$: set of constraints $\{C_i, \dots, C_m\}$
   $C_j = \langle \text{scope}, \text{rel} \rangle$
   $\text{scope}$: tuple of participating variables
   $\text{rel}$: relation that defines values that variables can take

Initial state: all variables unassigned
Intermediate state: partial assignment
Goal state: complete and consistent assignment
•  Complete: all variables assigned value
•  Consistent: no constraint violated
**Domain Types**
Continuous / discrete	Finite / infinite
**Constraint Types**
Linear / non-linear	Unary / binary / global
**Constraint Graph**
Simple vertex: variable
Linking vertex: global constraint
Edge: relation
**Search Tree Size**
Example: $D$ same for all domains, no constraints
Order of assignment is not important
At depth $\ell$ : $(|\mathcal{X}| - \ell) \cdot |D|$ states
Total # leaf states: $nm \times (n-1)m \times \dots \times m = n! m^n$
Where $n = |\mathcal{X}|$ and $m = |D|$
