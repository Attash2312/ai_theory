# Module 3: Constraint Satisfaction Problems (CSP)

## Table of Contents
1. [Introduction to CSP](#introduction-to-csp)
2. [CSP Components](#csp-components)
3. [Types of Constraints](#types-of-constraints)
4. [CSP Formulation from Natural Language](#csp-formulation-from-natural-language)
5. [Consistency and Solutions](#consistency-and-solutions)
6. [CSP Solving Techniques](#csp-solving-techniques)
7. [Solved Exam Questions](#solved-exam-questions)
8. [Practice Problems](#practice-problems)

---

# Introduction to CSP

## What is a Constraint Satisfaction Problem?

A **Constraint Satisfaction Problem (CSP)** is a mathematical problem where:
- We have a set of **variables**
- Each variable has a **domain** of possible values
- There are **constraints** that restrict which combinations of values are allowed

## Definition

```
A CSP is defined as a triple (X, D, C) where:

X = {X₁, X₂, ..., Xₙ}     → Set of variables
D = {D₁, D₂, ..., Dₙ}     → Set of domains (one per variable)
C = {C₁, C₂, ..., Cₘ}     → Set of constraints
```

## Why CSP?

```
┌─────────────────────────────────────────────────────────────┐
│                    WHY USE CSP?                              │
├─────────────────────────────────────────────────────────────┤
│ • Natural way to represent many real-world problems         │
│ • Separates problem structure from solving method            │
│ • Enables constraint propagation (early pruning)             │
│ • General-purpose solvers available                          │
│ • Declarative: specify WHAT, not HOW                         │
└─────────────────────────────────────────────────────────────┘
```

## Real-World CSP Examples

| Problem | Variables | Domain | Constraints |
|---------|-----------|--------|-------------|
| Map Coloring | Regions | Colors | Adjacent regions ≠ same color |
| Sudoku | Cells | 1-9 | Row, column, box uniqueness |
| Exam Scheduling | Exams | Time slots | No conflicts, room capacity |
| Job Scheduling | Tasks | Time slots | Precedence, resource limits |
| Seating Arrangement | Guests | Seats | Preferences, conflicts |

---

# CSP Components

## 1. Variables

**Variables** are the unknowns we need to assign values to.

```
Notation: X = {X₁, X₂, X₃, ..., Xₙ}

Example - Map Coloring:
Variables = {WA, NT, SA, Q, NSW, V, T}
(Australian states/territories)

Example - Exam Scheduling:
Variables = {Exam_Math, Exam_Physics, Exam_Chemistry, Exam_Biology}
```

## 2. Domains

**Domains** are the set of possible values each variable can take.

```
Notation: D = {D₁, D₂, D₃, ..., Dₙ}
          where Dᵢ is the domain for variable Xᵢ

Example - Map Coloring:
D(WA) = D(NT) = D(SA) = ... = {Red, Green, Blue}

Example - Exam Scheduling:
D(Exam_Math) = {Monday_9AM, Monday_2PM, Tuesday_9AM, Tuesday_2PM}

Example - Sudoku:
D(Cell_1_1) = {1, 2, 3, 4, 5, 6, 7, 8, 9}
```

### Domain Types

| Type | Description | Example |
|------|-------------|---------|
| Finite Discrete | Countable, limited values | Colors, time slots |
| Infinite Discrete | Countable, unlimited | Integers |
| Continuous | Uncountable | Real numbers |

## 3. Constraints

**Constraints** define the restrictions on variable assignments.

```
A constraint Cᵢ consists of:
• Scope: Variables involved in the constraint
• Relation: Allowed combinations of values

Example:
Constraint: WA ≠ NT
Scope: {WA, NT}
Relation: All pairs (a, b) where a ≠ b
          {(R,G), (R,B), (G,R), (G,B), (B,R), (B,G)}
```

---

# Types of Constraints

## 1. Unary Constraints

**Unary constraints** involve only ONE variable.

```
Definition: Restricts values of a single variable

Format: Cᵢ(Xⱼ)

Examples:
┌─────────────────────────────────────────────────────────────┐
│ • SA ≠ Green        (SA cannot be green)                    │
│ • X > 0             (X must be positive)                    │
│ • Exam_Math ≠ Monday_9AM  (Math exam not on Monday 9AM)     │
│ • Employee1 ≠ Night_Shift (E1 cannot work night)            │
└─────────────────────────────────────────────────────────────┘

Effect: Reduces domain of the variable
D(SA) = {Red, Green, Blue} → D(SA) = {Red, Blue}
```

## 2. Binary Constraints

**Binary constraints** involve exactly TWO variables.

```
Definition: Restricts combinations of two variables

Format: Cᵢ(Xⱼ, Xₖ)

Examples:
┌─────────────────────────────────────────────────────────────┐
│ • WA ≠ NT           (WA and NT must be different colors)    │
│ • X < Y             (X must be less than Y)                 │
│ • Exam_A ≠ Exam_B   (Exams A and B not at same time)        │
│ • E2 ≠ E3           (Employees 2 and 3 different shifts)    │
└─────────────────────────────────────────────────────────────┘

Representation: Constraint Graph
- Nodes = Variables
- Edges = Binary constraints between variables
```

### Constraint Graph Example

```
Map Coloring - Australia:

                    ┌────┐
                    │ NT │
                    └──┬─┘
                  ╱    │    ╲
              ╱        │        ╲
          ┌────┐    ┌──┴──┐    ┌────┐
          │ WA │────│ SA  │────│ Q  │
          └────┘    └──┬──┘    └──┬─┘
                       │          │
                   ┌───┴───┐      │
                   │  NSW  │──────┘
                   └───┬───┘
                       │
                   ┌───┴───┐
                   │   V   │
                   └───────┘
                   
                   ┌───────┐
                   │   T   │ (Tasmania - no edges, island)
                   └───────┘

Each edge represents constraint: "must be different colors"
```

## 3. Global Constraints (Higher-Order)

**Global constraints** involve MORE than two variables.

```
Definition: Restricts combinations of multiple variables

Format: Cᵢ(X₁, X₂, ..., Xₖ) where k > 2

Examples:
┌─────────────────────────────────────────────────────────────┐
│ AllDifferent(X₁, X₂, X₃, X₄)                                │
│   → All variables must have different values                │
│                                                              │
│ Sum(X₁, X₂, X₃) = 15                                        │
│   → Variables must sum to 15                                 │
│                                                              │
│ Exactly(2, [X₁,X₂,X₃,X₄], Red)                              │
│   → Exactly 2 variables must be Red                          │
│                                                              │
│ AtMost(3, [E1,E2,E3,E4,E5], MorningShift)                   │
│   → At most 3 employees on morning shift                     │
└─────────────────────────────────────────────────────────────┘
```

### Comparison Table

| Type | Variables | Example | Use Case |
|------|-----------|---------|----------|
| Unary | 1 | X ≠ 5 | Restricting domain |
| Binary | 2 | X ≠ Y | Pairwise differences |
| Global | 3+ | AllDiff(X,Y,Z) | Complex relationships |

---

# CSP Formulation from Natural Language

## Step-by-Step Process

```
┌─────────────────────────────────────────────────────────────┐
│           CSP FORMULATION PROCESS                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Step 1: IDENTIFY VARIABLES                                  │
│          What are the unknowns to determine?                 │
│                                                              │
│  Step 2: DEFINE DOMAINS                                      │
│          What values can each variable take?                 │
│                                                              │
│  Step 3: IDENTIFY CONSTRAINTS                                │
│          What restrictions exist?                            │
│          - Read carefully for keywords:                      │
│            "cannot", "must", "different", "same",            │
│            "before", "after", "at most", "exactly"           │
│                                                              │
│  Step 4: FORMALIZE CONSTRAINTS                               │
│          Write constraints in mathematical notation          │
│                                                              │
│  Step 5: VERIFY COMPLETENESS                                 │
│          Did you capture all requirements?                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Constraint Keywords Translation

| Natural Language | Constraint Type | Formal Notation |
|------------------|-----------------|-----------------|
| "cannot be" | Unary | X ≠ value |
| "must be" | Unary | X = value |
| "different from" | Binary | X ≠ Y |
| "same as" | Binary | X = Y |
| "before" | Binary | X < Y |
| "after" | Binary | X > Y |
| "at the same time" | Binary | X = Y |
| "not at the same time" | Binary | X ≠ Y |
| "all different" | Global | AllDiff(X, Y, Z, ...) |
| "at most k" | Global | AtMost(k, [...], val) |
| "exactly one" | Global | ExactlyOne([...]) |

---

## Formulation Example 1: Map Coloring

### Problem Statement

Color the map of Australia using three colors (Red, Green, Blue) such that no adjacent regions have the same color.

### CSP Formulation

```
STEP 1: Variables
X = {WA, NT, SA, Q, NSW, V, T}
(7 variables for 7 regions)

STEP 2: Domains
D(WA)  = {Red, Green, Blue}
D(NT)  = {Red, Green, Blue}
D(SA)  = {Red, Green, Blue}
D(Q)   = {Red, Green, Blue}
D(NSW) = {Red, Green, Blue}
D(V)   = {Red, Green, Blue}
D(T)   = {Red, Green, Blue}

STEP 3 & 4: Constraints (based on adjacency)
C1: WA ≠ NT    (Western Australia adjacent to Northern Territory)
C2: WA ≠ SA    (Western Australia adjacent to South Australia)
C3: NT ≠ SA    (Northern Territory adjacent to South Australia)
C4: NT ≠ Q     (Northern Territory adjacent to Queensland)
C5: SA ≠ Q     (South Australia adjacent to Queensland)
C6: SA ≠ NSW   (South Australia adjacent to New South Wales)
C7: SA ≠ V     (South Australia adjacent to Victoria)
C8: Q ≠ NSW    (Queensland adjacent to New South Wales)
C9: NSW ≠ V    (New South Wales adjacent to Victoria)

Note: T (Tasmania) has no constraints (island)

STEP 5: Verification
- 7 variables ✓
- All domains defined ✓
- 9 binary constraints for all adjacent pairs ✓
```

### One Valid Solution

```
┌─────────────────────────────────────────┐
│  WA = Red                               │
│  NT = Green                             │
│  SA = Blue                              │
│  Q = Red                                │
│  NSW = Green                            │
│  V = Red                                │
│  T = Red (or any color)                 │
└─────────────────────────────────────────┘

Verification:
WA(R) ≠ NT(G) ✓    NT(G) ≠ Q(R) ✓     Q(R) ≠ NSW(G) ✓
WA(R) ≠ SA(B) ✓    SA(B) ≠ Q(R) ✓     NSW(G) ≠ V(R) ✓
NT(G) ≠ SA(B) ✓    SA(B) ≠ NSW(G) ✓
                    SA(B) ≠ V(R) ✓
```

---

## Formulation Example 2: Exam Scheduling

### Problem Statement

Schedule 4 exams (Math, Physics, Chemistry, Biology) into 3 time slots (Morning, Afternoon, Evening).

Constraints:
- Math and Physics have common students (cannot be same slot)
- Chemistry must be in the Morning
- Biology and Chemistry have common students (cannot be same slot)
- Physics and Biology cannot be in the same slot

### CSP Formulation

```
STEP 1: Variables
X = {Math, Physics, Chemistry, Biology}

STEP 2: Domains
D(Math)      = {Morning, Afternoon, Evening}
D(Physics)   = {Morning, Afternoon, Evening}
D(Chemistry) = {Morning, Afternoon, Evening}
D(Biology)   = {Morning, Afternoon, Evening}

STEP 3 & 4: Constraints

Constraint 1 (from "Chemistry must be in Morning"):
   Type: Unary
   C1: Chemistry = Morning
   
   Effect: D(Chemistry) = {Morning}

Constraint 2 (from "Math and Physics have common students"):
   Type: Binary
   C2: Math ≠ Physics

Constraint 3 (from "Biology and Chemistry have common students"):
   Type: Binary
   C3: Biology ≠ Chemistry
   
   Since Chemistry = Morning:
   C3 becomes: Biology ≠ Morning
   Effect: D(Biology) = {Afternoon, Evening}

Constraint 4 (from "Physics and Biology cannot be same slot"):
   Type: Binary
   C4: Physics ≠ Biology

STEP 5: Summary
┌─────────────────────────────────────────────────────────────┐
│ Variables: {Math, Physics, Chemistry, Biology}              │
│                                                              │
│ Domains (after unary constraint propagation):               │
│   D(Math)      = {Morning, Afternoon, Evening}              │
│   D(Physics)   = {Morning, Afternoon, Evening}              │
│   D(Chemistry) = {Morning}                                  │
│   D(Biology)   = {Afternoon, Evening}                       │
│                                                              │
│ Constraints:                                                 │
│   C1: Chemistry = Morning        (Unary)                    │
│   C2: Math ≠ Physics             (Binary)                   │
│   C3: Biology ≠ Chemistry        (Binary)                   │
│   C4: Physics ≠ Biology          (Binary)                   │
└─────────────────────────────────────────────────────────────┘
```

### Finding a Solution

```
Fixed: Chemistry = Morning

Try: Biology = Afternoon
Then: Physics ≠ Afternoon (from C4)
So: Physics ∈ {Morning, Evening}

Try: Physics = Morning
Then: Math ≠ Morning (from C2)
So: Math ∈ {Afternoon, Evening}

Try: Math = Afternoon

Check all constraints:
C1: Chemistry = Morning ✓
C2: Math(Afternoon) ≠ Physics(Morning) ✓
C3: Biology(Afternoon) ≠ Chemistry(Morning) ✓
C4: Physics(Morning) ≠ Biology(Afternoon) ✓

SOLUTION FOUND:
┌─────────────────────────────────────────┐
│  Math      = Afternoon                  │
│  Physics   = Morning                    │
│  Chemistry = Morning                    │
│  Biology   = Afternoon                  │
└─────────────────────────────────────────┘

Wait - Physics and Chemistry both Morning?
This is allowed! They don't have common students.
```

---

## Formulation Example 3: Resource Allocation

### Problem Statement

Allocate 3 employees (E1, E2, E3) to 2 shifts (Morning, Evening) with these constraints:
- Each employee must be assigned exactly one shift
- At most 2 employees can be assigned to Morning (capacity constraint)
- E1 and E2 cannot work the same shift
- E3 cannot work Evening

### CSP Formulation

```
STEP 1: Variables
X = {E1, E2, E3}

STEP 2: Domains
D(E1) = {Morning, Evening}
D(E2) = {Morning, Evening}
D(E3) = {Morning, Evening}

STEP 3 & 4: Constraints
C1 (Binary): E1 ≠ E2
C2 (Unary):  E3 ≠ Evening   → E3 = Morning
C3 (Global): AtMost(2, [E1, E2, E3], Morning)

One valid solution:
E3 = Morning
E1 = Evening
E2 = Morning
```

---

# Consistency and Solutions

## Types of Assignments

### Complete Assignment

```
Definition: Every variable has a value assigned

Example:
{WA=Red, NT=Green, SA=Blue, Q=Red, NSW=Green, V=Red, T=Blue}

All 7 variables have values assigned ✓
```

### Partial Assignment

```
Definition: Only some variables have values assigned

Example:
{WA=Red, NT=Green, SA=?, Q=?, NSW=?, V=?, T=?}

Only 2 of 7 variables have values assigned
```

### Consistent Assignment

```
Definition: No constraint is violated by the assignment

Example (Consistent):
{WA=Red, NT=Green}
Check: WA ≠ NT? Red ≠ Green? ✓ YES - Consistent!

Example (Inconsistent):
{WA=Red, NT=Red}
Check: WA ≠ NT? Red ≠ Red? ✗ NO - Inconsistent!
```

### Solution

```
Definition: A complete assignment that is consistent
           (all variables assigned + no constraints violated)

A CSP can have:
• No solution (over-constrained)
• Exactly one solution
• Multiple solutions
```

## Checking Consistency

```
CONSISTENCY CHECK ALGORITHM:
────────────────────────────

function isConsistent(assignment):
    for each constraint C in CSP:
        variables_in_C = getScope(C)
        
        // Only check if all variables in scope are assigned
        if all variables_in_C are in assignment:
            if C is violated:
                return FALSE
    
    return TRUE
```

### Consistency Example

```
CSP: Map Coloring (simplified: 3 regions)
Variables: {A, B, C}
Constraints: A≠B, B≠C, A≠C

Assignment: {A=Red, B=Green, C=Red}

Check:
A ≠ B → Red ≠ Green → ✓
B ≠ C → Green ≠ Red → ✓
A ≠ C → Red ≠ Red → ✗ VIOLATION!

Result: INCONSISTENT (not a solution)
```

---

# CSP Solving Techniques

## 1. Backtracking Search

```
BACKTRACKING ALGORITHM:
───────────────────────

function BACKTRACK(assignment, csp):
    if assignment is complete:
        return assignment  // Solution found!
    
    var = SELECT-UNASSIGNED-VARIABLE(csp)
    
    for each value in DOMAIN(var):
        if value is consistent with assignment:
            assignment[var] = value
            result = BACKTRACK(assignment, csp)
            if result ≠ failure:
                return result
            assignment[var] = NULL  // Remove assignment
    
    return failure  // No solution in this branch
```

### Backtracking Example

```
CSP: 3 variables, domain {1, 2}, constraint: all different

Variables: X, Y, Z
Domain: {1, 2} for each
Constraint: AllDiff(X, Y, Z)

Backtracking Trace:
┌─────┬───────────────────────────────────────────────────────┐
│Step │ Action                                                │
├─────┼───────────────────────────────────────────────────────┤
│ 1   │ Select X, try X=1                                     │
│ 2   │ Select Y, try Y=1                                     │
│     │ Check: X≠Y? 1≠1? NO! Backtrack                        │
│ 3   │ Try Y=2                                               │
│     │ Check: X≠Y? 1≠2? YES! Continue                        │
│ 4   │ Select Z, try Z=1                                     │
│     │ Check: X≠Z? 1≠1? NO! Backtrack                        │
│ 5   │ Try Z=2                                               │
│     │ Check: Y≠Z? 2≠2? NO! Backtrack                        │
│ 6   │ No more values for Z, backtrack to Y                  │
│ 7   │ No more values for Y, backtrack to X                  │
│ 8   │ Try X=2                                               │
│ 9   │ Select Y, try Y=1                                     │
│     │ Check: X≠Y? 2≠1? YES! Continue                        │
│ 10  │ Select Z, try Z=1                                     │
│     │ Check: Y≠Z? 1≠1? NO! Backtrack                        │
│ 11  │ Try Z=2                                               │
│     │ Check: X≠Z? 2≠2? NO! Backtrack                        │
│ 12  │ No more values for Z, backtrack to Y                  │
│ 13  │ Try Y=2                                               │
│     │ Check: X≠Y? 2≠2? NO! Backtrack                        │
│ 14  │ No more values for Y, backtrack to X                  │
│ 15  │ No more values for X                                  │
│ 16  │ FAILURE - No solution exists!                         │
└─────┴───────────────────────────────────────────────────────┘

Result: NO SOLUTION
Reason: AllDiff needs at least 3 different values, but domain only has 2
```

## 2. Constraint Propagation

### Arc Consistency (AC-3)

```
ARC CONSISTENCY:
────────────────
An arc (Xᵢ, Xⱼ) is consistent if:
For every value in D(Xᵢ), there exists a value in D(Xⱼ)
that satisfies the constraint between them.

AC-3 ALGORITHM:
───────────────
1. Create queue of all arcs
2. While queue not empty:
   a. Remove arc (Xᵢ, Xⱼ)
   b. If REVISE(Xᵢ, Xⱼ) removes values:
      - If D(Xᵢ) is empty: return FAILURE
      - Add all arcs (Xₖ, Xᵢ) to queue (k≠j)
3. Return domains
```

### Arc Consistency Example

```
Variables: X, Y
Domains: D(X) = {1, 2, 3}, D(Y) = {1, 2, 3}
Constraint: X < Y

Check arc (X, Y):
For X=1: Is there Y where 1<Y? Y∈{2,3} works ✓
For X=2: Is there Y where 2<Y? Y=3 works ✓
For X=3: Is there Y where 3<Y? No Y works! Remove 3 from D(X)

D(X) = {1, 2}

Check arc (Y, X):
For Y=1: Is there X where X<1? No X works! Remove 1 from D(Y)
For Y=2: Is there X where X<2? X=1 works ✓
For Y=3: Is there X where X<3? X∈{1,2} works ✓

D(Y) = {2, 3}

Final domains after AC-3:
D(X) = {1, 2}
D(Y) = {2, 3}
```

---

# Solved Exam Questions

## Exam Question 1: Employee Shift Assignment

### Problem Statement

Four employees (E1, E2, E3, E4) must be assigned to four shifts (Morning, Evening, Night, Off).

Constraints:
- E1 cannot work Night
- E2 and E3 cannot work the same shift
- E4 must work Evening
- Each employee gets exactly one shift

Formulate the problem as a CSP and determine if a solution exists.

### Complete Solution

```
STEP 1: IDENTIFY VARIABLES
──────────────────────────
Variables represent the shift assignment for each employee:

X = {E1, E2, E3, E4}

STEP 2: DEFINE DOMAINS
──────────────────────
Each employee can be assigned to any of the four shifts:

D(E1) = {Morning, Evening, Night, Off}
D(E2) = {Morning, Evening, Night, Off}
D(E3) = {Morning, Evening, Night, Off}
D(E4) = {Morning, Evening, Night, Off}

STEP 3: FORMALIZE CONSTRAINTS
─────────────────────────────

Constraint 1: "E1 cannot work Night"
   Type: Unary
   C1: E1 ≠ Night
   Effect: D(E1) = {Morning, Evening, Off}

Constraint 2: "E2 and E3 cannot work the same shift"
   Type: Binary
   C2: E2 ≠ E3

Constraint 3: "E4 must work Evening"
   Type: Unary
   C3: E4 = Evening
   Effect: D(E4) = {Evening}

Constraint 4: "Each employee gets exactly one shift"
   This is implicit in our representation (each variable 
   gets exactly one value). No additional constraint needed.

Note: The problem does NOT say each shift must have exactly
one employee, so multiple employees can have the same shift,
and some shifts can be empty.

STEP 4: UPDATED DOMAINS (after unary constraints)
─────────────────────────────────────────────────
D(E1) = {Morning, Evening, Off}
D(E2) = {Morning, Evening, Night, Off}
D(E3) = {Morning, Evening, Night, Off}
D(E4) = {Evening}

STEP 5: FIND A SOLUTION
───────────────────────

Fixed: E4 = Evening

For E1: Choose from {Morning, Evening, Off}
   Let E1 = Morning

For E2: Choose from {Morning, Evening, Night, Off}
   Let E2 = Night

For E3: Must be ≠ E2 (Night)
   Choose from {Morning, Evening, Off}
   Let E3 = Evening

Check all constraints:
C1: E1 ≠ Night → Morning ≠ Night ✓
C2: E2 ≠ E3 → Night ≠ Evening ✓
C3: E4 = Evening ✓

SOLUTION EXISTS!
┌─────────────────────────────────────────┐
│  E1 = Morning                           │
│  E2 = Night                             │
│  E3 = Evening                           │
│  E4 = Evening                           │
└─────────────────────────────────────────┘

Note: E3 and E4 both work Evening - this is allowed since
there's no constraint against it.
```

### Alternative Solutions

```
Other valid solutions:

Solution 2:          Solution 3:          Solution 4:
E1 = Morning         E1 = Off             E1 = Evening
E2 = Morning         E2 = Morning         E2 = Off
E3 = Evening         E3 = Night           E3 = Morning
E4 = Evening         E4 = Evening         E4 = Evening

All satisfy:
- E1 ≠ Night ✓
- E2 ≠ E3 ✓
- E4 = Evening ✓
```

---

## Exam Question 2: Meeting Room Scheduling

### Problem Statement

Five meetings (M1, M2, M3, M4, M5) must be scheduled in three rooms (R1, R2, R3) with time constraints.

Constraints:
- M1 and M2 have overlapping participants (cannot be same room)
- M2 and M3 are at the same time (cannot be same room)
- M3 and M4 have overlapping participants (cannot be same room)
- M4 and M5 must be in the same room
- M1 must be in R1

Formulate as CSP and analyze consistency.

### Complete Solution

```
STEP 1: VARIABLES
─────────────────
X = {M1, M2, M3, M4, M5}

STEP 2: DOMAINS
───────────────
D(M1) = D(M2) = D(M3) = D(M4) = D(M5) = {R1, R2, R3}

STEP 3: CONSTRAINTS
───────────────────

C1: M1 ≠ M2     (overlapping participants)
C2: M2 ≠ M3     (same time)
C3: M3 ≠ M4     (overlapping participants)
C4: M4 = M5     (must be same room)
C5: M1 = R1     (unary constraint)

STEP 4: APPLY UNARY CONSTRAINTS
───────────────────────────────
After C5: D(M1) = {R1}

STEP 5: CONSTRAINT PROPAGATION
──────────────────────────────

From C1 (M1 ≠ M2) and M1 = R1:
   M2 ≠ R1
   D(M2) = {R2, R3}

STEP 6: FIND SOLUTION USING BACKTRACKING
────────────────────────────────────────

M1 = R1 (fixed)

Try M2 = R2:
   From C2: M3 ≠ R2, so D(M3) = {R1, R3}
   
   Try M3 = R1:
      From C3: M4 ≠ R1, so D(M4) = {R2, R3}
      From C4: M5 = M4
      
      Try M4 = R2, then M5 = R2
      
      Check all constraints:
      C1: M1(R1) ≠ M2(R2) ✓
      C2: M2(R2) ≠ M3(R1) ✓
      C3: M3(R1) ≠ M4(R2) ✓
      C4: M4(R2) = M5(R2) ✓
      C5: M1 = R1 ✓
      
      SOLUTION FOUND!

SOLUTION:
┌─────────────────────────────────────────┐
│  M1 = R1                                │
│  M2 = R2                                │
│  M3 = R1                                │
│  M4 = R2                                │
│  M5 = R2                                │
└─────────────────────────────────────────┘

CONSTRAINT GRAPH:
                M1 ─────── M2
                           │
                           │ (≠)
                           │
                M4 ─────── M3
                 │
                 │ (=)
                 │
                M5
```

---

## Exam Question 3: Project Team Assignment

### Problem Statement

Assign four projects (P1, P2, P3, P4) to four teams (T1, T2, T3, T4) with skill and dependency constraints.

Constraints:
- P1 requires AI skills (only T1 and T3 have AI skills)
- P2 and P3 are related and must be handled by consecutive teams (|team numbers differ by 1|)
- P4 cannot be assigned to T4
- Each project gets exactly one team
- Each team gets exactly one project (all different)

### Complete Solution

```
STEP 1: VARIABLES
─────────────────
X = {P1, P2, P3, P4}
Each variable represents which team gets that project.

STEP 2: INITIAL DOMAINS
────────────────────────
D(P1) = {T1, T2, T3, T4}
D(P2) = {T1, T2, T3, T4}
D(P3) = {T1, T2, T3, T4}
D(P4) = {T1, T2, T3, T4}

STEP 3: CONSTRAINTS
───────────────────

C1: P1 ∈ {T1, T3}              (AI skills - Unary)
C2: |P2 - P3| = 1              (Consecutive teams - Binary)
    Meaning: (P2,P3) ∈ {(T1,T2),(T2,T1),(T2,T3),(T3,T2),(T3,T4),(T4,T3)}
C3: P4 ≠ T4                    (Unary)
C4: AllDifferent(P1,P2,P3,P4)  (Global - each team one project)

STEP 4: APPLY UNARY CONSTRAINTS
───────────────────────────────
D(P1) = {T1, T3}        (from C1)
D(P2) = {T1, T2, T3, T4}
D(P3) = {T1, T2, T3, T4}
D(P4) = {T1, T2, T3}    (from C3)

STEP 5: SOLVE WITH BACKTRACKING
───────────────────────────────

Case 1: Try P1 = T1

   For AllDifferent: P2, P3, P4 ∈ {T2, T3, T4}
   But P4 ≠ T4, so P4 ∈ {T2, T3}
   
   For C2 (consecutive): With P2, P3 from {T2, T3, T4}:
   Valid pairs: (T2,T3), (T3,T2), (T3,T4), (T4,T3)
   
   Try P2 = T2, P3 = T3:
      Remaining for P4: {T4} but P4 ≠ T4! FAIL
   
   Try P2 = T3, P3 = T2:
      Remaining for P4: {T4} but P4 ≠ T4! FAIL
   
   Try P2 = T3, P3 = T4:
      Remaining for P4: {T2}
      P4 = T2 ✓
      
      Check AllDifferent: T1, T3, T4, T2 - all different ✓
      Check C2: |T3-T4| = 1 ✓
      Check C3: P4=T2 ≠ T4 ✓
      
      SOLUTION FOUND!

SOLUTION:
┌─────────────────────────────────────────┐
│  P1 = T1  (AI skills ✓)                 │
│  P2 = T3                                │
│  P3 = T4                                │
│  P4 = T2                                │
└─────────────────────────────────────────┘

Verification:
C1: P1 = T1 ∈ {T1, T3} ✓
C2: |3-4| = 1 ✓
C3: P4 = T2 ≠ T4 ✓
C4: AllDiff(T1, T3, T4, T2) ✓
```

---

## Exam Question 4: Classroom Allocation

### Problem Statement

Four courses (C1, C2, C3, C4) must be allocated to three classrooms (Room A, Room B, Room C).

Constraints:
- C1 and C2 are at the same time (cannot be same room)
- C2 and C3 have same professor (cannot be same time, hence same room is OK only if different times - assume different times, so no constraint)
- C3 and C4 have overlapping students (cannot be same room)
- C1 requires a lab (only Room C is a lab)
- Room B can accommodate at most 2 courses

### Complete Solution

```
STEP 1: VARIABLES
─────────────────
X = {C1, C2, C3, C4}

STEP 2: DOMAINS
───────────────
D(C1) = {A, B, C}
D(C2) = {A, B, C}
D(C3) = {A, B, C}
D(C4) = {A, B, C}

STEP 3: CONSTRAINTS
───────────────────

C1: C1 ≠ C2                    (same time, need different rooms)
C2: (No constraint between C2 and C3 for rooms - different times implied)
C3: C3 ≠ C4                    (overlapping students)
C4: C1 = C                     (requires lab - Unary)
C5: AtMost(2, [C1,C2,C3,C4], B) (Room B capacity)

STEP 4: APPLY CONSTRAINTS
─────────────────────────

From C4: D(C1) = {C}
From C1 (C1 ≠ C2) and C1 = C: D(C2) = {A, B}

STEP 5: SOLVE
─────────────

C1 = C (fixed)

Try C2 = A:
   C3 can be {A, B, C}
   C4 must be ≠ C3
   
   Try C3 = A:
      C4 ≠ A, so C4 ∈ {B, C}
      Try C4 = B
      
      Check AtMost(2, B): Only C4=B uses Room B. Count=1 ≤ 2 ✓
      
      SOLUTION FOUND!

SOLUTION:
┌─────────────────────────────────────────┐
│  C1 = Room C  (Lab requirement ✓)       │
│  C2 = Room A                            │
│  C3 = Room A                            │
│  C4 = Room B                            │
└─────────────────────────────────────────┘

Verification:
C1: C1(C) ≠ C2(A) ✓
C3: C3(A) ≠ C4(B) ✓
C4: C1 = C ✓
C5: Count of B assignments = 1 ≤ 2 ✓

Room allocation:
- Room A: C2, C3
- Room B: C4
- Room C: C1
```

---

## Exam Question 5: Library Book Eligibility (Logic + CSP)

### Problem Statement

Model the following as a CSP:

Library policy allows book issuance only to registered members with no overdue books. 

Members: Alice, Bob, Carol
Variables to determine: Can each member borrow a book?

Given facts:
- Alice is registered and has no overdue books
- Bob is registered but has overdue books
- Carol is not registered

### Solution

```
STEP 1: VARIABLES
─────────────────
X = {Alice_Borrow, Bob_Borrow, Carol_Borrow}

Each represents whether that member can borrow (Yes/No)

STEP 2: DOMAINS
───────────────
D(Alice_Borrow) = {Yes, No}
D(Bob_Borrow) = {Yes, No}
D(Carol_Borrow) = {Yes, No}

STEP 3: DEFINE HELPER PREDICATES
────────────────────────────────
Registered(Alice) = True
Registered(Bob) = True
Registered(Carol) = False

NoOverdue(Alice) = True
NoOverdue(Bob) = False
NoOverdue(Carol) = True (or undefined, doesn't matter)

STEP 4: CONSTRAINTS
───────────────────

Policy: Borrow = Yes ↔ (Registered AND NoOverdue)

For Alice:
   Registered(Alice) ∧ NoOverdue(Alice) = True ∧ True = True
   Constraint: Alice_Borrow = Yes

For Bob:
   Registered(Bob) ∧ NoOverdue(Bob) = True ∧ False = False
   Constraint: Bob_Borrow = No

For Carol:
   Registered(Carol) = False
   Constraint: Carol_Borrow = No

STEP 5: SOLUTION
────────────────

These are all unary constraints that fix the values:

┌─────────────────────────────────────────┐
│  Alice_Borrow = Yes                     │
│  Bob_Borrow = No                        │
│  Carol_Borrow = No                      │
└─────────────────────────────────────────┘

ANALYSIS:
- Alice: Registered ✓, No overdue ✓ → CAN borrow
- Bob: Registered ✓, Has overdue ✗ → CANNOT borrow
- Carol: Not registered ✗ → CANNOT borrow
```

---

## Mock Exam Pattern (Q3 Style): NLP Scenario → CSP (Variables, Domains, Constraints)

### What to Write in the Answer

1) **Variables** (what needs assigning)
2) **Domains** (allowed values for each variable)
3) **Constraints** (unary, binary, global; written clearly)
4) If asked, show **one consistent assignment** (a solution) and justify it

### Fast Template

```
STEP 1: VARIABLES
X = { ... }

STEP 2: DOMAINS
D(X1) = { ... }
D(X2) = { ... }

STEP 3: CONSTRAINTS
C1: ...
C2: ...

STEP 4: (OPTIONAL) SOLUTION
Assignment: { X1 = ..., X2 = ... }
```

# Practice Problems

## Problem 1: Simple Scheduling

```
Schedule 3 tasks (A, B, C) into 2 time slots (1, 2).

Constraints:
- A and B cannot be in the same slot
- B and C cannot be in the same slot

Questions:
a) Formulate as CSP
b) Find all solutions
c) How many solutions exist?
```

### Solution

```
a) CSP Formulation:
   Variables: X = {A, B, C}
   Domains: D(A) = D(B) = D(C) = {1, 2}
   Constraints: 
      C1: A ≠ B
      C2: B ≠ C

b) Finding Solutions:

   Try B = 1:
      A ≠ 1, so A = 2
      C ≠ 1, so C = 2
      Solution 1: {A=2, B=1, C=2}
   
   Try B = 2:
      A ≠ 2, so A = 1
      C ≠ 2, so C = 1
      Solution 2: {A=1, B=2, C=1}

c) Number of solutions: 2
```

## Problem 2: Graph Coloring

```
Color this graph with 2 colors (R, B):

    A --- B
    |     |
    C --- D

Constraints: Adjacent nodes must have different colors.

Questions:
a) Formulate as CSP
b) Does a solution exist?
c) What is the minimum number of colors needed?
```

### Solution

```
a) CSP Formulation:
   Variables: X = {A, B, C, D}
   Domains: D = {R, B} for all
   Constraints:
      A ≠ B (edge A-B)
      A ≠ C (edge A-C)
      B ≠ D (edge B-D)
      C ≠ D (edge C-D)

b) Finding Solution:
   Try A = R:
      B ≠ R → B = B
      C ≠ R → C = B
      D ≠ B (from B) and D ≠ B (from C)
      D must be ≠ B, so D = R
      
   Check: A(R)≠B(B)✓, A(R)≠C(B)✓, B(B)≠D(R)✓, C(B)≠D(R)✓
   
   Solution exists: {A=R, B=B, C=B, D=R}

c) Minimum colors: 2 (as demonstrated above)
```

## Problem 3: N-Queens (4-Queens)

```
Place 4 queens on a 4×4 chessboard so no two attack each other.

Formulate as CSP and find a solution.
```

### Solution

```
CSP Formulation:
   Variables: Q1, Q2, Q3, Q4 (one per column)
   Domain: {1, 2, 3, 4} (row numbers)
   
   Constraints:
   - All different rows: AllDiff(Q1, Q2, Q3, Q4)
   - No diagonal attacks:
     |Qi - Qj| ≠ |i - j| for all i ≠ j

Solution by backtracking:
   Q1 = 2
   Q2 = 4 (not same row, not diagonal to Q1)
   Q3 = 1 (not same row as Q1,Q2; not diagonal)
   Q4 = 3 (not same row as others; not diagonal)

Solution: {Q1=2, Q2=4, Q3=1, Q4=3}

Visual:
   1 2 3 4
1  . . Q .
2  Q . . .
3  . . . Q
4  . Q . .
```

---

# Quick Reference

## CSP Definition Template

```
┌─────────────────────────────────────────────────────────────┐
│                    CSP = (X, D, C)                           │
├─────────────────────────────────────────────────────────────┤
│ X = {X₁, X₂, ..., Xₙ}           Variables                   │
│ D = {D₁, D₂, ..., Dₙ}           Domains                     │
│ C = {C₁, C₂, ..., Cₘ}           Constraints                 │
└─────────────────────────────────────────────────────────────┘
```

## Constraint Types Summary

```
┌─────────────────────────────────────────────────────────────┐
│                  CONSTRAINT TYPES                            │
├─────────────────────────────────────────────────────────────┤
│ UNARY:    C(Xᵢ)           Example: X ≠ 5                    │
│ BINARY:   C(Xᵢ, Xⱼ)       Example: X ≠ Y                    │
│ GLOBAL:   C(X₁,...,Xₖ)    Example: AllDiff(X, Y, Z)         │
└─────────────────────────────────────────────────────────────┘
```

## Common Constraint Patterns

```
┌─────────────────────────────────────────────────────────────┐
│              COMMON CONSTRAINT PATTERNS                      │
├─────────────────────────────────────────────────────────────┤
│ Different values:     X ≠ Y                                  │
│ Same value:           X = Y                                  │
│ Fixed value:          X = constant                           │
│ Excluded value:       X ≠ constant                           │
│ Ordering:             X < Y, X > Y                           │
│ All different:        AllDiff(X₁, X₂, ..., Xₙ)              │
│ At most k:            AtMost(k, [vars], value)               │
│ Exactly k:            Exactly(k, [vars], value)              │
│ Sum constraint:       X + Y + Z = k                          │
└─────────────────────────────────────────────────────────────┘
```

## Solution Verification Checklist

```
┌─────────────────────────────────────────────────────────────┐
│              SOLUTION VERIFICATION                           │
├─────────────────────────────────────────────────────────────┤
│ □ All variables have exactly one value assigned              │
│ □ All values are from the respective domains                 │
│ □ All unary constraints satisfied                            │
│ □ All binary constraints satisfied                           │
│ □ All global constraints satisfied                           │
│ □ No constraint violations                                   │
└─────────────────────────────────────────────────────────────┘
```

## Exam Tips for CSP Questions

```
┌─────────────────────────────────────────────────────────────┐
│                    EXAM TIPS                                 │
├─────────────────────────────────────────────────────────────┤
│ 1. Read problem carefully - identify ALL constraints         │
│ 2. Look for keywords: "cannot", "must", "different", "same" │
│ 3. Apply unary constraints first to reduce domains           │
│ 4. Draw constraint graph for binary constraints              │
│ 5. Use systematic backtracking to find solutions             │
│ 6. Verify solution against ALL constraints                   │
│ 7. State clearly if no solution exists (and why)             │
└─────────────────────────────────────────────────────────────┘
```