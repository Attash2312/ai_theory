# Module 2: AI Problem Solving and Genetic Algorithms

## Table of Contents
1. [Problem Formulation](#problem-formulation)
2. [State Space Representation](#state-space-representation)
3. [Uninformed Search Algorithms](#uninformed-search-algorithms)
4. [Optimization Problems in AI](#optimization-problems-in-ai)
5. [Genetic Algorithms](#genetic-algorithms)
6. [Comparative Analysis](#comparative-analysis)
7. [Solved Exam Questions](#solved-exam-questions)

---

# Problem Formulation

## What is a Problem in AI?

A **problem** in AI is a task that an agent needs to solve by finding a sequence of actions that leads from an initial state to a goal state.

## Components of Problem Formulation

| Component | Description | Example (Route Finding) |
|-----------|-------------|------------------------|
| **Initial State** | Starting configuration | Agent in City A |
| **Actions** | Possible moves from a state | Drive to adjacent city |
| **Transition Model** | Result of applying action | New city location |
| **Goal Test** | Check if goal is reached | Are we in City Z? |
| **Path Cost** | Cost of the solution path | Total distance traveled |

## Problem Formulation Template

```
PROBLEM FORMULATION
│
├── State Space: All possible configurations
├── Initial State: Where we start
├── Actions(s): Available actions in state s
├── Result(s, a): State after action a in state s
├── Goal Test: Is current state a goal?
└── Path Cost: Sum of action costs
```

## Example: 8-Puzzle Problem

```
Initial State:          Goal State:
┌───┬───┬───┐          ┌───┬───┬───┐
│ 1 │ 2 │ 5 │          │ 1 │ 2 │ 3 │
├───┼───┼───┤          ├───┼───┼───┤
│ 3 │ 4 │   │    →     │ 4 │ 5 │ 6 │
├───┼───┼───┤          ├───┼───┼───┤
│ 6 │ 7 │ 8 │          │ 7 │ 8 │   │
└───┴───┴───┘          └───┴───┴───┘

State Space: All 9!/2 = 181,440 possible configurations
Actions: Move blank Up, Down, Left, Right
Goal Test: Tiles in order 1-8 with blank at bottom-right
Path Cost: Number of moves
```

---

# State Space Representation

## Definition

**State Space** is the set of all possible states reachable from the initial state by any sequence of actions.

## State Space Graph Example

```
                       ┌───┐
                       │ A │ (Start)
                       └─┬─┘
            ┌──────────┼──────────┐
            ▼          ▼          ▼
          ┌───┐      ┌───┐      ┌───┐
          │ B │      │ C │      │ D │
          └─┬─┘      └─┬─┘      └─┬─┘
         ┌──┴──┐       │       ┌──┴──┐
         ▼     ▼       ▼       ▼     ▼
       ┌───┐ ┌───┐   ┌───┐   ┌───┐ ┌───┐
       │ E │ │ F │   │ G │   │ H │ │ I │ (Goal)
       └───┘ └───┘   └───┘   └───┘ └───┘
```

## Search Tree vs State Space Graph

| Aspect | State Space Graph | Search Tree |
|--------|-------------------|-------------|
| Nodes | Unique states | Paths to states |
| Duplicates | No duplicate nodes | Can have duplicates |
| Size | Finite (usually) | Can be infinite |
| Edges | Actions between states | Parent-child relations |
| Purpose | Problem representation | Search exploration |

---

# Uninformed Search Algorithms

## Overview

**Uninformed (Blind) Search:** No additional information about how close a state is to the goal. Only uses the problem definition.

| Algorithm | Strategy | Complete? | Optimal? | Time | Space |
|-----------|----------|-----------|----------|------|-------|
| BFS | Shallowest first | Yes* | Yes* | O(b^d) | O(b^d) |
| DFS | Deepest first | No | No | O(b^m) | O(bm) |
| UCS | Lowest cost first | Yes | Yes | O(b^(C*/ε)) | O(b^(C*/ε)) |

**Where:** b = branching factor, d = depth of solution, m = max depth

---

## Breadth-First Search (BFS)

### Definition

BFS explores all nodes at the current depth level before moving to nodes at the next depth level. Uses a **FIFO Queue** (First-In-First-Out).

### Algorithm Pseudocode

```
function BFS(problem):
    frontier ← Queue containing initial state
    explored ← empty set
    
    while frontier is not empty:
        node ← frontier.dequeue()
        
        if goal_test(node):
            return solution
        
        explored.add(node)
        
        for each child of node:
            if child not in explored and child not in frontier:
                frontier.enqueue(child)
    
    return failure
```

### BFS Solved Example

```
Problem: Find path from A to G

Graph:
                    A
                   /|\
                  B C D
                 /|   |\
                E F   G H

BFS Exploration Order: A → B → C → D → E → F → G (FOUND!)

Step-by-step Trace:
┌──────┬─────────────────┬───────────────┬────────────┐
│ Step │ Current Node    │ Queue         │ Explored   │
├──────┼─────────────────┼───────────────┼────────────┤
│  1   │ -               │ [A]           │ {}         │
│  2   │ A               │ [B, C, D]     │ {A}        │
│  3   │ B               │ [C, D, E, F]  │ {A, B}     │
│  4   │ C               │ [D, E, F]     │ {A, B, C}  │
│  5   │ D               │ [E, F, G, H]  │ {A,B,C,D}  │
│  6   │ E               │ [F, G, H]     │ {A,B,C,D,E}│
│  7   │ F               │ [G, H]        │ {A,B,C,D,E,F}│
│  8   │ G               │ GOAL FOUND!   │            │
└──────┴─────────────────┴───────────────┴────────────┘

Solution Path: A → D → G
Path Length: 2
```

### BFS Properties

| Property | Value | Explanation |
|----------|-------|-------------|
| Complete | Yes* | Finds solution if it exists (*if b is finite) |
| Optimal | Yes* | Finds shortest path (*if all step costs are equal) |
| Time | O(b^d) | Explores all nodes up to depth d |
| Space | O(b^d) | Stores all nodes in memory |

---

## Depth-First Search (DFS)

### Definition

DFS explores as deep as possible along each branch before backtracking. Uses a **LIFO Stack** (Last-In-First-Out).

### Algorithm Pseudocode

```
function DFS(problem):
    frontier ← Stack containing initial state
    explored ← empty set
    
    while frontier is not empty:
        node ← frontier.pop()
        
        if goal_test(node):
            return solution
        
        explored.add(node)
        
        for each child of node (in reverse order):
            if child not in explored:
                frontier.push(child)
    
    return failure
```

### DFS Solved Example

```
Problem: Find path from A to G

Graph:
                    A
                   /|\
                  B C D
                 /|   |\
                E F   G H

DFS Exploration Order: A → B → E → F → C → D → G (FOUND!)

Step-by-step Trace:
┌──────┬─────────────────┬───────────────┬────────────┐
│ Step │ Current Node    │ Stack         │ Explored   │
├──────┼─────────────────┼───────────────┼────────────┤
│  1   │ -               │ [A]           │ {}         │
│  2   │ A               │ [B, C, D]     │ {A}        │
│  3   │ B               │ [E, F, C, D]  │ {A, B}     │
│  4   │ E               │ [F, C, D]     │ {A, B, E}  │
│  5   │ F               │ [C, D]        │ {A,B,E,F}  │
│  6   │ C               │ [D]           │ {A,B,E,F,C}│
│  7   │ D               │ [G, H]        │ {A,B,E,F,C,D}│
│  8   │ G               │ GOAL FOUND!   │            │
└──────┴─────────────────┴───────────────┴────────────┘

Solution Path: A → D → G
```

### DFS Properties

| Property | Value | Explanation |
|----------|-------|-------------|
| Complete | No | Can get stuck in infinite loops |
| Optimal | No | May find longer path first |
| Time | O(b^m) | m = maximum depth of tree |
| Space | O(bm) | Only stores current path |

---

## BFS vs DFS Comparison

| Criteria | BFS | DFS |
|----------|-----|-----|
| Data Structure | Queue (FIFO) | Stack (LIFO) |
| Exploration | Level by level | Branch by branch |
| Complete | Yes | No |
| Optimal | Yes (uniform cost) | No |
| Memory | High (O(b^d)) | Low (O(bm)) |
| Best for | Shortest path | Memory-limited search |

---

# Optimization Problems in AI

## Definition

An **optimization problem** in AI is a problem where we want to find the **best** solution among many possible solutions, according to an **objective function**.

Typical goal:
- **Minimize** cost (time, distance, errors) or
- **Maximize** reward/utility (profit, accuracy, performance)

## Core Components

| Component | Meaning | Example (Exam Timetable) |
|----------|---------|--------------------------|
| Decision Variables | What you choose/assign | Timeslot for each exam |
| Objective Function | What “best” means | Minimize student clashes |
| Constraints | What must be satisfied | Two exams can’t share a slot |

## Examples in AI

- Route/Path optimization (shortest or cheapest route)
- Exam timetabling (no conflicts, minimize gaps)
- Resource allocation (assign limited resources efficiently)
- Feature selection / model tuning (maximize accuracy)

## Global vs Local Optimum

- **Global optimum:** best overall solution.
- **Local optimum:** best within a neighborhood, but not necessarily best overall.

Genetic Algorithms are commonly used when the search space is large and we want a strong general-purpose approach to reach good (often near-optimal) solutions.

---

# Genetic Algorithms

## Definition

**Genetic Algorithm (GA)** is a population-based search technique inspired by natural evolution. It uses selection, crossover, and mutation to evolve solutions over generations.

## Key Terminology

| Term | Definition |
|------|------------|
| **Chromosome** | Encoded solution (string of genes) |
| **Gene** | Single element of chromosome |
| **Population** | Set of chromosomes |
| **Fitness** | Quality measure of a solution |
| **Generation** | One iteration of the algorithm |
| **Selection** | Choosing parents for reproduction |
| **Crossover** | Combining two parents to create offspring |
| **Mutation** | Random change in chromosome |

## Genetic Algorithm Flowchart

```
┌─────────────────────────────────────────────────┐
│   Initialize Random Population  │
└────────────────┬────────────────┘
                 ▼
┌─────────────────────────────────────────────────┐
│   Evaluate Fitness of Each      │
│   Chromosome                    │
└────────────────┬────────────────┘
                 ▼
        ┌────────────────┐
        │ Termination    │──Yes──→ Return Best Solution
        │ Condition Met? │
        └───────┬────────┘
                │ No
                ▼
┌─────────────────────────────────────────────────┐
│   Selection: Choose Parents     │
│   (Roulette/Tournament)         │
└────────────────┬────────────────┘
                 ▼
┌─────────────────────────────────────────────────┐
│   Crossover: Create Offspring   │
│   (Single-point/Two-point)      │
└────────────────┬────────────────┘
                 ▼
┌─────────────────────────────────────────────────┐
│   Mutation: Random Changes      │
│   (Bit-flip/Swap)               │
└────────────────┬────────────────┘
                 ▼
┌─────────────────────────────────────────────────┐
│   Replace Old Population        │
│   with New Generation           │
└────────────────┬────────────────┘
                 │
                 └──────→ (Loop back to Fitness Evaluation)
```

## GA Components Explained

### 1. Chromosome Encoding

```
Binary Encoding:
Chromosome: [1, 0, 1, 1, 0, 1, 0, 0]
            └─ Each bit represents a decision

Permutation Encoding (for ordering problems):
Chromosome: [3, 1, 4, 2, 5]
            └─ Order of cities to visit

Value Encoding:
Chromosome: [2.5, 3.7, 1.2, 4.8]
            └─ Actual parameter values
```

### 2. Fitness Function

```
Fitness Function evaluates how good a solution is.

Example - Delivery Route:
┌─────────────────────────────────────────────────┐
│ Route: A → C → B → D → A                        │
│ Total Distance: 50 km                           │
│ Fitness = 1 / Distance = 1/50 = 0.02            │
│                                                 │
│ Route: A → B → C → D → A                        │
│ Total Distance: 35 km                           │
│ Fitness = 1 / Distance = 1/35 = 0.029 (Better!) │
└─────────────────────────────────────────────────┘
```

### 3. Selection Methods

#### Roulette Wheel Selection

```
Chromosome  Fitness  Probability  Wheel Section
    A         10      10/30=33%   ████████
    B         15      15/30=50%   ████████████
    C          5       5/30=17%   ████
              ──
Total:        30       100%

Spin the wheel → Higher fitness = Higher chance of selection
```

#### Tournament Selection

```
Tournament Size = 3

Round 1: Pick 3 random chromosomes
         [A(fit=10), C(fit=5), D(fit=8)]
         Winner: A (highest fitness)

Round 2: Pick 3 random chromosomes
         [B(fit=15), E(fit=12), A(fit=10)]
         Winner: B (highest fitness)

Parents for crossover: A and B
```

### 4. Crossover Operations

#### Single-Point Crossover

```
Parent 1:  [1 0 1 | 1 0 1 0 0]
Parent 2:  [0 1 0 | 0 1 0 1 1]
                 ↑
           Crossover Point

Child 1:   [1 0 1 | 0 1 0 1 1]  (P1 left + P2 right)
Child 2:   [0 1 0 | 1 0 1 0 0]  (P2 left + P1 right)
```

#### Two-Point Crossover

```
Parent 1:  [1 0 | 1 1 0 | 1 0 0]
Parent 2:  [0 1 | 0 0 1 | 0 1 1]
               ↑       ↑
         Point 1    Point 2

Child 1:   [1 0 | 0 0 1 | 1 0 0]  (P1-P2-P1)
Child 2:   [0 1 | 1 1 0 | 0 1 1]  (P2-P1-P2)
```

#### Order Crossover (for Permutations)

```
Parent 1:  [1 2 3 | 4 5 | 6 7 8 9]
Parent 2:  [9 3 7 | 8 2 | 4 1 5 6]

Step 1: Copy segment from P1 to Child
Child:     [_ _ _ | 4 5 | _ _ _ _]

Step 2: Fill remaining from P2 (in order, skip 4,5)
P2 order: 9 3 7 8 2 1 6 (excluding 4, 5)

Child:     [9 3 7 | 4 5 | 8 2 1 6]
```

### 5. Mutation Operations

#### Bit-Flip Mutation

```
Before: [1 0 1 1 0 1 0 0]
                   ↑
             Mutation point

After:  [1 0 1 1 0 0 0 0]  (1 flipped to 0)
```

#### Swap Mutation (for Permutations)

```
Before: [3 1 4 2 5]
           ↑   ↑
        Swap positions

After:  [3 2 4 1 5]
```

---

## Complete GA Solved Example: Delivery Route Optimization

### Problem Statement

A delivery company wants to optimize routes for deliveries. Find the shortest route visiting cities A, B, C, D and returning to A.

### Distance Matrix

```
     A    B    C    D
A    0   10   15   20
B   10    0   35   25
C   15   35    0   30
D   20   25   30    0
```

### Step 1: Initialize Population (4 chromosomes)

```
Chromosome Encoding: Permutation of cities (excluding start A)
Example: [B, C, D] means route A → B → C → D → A

Initial Population:
┌──────┬─────────────┬─────────────────────┐
│ Chr# │ Chromosome  │ Route               │
├──────┼─────────────┼─────────────────────┤
│  C1  │ [B, C, D]   │ A → B → C → D → A   │
│  C2  │ [B, D, C]   │ A → B → D → C → A   │
│  C3  │ [C, B, D]   │ A → C → B → D → A   │
│  C4  │ [D, C, B]   │ A → D → C → B → A   │
└──────┴─────────────┴─────────────────────┘
```

### Step 2: Calculate Fitness

```
Fitness = 1 / Total Distance (to maximize)

C1: A→B→C→D→A = 10 + 35 + 30 + 20 = 95 km
    Fitness = 1/95 = 0.0105

C2: A→B→D→C→A = 10 + 25 + 30 + 15 = 80 km
    Fitness = 1/80 = 0.0125

C3: A→C→B→D→A = 15 + 35 + 25 + 20 = 95 km
    Fitness = 1/95 = 0.0105

C4: A→D→C→B→A = 20 + 30 + 35 + 10 = 95 km
    Fitness = 1/95 = 0.0105

Fitness Table:
┌──────┬──────────┬─────────┬─────────────┐
│ Chr# │ Distance │ Fitness │ Probability │
├──────┼──────────┼─────────┼─────────────┤
│  C1  │   95     │ 0.0105  │   24%       │
│  C2  │   80     │ 0.0125  │   28%       │ ← Best
│  C3  │   95     │ 0.0105  │   24%       │
│  C4  │   95     │ 0.0105  │   24%       │
├──────┼──────────┼─────────┼─────────────┤
│Total │    -     │ 0.0440  │   100%      │
└──────┴──────────┴─────────┴─────────────┘
```

### Step 3: Selection (Tournament, k=2)

```
Tournament 1: C1 vs C3
  C1 fitness = 0.0105
  C3 fitness = 0.0105
  Winner: C1 (tie, pick first)

Tournament 2: C2 vs C4
  C2 fitness = 0.0125
  C4 fitness = 0.0105
  Winner: C2

Selected Parents: C1 [B, C, D] and C2 [B, D, C]
```

### Step 4: Crossover (Order Crossover)

```
Parent 1: [B | C | D]
Parent 2: [B | D | C]
              ↑
        Crossover point

Child 1: Keep C from P1, fill rest from P2 order
         [B | C | D]  (B, D from P2, skip C)
         Result: [B, C, D]

Child 2: Keep D from P2, fill rest from P1 order
         [B | D | C]  (B, C from P1, skip D)
         Result: [B, D, C]

Offspring: [B, C, D] and [B, D, C]
```

### Step 5: Mutation (Swap, probability = 20%)

```
Child 1: [B, C, D]
Random number = 0.15 < 0.20 → Mutate!
Swap positions 1 and 2:
After mutation: [B, D, C]

Child 2: [B, D, C]
Random number = 0.85 > 0.20 → No mutation
After mutation: [B, D, C]

New offspring: [B, D, C] and [B, D, C]
```

### Step 6: New Generation

```
New Population (Generation 2):
┌──────┬─────────────┬──────────┬─────────┐
│ Chr# │ Chromosome  │ Distance │ Fitness │
├──────┼─────────────┼──────────┼─────────┤
│  C1  │ [B, D, C]   │   80     │ 0.0125  │
│  C2  │ [B, D, C]   │   80     │ 0.0125  │
│  C3  │ [B, C, D]   │   95     │ 0.0105  │
│  C4  │ [C, B, D]   │   95     │ 0.0105  │
└──────┴─────────────┴──────────┴─────────┘

Best solution so far: [B, D, C] with distance 80 km
Route: A → B → D → C → A
```

### Termination

After several generations, if no improvement or max generations reached:

```
Final Solution:
Route: A → B → D → C → A
Total Distance: 80 km

This is the optimal route for this small example!
```

---

## Why Genetic Algorithm vs Other Methods?

### GA vs Brute Force

| Aspect | Brute Force | Genetic Algorithm |
|--------|-------------|-------------------|
| Approach | Try all solutions | Evolve good solutions |
| Time (n cities) | O(n!) | O(generations × population) |
| 10 cities | 3,628,800 routes | ~1000 evaluations |
| 20 cities | 2.4 × 10^18 routes | ~5000 evaluations |
| Optimal | Always | Usually near-optimal |
| Scalability | Poor | Good |

### Genetic Algorithms vs Uninformed Search Techniques

| Aspect | Uninformed Search (BFS/DFS/UCS) | Genetic Algorithm |
|--------|----------------------------------|------------------|
| Goal | Find a path/goal state | Optimize an objective (fitness) |
| Search style | Systematic exploration | Population-based evolutionary search |
| Guarantees | Can be complete/optimal under conditions | No guarantee of optimality |
| Best for | Explicit goal + manageable state space | Large search space + tough optimization |
| Output | One solution path | Best solution found after generations |

---

# Comparative Analysis

## Genetic Algorithms vs Uninformed Search (Summary)

| Approach | Complete? | Optimal? | Typical Time | Typical Space |
|---------|-----------|----------|--------------|---------------|
| BFS | Yes (finite b) | Yes (uniform step cost) | O(b^d) | O(b^d) |
| DFS | Not always | No | O(b^m) | O(bm) |
| UCS | Yes (costs > 0) | Yes | Exponential (worst case) | Exponential (worst case) |
| Genetic Algorithm | Not guaranteed | Not guaranteed | O(generations × population) | O(population) |

*Under specific conditions (finite branching, uniform cost)

## When to Use Which Approach?

```
Decision Guide:

Do you need a path to a specific goal state?
├── Yes → Use uninformed search (BFS/DFS/UCS) depending on constraints
└── No  → Is it an optimization problem (maximize/minimize an objective)?
         └── Yes → Use Genetic Algorithm
```

### Genetic Algorithm vs Greedy (Common Exam Comparison)

Some exams compare **Genetic Algorithms** with a **greedy strategy** (pick the locally best choice at each step). Even though greedy is not an uninformed search algorithm, the comparison is useful for optimization problems.

| Aspect | Greedy Strategy | Genetic Algorithm |
|--------|------------------|------------------|
| Decision style | Locally best step-by-step | Population-based exploration |
| Local optimum risk | High | Lower (mutation + diversity can escape) |
| Solution diversity | One path built incrementally | Many candidate solutions in parallel |
| Best for | Simple/structured objectives | Large, multi-constraint, noisy objectives |

---

# Solved Exam Questions

## Question Type 2: Genetic Algorithm for Route Optimization

### Problem

A delivery company wants to optimize daily delivery routes for multiple vehicles while minimizing fuel consumption and delivery time. Routes differ in distance, traffic, and number of deliveries.

Using a Genetic Algorithm:

a) Describe how a route plan can be encoded as a chromosome and evaluated using a fitness function.
b) Explain the roles of selection, crossover, and mutation.
c) Justify why a Genetic Algorithm is preferable over brute-force search.

### Solution

#### Part (a): Chromosome Encoding and Fitness Function

**Chromosome Encoding:**

```
Route Representation: Permutation encoding

Example: 5 delivery locations (L1, L2, L3, L4, L5)
Chromosome: [L3, L1, L5, L2, L4]

This represents the delivery order:
Depot → L3 → L1 → L5 → L2 → L4 → Depot

For multiple vehicles, we can use delimiters:
[L3, L1 | L5, L2 | L4]
Vehicle1  Vehicle2  Vehicle3
```

**Fitness Function:**

```
Fitness = 1 / (α × TotalDistance + β × TotalTime + γ × FuelCost)

Where:
- α, β, γ are weight factors
- Lower total cost = Higher fitness

Example Calculation:
Route: [L3, L1, L5, L2, L4]
- Total Distance: 45 km
- Total Time: 2.5 hours
- Fuel Cost: $15

If α=1, β=10, γ=2:
Cost = 1(45) + 10(2.5) + 2(15) = 45 + 25 + 30 = 100
Fitness = 1/100 = 0.01
```

#### Part (b): Genetic Operators

**Selection:**
```
Purpose: Choose better chromosomes for reproduction

Method: Tournament Selection (k=3)
1. Randomly pick 3 chromosomes
2. Select the one with highest fitness
3. Repeat to get second parent

This ensures fitter routes have higher chance of
contributing to next generation.
```

**Crossover:**
```
Purpose: Combine good features of two parent routes

Method: Order Crossover (OX)
Parent 1: [L3, L1, L5, L2, L4]
Parent 2: [L2, L4, L1, L3, L5]

Step 1: Select segment from P1
        [L3, |L1, L5|, L2, L4]

Step 2: Child inherits segment
        [_, |L1, L5|, _, _]

Step 3: Fill from P2 (maintaining order, skip L1, L5)
        P2 order: L2, L4, L3 (excluding L1, L5)

Child:  [L2, L1, L5, L4, L3]
```

**Mutation:**
```
Purpose: Introduce diversity, escape local optima

Method: Swap Mutation
Before: [L3, L1, L5, L2, L4]
              ↑       ↑
         Random positions

After:  [L3, L2, L5, L1, L4]

Mutation rate: Typically 1-5%
```

#### Part (c): GA vs Brute Force Justification

```
Comparison for n delivery locations:

┌─────────────────┬─────────────────┬─────────────────┐
│ Locations (n)   │ Brute Force     │ Genetic Algo    │
├─────────────────┼─────────────────┼─────────────────┤
│ 5               │ 120 routes      │ ~100 evals      │
│ 10              │ 3.6 million     │ ~500 evals      │
│ 15              │ 1.3 trillion    │ ~1000 evals     │
│ 20              │ 2.4 × 10^18     │ ~2000 evals     │
└─────────────────┴─────────────────┴─────────────────┘

Reasons GA is Better:

1. SCALABILITY
   - Brute force: O(n!) - factorial growth
   - GA: O(generations × population) - linear

2. REAL-WORLD CONSTRAINTS
   - Can incorporate traffic, time windows
   - Flexible fitness function

3. NEAR-OPTIMAL SOLUTIONS
   - Finds good solutions quickly
   - 95%+ optimality in reasonable time

4. PARALLEL PROCESSING
   - Population can be evaluated in parallel
   - Brute force is sequential

5. DYNAMIC ADAPTATION
   - Can re-run when conditions change
   - Brute force must restart completely
```

---

## Question Type 2 (Variant): Exam Timetable Optimization

### Problem

Optimize exam timetable generation using Genetic Algorithms.

### Solution

```
CHROMOSOME ENCODING:
Each gene represents an exam slot assignment

Exams: E1, E2, E3, E4, E5
Slots: S1 (Mon 9AM), S2 (Mon 2PM), S3 (Tue 9AM), S4 (Tue 2PM)

Chromosome: [S1, S3, S1, S2, S4]
            E1→S1, E2→S3, E3→S1, E4→S2, E5→S4

FITNESS FUNCTION:
Minimize conflicts and spread exams

Fitness = 1 / (α × Conflicts + β × ConsecutiveExams + γ × SameDay)

Conflict: Two exams with common students in same slot
Consecutive: Student has back-to-back exams
SameDay: Student has multiple exams in one day

EXAMPLE:
Chromosome: [S1, S3, S1, S2, S4]
- E1 and E3 both in S1 → If common students: Conflict!
- Fitness = 1 / (10×1 + 5×0 + 2×1) = 1/12 = 0.083

CROSSOVER:
Parent 1: [S1, S3, S1, S2, S4]
Parent 2: [S2, S1, S3, S4, S2]
Point: 3
Child 1:  [S1, S3, S1, S4, S2]

MUTATION:
Before: [S1, S3, S1, S2, S4]
After:  [S1, S4, S1, S2, S4]  (S3 → S4 at position 2)
```

---

## Question Type 2 (Variant): Scholarship Allocation

### Problem

A university wants to allocate scholarships to students based on GPA, financial need, and extracurriculars using Genetic Algorithms.

### Solution

```
PROBLEM SETUP:
- 10 scholarships to distribute
- 50 applicants
- Criteria: GPA (40%), Financial Need (35%), Extracurriculars (25%)

CHROMOSOME ENCODING:
Binary string of length 50
1 = Student gets scholarship
0 = Student doesn't get scholarship

Constraint: Exactly 10 ones in chromosome

Chromosome: [0,1,0,0,1,1,0,1,0,0,1,1,0,0,1,0,1,0,0,1,0,0,...]
            Students 2,5,6,8,11,12,15,17,20 get scholarships

FITNESS FUNCTION:
Maximize total benefit while meeting constraints

For each selected student i:
Score(i) = 0.4×GPA(i) + 0.35×Need(i) + 0.25×Extra(i)

Fitness = Σ Score(i) for all selected students

EXAMPLE:
Selected: Students 2, 5, 6
- Student 2: GPA=3.8, Need=0.7, Extra=0.6
  Score = 0.4(3.8) + 0.35(0.7) + 0.25(0.6) = 1.52+0.245+0.15 = 1.915

- Student 5: GPA=3.5, Need=0.9, Extra=0.4
  Score = 0.4(3.5) + 0.35(0.9) + 0.25(0.4) = 1.4+0.315+0.1 = 1.815

Total Fitness = 1.915 + 1.815 + ... (for all 10)

GENETIC OPERATORS:

Selection: Tournament (k=3)
Pick 3 chromosomes, select highest fitness

Crossover: Uniform crossover with repair
Parent 1: [0,1,0,0,1,1,0,1,0,0]
Parent 2: [1,0,1,0,0,1,1,0,0,0]
Child:    [0,0,0,0,1,1,1,0,0,0] → Only 3 ones, need repair!
Repair:   Add random 1s until count = 10

Mutation: Swap mutation
Find a 0 and a 1, swap them
Before: [0,1,0,0,1,1,0,1,0,0]
After:  [1,1,0,0,0,1,0,1,0,0]  (positions 0 and 4 swapped)

WHY GA OVER GREEDY:
- Greedy: Always picks current best student
  Problem: May miss optimal combination
  
- GA: Explores multiple combinations simultaneously
  Benefit: Finds globally optimal allocation

Example:
Greedy picks top 10 by individual score
But GA might find that certain combinations
work better due to diversity requirements
```

---

## Question Type 2 (Variant): Energy Consumption Optimization

### Problem

Optimize energy consumption schedules using Genetic Algorithms for a smart home.

### Solution

```
PROBLEM:
Schedule appliances (Washer, Dryer, Dishwasher, EV Charger)
across 24 hours to minimize electricity cost.

ELECTRICITY RATES:
Off-Peak (11PM-7AM): $0.08/kWh
Mid-Peak (7AM-11AM, 5PM-11PM): $0.12/kWh
On-Peak (11AM-5PM): $0.18/kWh

APPLIANCE REQUIREMENTS:
┌────────────┬───────────┬──────────┬─────────────┐
│ Appliance  │ Duration  │ kWh/hour │ Constraints │
├────────────┼───────────┼──────────┼─────────────┤
│ Washer     │ 2 hours   │ 0.5      │ Before 8PM  │
│ Dryer      │ 1 hour    │ 3.0      │ After Washer│
│ Dishwasher │ 1.5 hours │ 1.8      │ After 7PM   │
│ EV Charger │ 4 hours   │ 7.0      │ By 7AM      │
└────────────┴───────────┴──────────┴─────────────┘

CHROMOSOME ENCODING:
Start times for each appliance (in hours, 0-23)

Chromosome: [14, 16, 21, 2]
            Washer@2PM, Dryer@4PM, Dishwasher@9PM, EV@2AM

FITNESS FUNCTION:
Calculate total electricity cost

Washer: 2PM-4PM (On-Peak) = 2 × 0.5 × $0.18 = $0.18
Dryer: 4PM-5PM (On-Peak) = 1 × 3.0 × $0.18 = $0.54
Dishwasher: 9PM-10:30PM (Mid-Peak) = 1.5 × 1.8 × $0.12 = $0.324
EV: 2AM-6AM (Off-Peak) = 4 × 7.0 × $0.08 = $2.24

Total Cost = $3.284
Fitness = 1 / 3.284 = 0.305

BETTER CHROMOSOME: [23, 1, 21, 2]
Washer: 11PM-1AM (Off-Peak) = 2 × 0.5 × $0.08 = $0.08
Dryer: 1AM-2AM (Off-Peak) = 1 × 3.0 × $0.08 = $0.24
Dishwasher: 9PM-10:30PM (Mid-Peak) = $0.324
EV: 2AM-6AM (Off-Peak) = $2.24

Total Cost = $2.884
Fitness = 1 / 2.884 = 0.347 (Better!)

CROSSOVER:
Parent 1: [14, 16, 21, 2]
Parent 2: [23, 1, 19, 3]
Child:    [14, 1, 21, 3]  (single-point at position 1)

MUTATION:
Before: [14, 16, 21, 2]
After:  [14, 16, 22, 2]  (Dishwasher moved 1 hour)

Check constraints after mutation!
```

---

## Practice Problems

### Problem 1: BFS/DFS

```
Given the tree below, show BFS and DFS traversal:

        1
       /|\
      2 3 4
     /|   |
    5 6   7
       |
       8

BFS Order: _______________
DFS Order: _______________
```

### Problem 2: Genetic Algorithm

```
Maximize f(x) = x² where x is a 4-bit binary number (0-15)

Initial population: [0110, 1001, 0100, 1100]

Calculate fitness and perform:
1. Roulette wheel selection
2. Single-point crossover
3. Bit-flip mutation (1 bit)
```

---

## Key Formulas Summary

```
BFS/DFS:
- Time Complexity (BFS): O(b^d)
- Space Complexity (BFS): O(b^d)
- Time Complexity (DFS): O(b^m)
- Space Complexity (DFS): O(bm)

Genetic Algorithm:
- Fitness proportionate selection: P(i) = f(i) / Σf(j)
- Crossover rate: typically 0.6-0.9
- Mutation rate: typically 0.01-0.05
```

---

## Quick Reference: Algorithm Selection

```
┌─────────────────────────────────────────────────────────────┐
│                    ALGORITHM SELECTION                       │
├─────────────────────────────────────────────────────────────┤
│ Need shortest path + small state space     → BFS            │
│ Memory is limited                          → DFS            │
│ Complex optimization, need global search   → Genetic Algo   │
│ Need fast approximate solution             → Genetic Algo   │
└─────────────────────────────────────────────────────────────┘
```