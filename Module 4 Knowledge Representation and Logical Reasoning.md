# Module 4: Knowledge Representation and Logical Reasoning

## Table of Contents
1. [Introduction to Knowledge Representation](#introduction-to-knowledge-representation)
2. [Knowledge Base Construction](#knowledge-base-construction)
3. [Graph-Based Knowledge Representation](#graph-based-knowledge-representation)
4. [Propositional Logic](#propositional-logic)
5. [First-Order Logic (Predicate Logic)](#first-order-logic-predicate-logic)
6. [Inference Methods](#inference-methods)
7. [Resolution and Unification](#resolution-and-unification)
8. [Solved Exam Questions](#solved-exam-questions)
9. [Practice Problems](#practice-problems)

---

# Introduction to Knowledge Representation

## What is Knowledge Representation?

**Knowledge Representation (KR)** is a field of AI that focuses on representing information about the world in a form that a computer system can use to solve complex tasks.

```
┌─────────────────────────────────────────────────────────────┐
│              KNOWLEDGE REPRESENTATION                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Real World  →  Knowledge  →  Reasoning  →  Conclusions     │
│   Facts         Encoding       Engine        & Actions       │
│                                                              │
│  "Socrates     "Human(S)"     Apply         "Mortal(S)"     │
│   is human"                   rules                          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Why is KR Important?

| Purpose | Description |
|---------|-------------|
| Storage | Store facts and relationships systematically |
| Inference | Derive new knowledge from existing knowledge |
| Reasoning | Make logical conclusions |
| Communication | Share knowledge between systems |
| Problem Solving | Use knowledge to solve complex problems |

## Types of Knowledge

```
┌─────────────────────────────────────────────────────────────┐
│                  TYPES OF KNOWLEDGE                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. DECLARATIVE (Factual)                                   │
│     "What is true"                                           │
│     Example: "Paris is the capital of France"                │
│                                                              │
│  2. PROCEDURAL (How-to)                                      │
│     "How to do something"                                    │
│     Example: "To make tea, boil water, add tea leaves..."   │
│                                                              │
│  3. META-KNOWLEDGE                                           │
│     "Knowledge about knowledge"                              │
│     Example: "Rule A is more reliable than Rule B"           │
│                                                              │
│  4. HEURISTIC                                                │
│     "Rules of thumb"                                         │
│     Example: "If traffic is heavy, take the highway"         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Knowledge Representation Schemes

| Scheme | Description | Example |
|--------|-------------|---------|
| Logic | Formal symbolic representation | ∀x Human(x) → Mortal(x) |
| Semantic Networks | Graph-based relationships | Node-edge-node diagrams |
| Frames | Structured slot-filler templates | Object with attributes |
| Production Rules | IF-THEN rules | IF fever THEN take medicine |
| Ontologies | Formal vocabulary + relationships | Class hierarchies |

---

# Knowledge Base Construction

A **Knowledge Base (KB)** stores structured knowledge so an inference engine can answer questions and derive new facts.

## Policy Representation

A **policy** represents a rule/constraint about what is allowed, required, or forbidden.

Examples (rule-style):

```
Policy 1: IF user_is_admin THEN allow_access
Policy 2: IF claim_amount > 50000 THEN require_manual_review
Policy 3: IF NOT has_valid_id THEN deny_account_creation
```

## Fact Representation

A **fact** is a statement the KB treats as true (given the current information).

Examples:

```
Fact: Student(Ali)
Fact: Enrolled(Ali, AI101)
Fact: HasFever(Patient1)
```

## Claim Representation

A **claim** is an asserted statement that may need verification/evidence.

Examples:

```
Claim: EligibleForRefund(Order42)
Claim: AccidentReported(PolicyHolder7)
Claim: PlagiarismDetected(Submission9)
```

In many real systems:
- Facts are confirmed/observed.
- Claims are submitted/assumed until proved/checked.
- Policies are rules used to judge or derive outcomes.

---

# Graph-Based Knowledge Representation

Graph-based KR represents knowledge as **nodes (entities/concepts)** and **edges (relationships)**.

## Semantic Networks

A **semantic network** is a labeled graph where links represent relationships such as *is-a* and *part-of*.

Example:

```
Canary  --is-a-->  Bird  --is-a-->  Animal
Bird    --can-->   Fly
Canary  --color--> Yellow
```

## Knowledge Graphs

A **knowledge graph** is a richer semantic network where facts are often stored as triples:

$$ (subject,\ predicate,\ object) $$

Example triples:

```
(Ali, enrolledIn, AI101)
(AI101, taughtBy, DrSara)
(DrSara, worksAt, University)
```

## Conceptual Dependency Graphs

**Conceptual Dependency (CD)** graphs represent the meaning of sentences using conceptual primitives (actions) and relationships.

Example idea:

```
"Ali gave Sara a book"

AGENT: Ali
ACT:   GIVE
OBJ:   Book
RECIP: Sara
```

CD helps normalize different sentence forms into a similar meaning representation (useful for reasoning in NLP contexts).

---

# Propositional Logic

## Definition

**Propositional Logic** (also called Boolean Logic or Sentential Logic) deals with propositions that are either TRUE or FALSE.

## Syntax Elements

### Atomic Propositions

```
Atomic propositions are simple statements that are either true or false.

Examples:
P = "It is raining"
Q = "The ground is wet"
R = "I will take an umbrella"

Represented by: P, Q, R, S, ... or p, q, r, s, ...
```

### Logical Connectives

| Symbol | Name | Meaning | Example |
|--------|------|---------|---------|
| ¬ | Negation (NOT) | "not P" | ¬P = "It is not raining" |
| ∧ | Conjunction (AND) | "P and Q" | P ∧ Q = "Raining and wet" |
| ∨ | Disjunction (OR) | "P or Q" | P ∨ Q = "Raining or wet" |
| → | Implication (IF-THEN) | "if P then Q" | P → Q = "If raining, then wet" |
| ↔ | Biconditional (IFF) | "P if and only if Q" | P ↔ Q = "Raining iff wet" |

### Operator Precedence

```
Highest to Lowest:
1. ¬ (NOT)           - evaluated first
2. ∧ (AND)
3. ∨ (OR)
4. → (IMPLIES)
5. ↔ (IFF)           - evaluated last

Example: ¬P ∧ Q → R
Parsed as: ((¬P) ∧ Q) → R
```

## Truth Tables

### NOT (¬)

| P | ¬P |
|---|-----|
| T | F |
| F | T |

### AND (∧)

| P | Q | P ∧ Q |
|---|---|-------|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | F |

### OR (∨)

| P | Q | P ∨ Q |
|---|---|-------|
| T | T | T |
| T | F | T |
| F | T | T |
| F | F | F |

### IMPLICATION (→)

| P | Q | P → Q |
|---|---|-------|
| T | T | T |
| T | F | F |
| F | T | T |
| F | F | T |

```
IMPORTANT: P → Q is FALSE only when P is TRUE and Q is FALSE!

Intuition: "If it rains, the ground is wet"
- Raining and wet: Promise kept ✓
- Raining and dry: Promise broken ✗
- Not raining and wet: No promise made, OK ✓
- Not raining and dry: No promise made, OK ✓
```

### BICONDITIONAL (↔)

| P | Q | P ↔ Q |
|---|---|-------|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | T |

```
P ↔ Q is TRUE when P and Q have the SAME truth value.
```

## Logical Equivalences

### Important Equivalences

```
┌─────────────────────────────────────────────────────────────┐
│              IMPORTANT LOGICAL EQUIVALENCES                  │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│ DOUBLE NEGATION:                                             │
│   ¬(¬P) ≡ P                                                  │
│                                                              │
│ DE MORGAN'S LAWS:                                            │
│   ¬(P ∧ Q) ≡ ¬P ∨ ¬Q                                        │
│   ¬(P ∨ Q) ≡ ¬P ∧ ¬Q                                        │
│                                                              │
│ IMPLICATION:                                                 │
│   P → Q ≡ ¬P ∨ Q                                            │
│   P → Q ≡ ¬Q → ¬P  (Contrapositive)                         │
│                                                              │
│ BICONDITIONAL:                                               │
│   P ↔ Q ≡ (P → Q) ∧ (Q → P)                                 │
│                                                              │
│ DISTRIBUTIVE:                                                │
│   P ∧ (Q ∨ R) ≡ (P ∧ Q) ∨ (P ∧ R)                          │
│   P ∨ (Q ∧ R) ≡ (P ∨ Q) ∧ (P ∨ R)                          │
│                                                              │
│ COMMUTATIVE:                                                 │
│   P ∧ Q ≡ Q ∧ P                                             │
│   P ∨ Q ≡ Q ∨ P                                             │
│                                                              │
│ ASSOCIATIVE:                                                 │
│   (P ∧ Q) ∧ R ≡ P ∧ (Q ∧ R)                                 │
│   (P ∨ Q) ∨ R ≡ P ∨ (Q ∨ R)                                 │
│                                                              │
│ IDENTITY:                                                    │
│   P ∧ TRUE ≡ P                                               │
│   P ∨ FALSE ≡ P                                              │
│                                                              │
│ DOMINATION:                                                  │
│   P ∨ TRUE ≡ TRUE                                            │
│   P ∧ FALSE ≡ FALSE                                          │
│                                                              │
│ IDEMPOTENT:                                                  │
│   P ∧ P ≡ P                                                  │
│   P ∨ P ≡ P                                                  │
│                                                              │
│ ABSORPTION:                                                  │
│   P ∨ (P ∧ Q) ≡ P                                           │
│   P ∧ (P ∨ Q) ≡ P                                           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Terminology

### Tautology

```
A formula that is TRUE for ALL possible truth value assignments.

Example: P ∨ ¬P (Law of Excluded Middle)

| P | ¬P | P ∨ ¬P |
|---|-----|--------|
| T | F   | T      |
| F | T   | T      |

Always TRUE → TAUTOLOGY
```

### Contradiction

```
A formula that is FALSE for ALL possible truth value assignments.

Example: P ∧ ¬P

| P | ¬P | P ∧ ¬P |
|---|-----|--------|
| T | F   | F      |
| F | T   | F      |

Always FALSE → CONTRADICTION
```

### Contingency

```
A formula that is TRUE for some assignments and FALSE for others.

Example: P → Q

| P | Q | P → Q |
|---|---|-------|
| T | T | T     |
| T | F | F     |  ← Sometimes TRUE, sometimes FALSE
| F | T | T     |
| F | F | T     |

→ CONTINGENCY
```

### Satisfiability

```
A formula is SATISFIABLE if there exists at least one
truth value assignment that makes it TRUE.

- Tautology: Always satisfiable
- Contingency: Satisfiable
- Contradiction: NOT satisfiable (unsatisfiable)
```

---

## Propositional Logic Example: Truth Table Construction

### Problem

Construct the truth table for: (P → Q) ∧ (Q → R) → (P → R)

### Solution

```
Step 1: Identify all propositional variables
        Variables: P, Q, R (3 variables = 2³ = 8 rows)

Step 2: Create columns for each subformula
        - P, Q, R (atomic)
        - P → Q
        - Q → R
        - (P → Q) ∧ (Q → R)
        - P → R
        - (P → Q) ∧ (Q → R) → (P → R)

Step 3: Fill in the truth table

| P | Q | R | P→Q | Q→R | (P→Q)∧(Q→R) | P→R | Final |
|---|---|---|-----|-----|-------------|-----|-------|
| T | T | T |  T  |  T  |      T      |  T  |   T   |
| T | T | F |  T  |  F  |      F      |  F  |   T   |
| T | F | T |  F  |  T  |      F      |  T  |   T   |
| T | F | F |  F  |  T  |      F      |  F  |   T   |
| F | T | T |  T  |  T  |      T      |  T  |   T   |
| F | T | F |  T  |  F  |      F      |  T  |   T   |
| F | F | T |  T  |  T  |      T      |  T  |   T   |
| F | F | F |  T  |  T  |      T      |  T  |   T   |

All entries in Final column are T → TAUTOLOGY!

This formula represents the transitivity of implication:
"If P implies Q, and Q implies R, then P implies R"
```

---

# First-Order Logic (Predicate Logic)

## Definition

**First-Order Logic (FOL)**, also called **Predicate Logic**, extends propositional logic with:
- Variables
- Predicates (properties and relations)
- Quantifiers (∀ and ∃)
- Functions

## Syntax Elements

### Constants

```
Constants: Specific objects in the domain

Examples:
- john, mary, bob (people)
- 1, 2, 3 (numbers)
- monday, tuesday (days)

Notation: Lowercase letters or names
```

### Variables

```
Variables: Placeholders for objects

Examples:
- x, y, z (any object)

Notation: Lowercase letters (usually x, y, z)
```

### Predicates

```
Predicates: Properties or relations between objects

Unary Predicate (property):
  Human(x)     - "x is human"
  Tall(john)   - "John is tall"

Binary Predicate (relation):
  Loves(x, y)  - "x loves y"
  Father(x, y) - "x is father of y"

N-ary Predicate:
  Between(x, y, z) - "x is between y and z"

Notation: Capitalized, with arguments in parentheses
```

### Functions

```
Functions: Map objects to objects

Examples:
  father(x)    - "the father of x" (returns a person)
  mother(x)    - "the mother of x"
  age(x)       - "the age of x" (returns a number)
  plus(x, y)   - "x + y" (returns a number)

Notation: Lowercase, with arguments in parentheses

Difference from Predicates:
- Predicate: Returns TRUE/FALSE
- Function: Returns an object/value
```

### Quantifiers

```
┌─────────────────────────────────────────────────────────────┐
│                    QUANTIFIERS                               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│ UNIVERSAL QUANTIFIER (∀):                                    │
│   ∀x P(x) = "For all x, P(x) is true"                       │
│                                                              │
│   Example: ∀x Human(x) → Mortal(x)                          │
│   "All humans are mortal"                                    │
│                                                              │
│ EXISTENTIAL QUANTIFIER (∃):                                  │
│   ∃x P(x) = "There exists an x such that P(x) is true"      │
│                                                              │
│   Example: ∃x Human(x) ∧ Tall(x)                            │
│   "There exists a human who is tall"                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Terms and Formulas

```
TERMS (refer to objects):
- Constants: john, 5, monday
- Variables: x, y, z
- Functions applied to terms: father(john), age(x)

ATOMIC FORMULAS:
- Predicate applied to terms: Human(x), Loves(john, mary)

COMPLEX FORMULAS:
- Atomic formulas combined with connectives and quantifiers
- Example: ∀x (Human(x) → ∃y Loves(x, y))
           "Every human loves someone"
```

## Quantifier Equivalences

```
┌─────────────────────────────────────────────────────────────┐
│              QUANTIFIER EQUIVALENCES                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│ NEGATION:                                                    │
│   ¬(∀x P(x)) ≡ ∃x ¬P(x)                                     │
│   "Not all x have P" ≡ "Some x doesn't have P"               │
│                                                              │
│   ¬(∃x P(x)) ≡ ∀x ¬P(x)                                     │
│   "No x has P" ≡ "All x don't have P"                        │
│                                                              │
│ DISTRIBUTION:                                                │
│   ∀x (P(x) ∧ Q(x)) ≡ ∀x P(x) ∧ ∀x Q(x)                     │
│   ∃x (P(x) ∨ Q(x)) ≡ ∃x P(x) ∨ ∃x Q(x)                     │
│                                                              │
│ BUT NOTE (not equivalent):                                   │
│   ∀x (P(x) ∨ Q(x)) ≢ ∀x P(x) ∨ ∀x Q(x)                     │
│   ∃x (P(x) ∧ Q(x)) ≢ ∃x P(x) ∧ ∃x Q(x)                     │
│                                                              │
│ QUANTIFIER ORDER:                                            │
│   ∀x ∀y P(x,y) ≡ ∀y ∀x P(x,y)  (same quantifier)           │
│   ∃x ∃y P(x,y) ≡ ∃y ∃x P(x,y)  (same quantifier)           │
│                                                              │
│   BUT:                                                       │
│   ∀x ∃y P(x,y) ≢ ∃y ∀x P(x,y)  (different quantifiers)     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Translating English to FOL

### Common Patterns

| English | FOL Pattern |
|---------|-------------|
| All A are B | ∀x (A(x) → B(x)) |
| Some A are B | ∃x (A(x) ∧ B(x)) |
| No A are B | ∀x (A(x) → ¬B(x)) or ¬∃x (A(x) ∧ B(x)) |
| Some A are not B | ∃x (A(x) ∧ ¬B(x)) |
| Not all A are B | ¬∀x (A(x) → B(x)) or ∃x (A(x) ∧ ¬B(x)) |
| Only A are B | ∀x (B(x) → A(x)) |
| x is the only A | A(x) ∧ ∀y (A(y) → y=x) |

### Translation Examples

```
Example 1: "All students are hardworking"
FOL: ∀x (Student(x) → Hardworking(x))

Example 2: "Some students like mathematics"
FOL: ∃x (Student(x) ∧ Likes(x, mathematics))

Example 3: "No student likes exams"
FOL: ∀x (Student(x) → ¬Likes(x, exams))
  or: ¬∃x (Student(x) ∧ Likes(x, exams))

Example 4: "Every student has a teacher"
FOL: ∀x (Student(x) → ∃y (Teacher(y) ∧ Teaches(y, x)))

Example 5: "John loves everyone who loves him"
FOL: ∀x (Loves(x, john) → Loves(john, x))

Example 6: "There is someone who loves everyone"
FOL: ∃x ∀y Loves(x, y)

Example 7: "Everyone loves someone"
FOL: ∀x ∃y Loves(x, y)

Note: Examples 6 and 7 are DIFFERENT!
- Ex 6: One person loves all
- Ex 7: Each person loves at least one (possibly different) person
```

---

# Inference Methods

## What is Inference?

```
INFERENCE: Deriving new statements from existing statements
           using logical rules.

Knowledge Base (KB): Set of known facts and rules
Query (α): Statement we want to prove

KB ⊨ α : "KB entails α" (α is true whenever KB is true)
KB ⊢ α : "α is derivable from KB" (can prove α from KB)
```

## Inference Rules for Propositional Logic

### Modus Ponens

```
┌─────────────────────────────────────────────────────────────┐
│                    MODUS PONENS                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise 1:  P → Q        (If P then Q)                     │
│  Premise 2:  P            (P is true)                       │
│  ─────────────────                                           │
│  Conclusion: Q            (Therefore Q)                      │
│                                                              │
│  Example:                                                    │
│    P → Q : "If it rains, the ground is wet"                 │
│    P     : "It is raining"                                   │
│    ─────────────────────                                     │
│    Q     : "The ground is wet"                               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Modus Tollens

```
┌─────────────────────────────────────────────────────────────┐
│                    MODUS TOLLENS                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise 1:  P → Q        (If P then Q)                     │
│  Premise 2:  ¬Q           (Q is false)                      │
│  ─────────────────                                           │
│  Conclusion: ¬P           (Therefore P is false)             │
│                                                              │
│  Example:                                                    │
│    P → Q : "If it rains, the ground is wet"                 │
│    ¬Q    : "The ground is not wet"                          │
│    ─────────────────────                                     │
│    ¬P    : "It is not raining"                              │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Hypothetical Syllogism

```
┌─────────────────────────────────────────────────────────────┐
│              HYPOTHETICAL SYLLOGISM                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise 1:  P → Q                                          │
│  Premise 2:  Q → R                                          │
│  ─────────────────                                           │
│  Conclusion: P → R                                           │
│                                                              │
│  Example:                                                    │
│    P → Q : "If it rains, the ground is wet"                 │
│    Q → R : "If ground is wet, it's slippery"                │
│    ─────────────────────                                     │
│    P → R : "If it rains, it's slippery"                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Disjunctive Syllogism

```
┌─────────────────────────────────────────────────────────────┐
│              DISJUNCTIVE SYLLOGISM                           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise 1:  P ∨ Q        (P or Q)                          │
│  Premise 2:  ¬P           (Not P)                           │
│  ─────────────────                                           │
│  Conclusion: Q            (Therefore Q)                      │
│                                                              │
│  Example:                                                    │
│    P ∨ Q : "It's raining or sunny"                          │
│    ¬P    : "It's not raining"                               │
│    ─────────────────────                                     │
│    Q     : "It's sunny"                                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### And-Elimination

```
┌─────────────────────────────────────────────────────────────┐
│                  AND-ELIMINATION                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise:    P ∧ Q                                          │
│  ─────────────────                                           │
│  Conclusion: P              (can also conclude Q)            │
│                                                              │
│  Example:                                                    │
│    P ∧ Q : "It's cold and raining"                          │
│    ─────────────────────                                     │
│    P     : "It's cold"                                       │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### And-Introduction

```
┌─────────────────────────────────────────────────────────────┐
│                  AND-INTRODUCTION                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise 1:  P                                              │
│  Premise 2:  Q                                              │
│  ─────────────────                                           │
│  Conclusion: P ∧ Q                                          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Or-Introduction

```
┌─────────────────────────────────────────────────────────────┐
│                  OR-INTRODUCTION                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise:    P                                              │
│  ─────────────────                                           │
│  Conclusion: P ∨ Q          (for any Q)                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Inference Rules Summary Table

| Rule | Premises | Conclusion |
|------|----------|------------|
| Modus Ponens | P → Q, P | Q |
| Modus Tollens | P → Q, ¬Q | ¬P |
| Hypothetical Syllogism | P → Q, Q → R | P → R |
| Disjunctive Syllogism | P ∨ Q, ¬P | Q |
| And-Elimination | P ∧ Q | P (or Q) |
| And-Introduction | P, Q | P ∧ Q |
| Or-Introduction | P | P ∨ Q |
| Resolution | P ∨ Q, ¬P ∨ R | Q ∨ R |

---

## Inference Rules for First-Order Logic

### Universal Instantiation (UI)

```
┌─────────────────────────────────────────────────────────────┐
│              UNIVERSAL INSTANTIATION                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise:    ∀x P(x)                                        │
│  ─────────────────                                           │
│  Conclusion: P(a)           (for any constant a)            │
│                                                              │
│  Example:                                                    │
│    ∀x Human(x) → Mortal(x)                                  │
│    ─────────────────────────                                 │
│    Human(socrates) → Mortal(socrates)                       │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Existential Instantiation (EI)

```
┌─────────────────────────────────────────────────────────────┐
│              EXISTENTIAL INSTANTIATION                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise:    ∃x P(x)                                        │
│  ─────────────────                                           │
│  Conclusion: P(c)           (for a NEW constant c)          │
│                                                              │
│  Example:                                                    │
│    ∃x King(x) ∧ Rich(x)                                     │
│    ─────────────────────                                     │
│    King(c) ∧ Rich(c)        (c is a new Skolem constant)    │
│                                                              │
│  IMPORTANT: c must be a NEW constant not appearing elsewhere│
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Universal Generalization (UG)

```
┌─────────────────────────────────────────────────────────────┐
│              UNIVERSAL GENERALIZATION                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise:    P(x)           (x is arbitrary/not specific)   │
│  ─────────────────                                           │
│  Conclusion: ∀x P(x)                                        │
│                                                              │
│  RESTRICTION: x must not have been introduced by EI         │
│               and must be truly arbitrary                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Existential Generalization (EG)

```
┌─────────────────────────────────────────────────────────────┐
│              EXISTENTIAL GENERALIZATION                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Premise:    P(a)           (for some constant a)           │
│  ─────────────────                                           │
│  Conclusion: ∃x P(x)                                        │
│                                                              │
│  Example:                                                    │
│    Loves(john, mary)                                        │
│    ─────────────────                                         │
│    ∃x Loves(x, mary)        (Someone loves Mary)            │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

# Resolution and Unification

## Resolution

### Overview

```
RESOLUTION is a single inference rule that is:
- Complete for propositional logic
- Complete for FOL (with certain restrictions)
- Basis for many automated theorem provers

GOAL: Prove KB ⊨ α by showing KB ∧ ¬α is unsatisfiable
      (Proof by contradiction)
```

### Clausal Form (CNF)

```
CONJUNCTIVE NORMAL FORM (CNF):
- Conjunction (AND) of clauses
- Each clause is a disjunction (OR) of literals
- A literal is an atom or negated atom

Example:
(P ∨ Q ∨ ¬R) ∧ (¬P ∨ R) ∧ (Q ∨ R)

Clause 1: P ∨ Q ∨ ¬R
Clause 2: ¬P ∨ R
Clause 3: Q ∨ R
```

### Converting to CNF

```
STEPS TO CONVERT TO CNF:
─────────────────────────

1. Eliminate biconditionals (↔)
   P ↔ Q  →  (P → Q) ∧ (Q → P)

2. Eliminate implications (→)
   P → Q  →  ¬P ∨ Q

3. Move negation inward (De Morgan's)
   ¬(P ∧ Q)  →  ¬P ∨ ¬Q
   ¬(P ∨ Q)  →  ¬P ∧ ¬Q
   ¬¬P  →  P

4. Distribute OR over AND
   P ∨ (Q ∧ R)  →  (P ∨ Q) ∧ (P ∨ R)

5. Flatten nested conjunctions and disjunctions
```

### CNF Conversion Example

```
Convert: (P → Q) → R

Step 1: No biconditionals

Step 2: Eliminate implications (innermost first)
   P → Q  →  ¬P ∨ Q
   
   So: (¬P ∨ Q) → R
   →  ¬(¬P ∨ Q) ∨ R

Step 3: Move negation inward
   ¬(¬P ∨ Q)  →  ¬¬P ∧ ¬Q  →  P ∧ ¬Q
   
   So: (P ∧ ¬Q) ∨ R

Step 4: Distribute OR over AND
   (P ∧ ¬Q) ∨ R  →  (P ∨ R) ∧ (¬Q ∨ R)

CNF: (P ∨ R) ∧ (¬Q ∨ R)

Clauses: {P, R} and {¬Q, R}
```

### Resolution Rule

```
┌─────────────────────────────────────────────────────────────┐
│                  RESOLUTION RULE                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Clause 1:  P ∨ Q₁ ∨ Q₂ ∨ ... ∨ Qₙ                         │
│  Clause 2:  ¬P ∨ R₁ ∨ R₂ ∨ ... ∨ Rₘ                        │
│  ──────────────────────────────────                          │
│  Resolvent: Q₁ ∨ Q₂ ∨ ... ∨ Qₙ ∨ R₁ ∨ R₂ ∨ ... ∨ Rₘ       │
│                                                              │
│  The complementary literals (P and ¬P) cancel out.          │
│                                                              │
│  Simple Example:                                             │
│    Clause 1:  P ∨ Q                                         │
│    Clause 2:  ¬P ∨ R                                        │
│    ──────────────────                                        │
│    Resolvent: Q ∨ R                                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Empty Clause

```
When resolution produces an EMPTY CLAUSE (denoted ⊥ or □):
- This represents a contradiction
- Proves that the original set of clauses is unsatisfiable

Example:
  Clause 1: P
  Clause 2: ¬P
  ─────────────
  Resolvent: □ (empty clause)

This means P ∧ ¬P is unsatisfiable (contradiction).
```

### Resolution Proof Example

```
PROVE: From (P ∨ Q), (¬P ∨ R), (¬Q ∨ R), derive R

Step 1: Assume ¬R (for proof by contradiction)

Clauses:
1. P ∨ Q       (given)
2. ¬P ∨ R      (given)
3. ¬Q ∨ R      (given)
4. ¬R          (negation of goal)

Step 2: Apply resolution

Resolve 2 and 4 on R:
  ¬P ∨ R
  ¬R
  ─────────
5. ¬P

Resolve 3 and 4 on R:
  ¬Q ∨ R
  ¬R
  ─────────
6. ¬Q

Resolve 1 and 5 on P:
  P ∨ Q
  ¬P
  ─────────
7. Q

Resolve 6 and 7 on Q:
  ¬Q
  Q
  ─────────
8. □ (empty clause - CONTRADICTION!)

Step 3: Conclusion
Since adding ¬R leads to contradiction, R must be true.
Therefore, the original clauses entail R. ∎

Resolution Tree:
                   C1: P∨Q    C2: ¬P∨R    C3: ¬Q∨R    C4: ¬R
                       │           │            │          │
                       │           └─────┬──────┘          │
                       │                 │                 │
                       │            C5: ¬P     C6: ¬Q     │
                       │                 │          │      │
                       └────────┬────────┘          └──┬───┘
                                │                      │
                            C7: Q                  C6: ¬Q
                                │                      │
                                └──────────┬───────────┘
                                           │
                                        C8: □
```

## Unification

### Definition

```
UNIFICATION: Finding a substitution that makes two expressions identical.

SUBSTITUTION (θ): A mapping from variables to terms.
  θ = {x/a, y/f(b)}
  Means: Replace x with a, replace y with f(b)

UNIFIER: A substitution θ such that applying θ to both expressions
         makes them identical.

MOST GENERAL UNIFIER (MGU): The unifier that makes the 
         minimum commitment (most general substitution).
```

### Unification Examples

```
Example 1: Unify P(x) and P(a)
  x is a variable, a is a constant
  Substitution: θ = {x/a}
  Result: P(a) and P(a) ✓
  MGU: {x/a}

Example 2: Unify P(x, y) and P(a, b)
  Substitution: θ = {x/a, y/b}
  Result: P(a, b) and P(a, b) ✓
  MGU: {x/a, y/b}

Example 3: Unify P(x, x) and P(a, b)
  x must equal both a and b
  But a ≠ b
  CANNOT UNIFY ✗

Example 4: Unify P(x, f(x)) and P(a, y)
  x must equal a: {x/a}
  y must equal f(x) = f(a): {y/f(a)}
  MGU: {x/a, y/f(a)}
  Result: P(a, f(a)) and P(a, f(a)) ✓

Example 5: Unify P(x, f(y)) and P(g(z), f(a))
  x must equal g(z): {x/g(z)}
  y must equal a: {y/a}
  MGU: {x/g(z), y/a}
  Result: P(g(z), f(a)) and P(g(z), f(a)) ✓

Example 6: Unify P(x) and P(f(x))
  x must equal f(x)
  This is a circular dependency (occurs check fails)
  CANNOT UNIFY ✗
```

### Unification Algorithm

```
UNIFY(E1, E2, θ):
  1. If θ = FAILURE, return FAILURE
  2. If E1 = E2 (already identical), return θ
  3. If E1 is a variable:
     - If E1 occurs in E2, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E1/E2})
  4. If E2 is a variable:
     - If E2 occurs in E1, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E2/E1})
  5. If E1 and E2 are both compound (same functor, same arity):
     - Unify arguments one by one
  6. Otherwise, return FAILURE
```

### Unification Practice Table

| Expression 1 | Expression 2 | MGU | Result |
|--------------|--------------|-----|--------|
| P(x) | P(a) | {x/a} | P(a) |
| P(a) | P(a) | {} (empty) | P(a) |
| P(x) | Q(a) | FAIL | Different predicates |
| P(x,y) | P(y,a) | {x/a, y/a} | P(a,a) |
| P(f(x)) | P(f(a)) | {x/a} | P(f(a)) |
| P(x,x) | P(a,b) | FAIL | x can't be both a and b |
| P(x,f(a)) | P(b,y) | {x/b, y/f(a)} | P(b,f(a)) |
| P(x) | P(f(x)) | FAIL | Occurs check |

---

# Solved Exam Questions

## Exam Question 1: Truth Table and Validity

### Problem

Construct a truth table for the formula: (P → Q) ∧ (Q → P) ↔ (P ↔ Q)

Determine if this is a tautology, contradiction, or contingency.

### Solution

```
Step 1: Identify variables: P, Q (2 variables = 4 rows)

Step 2: Build subformulas:
- P → Q
- Q → P
- (P → Q) ∧ (Q → P)
- P ↔ Q
- (P → Q) ∧ (Q → P) ↔ (P ↔ Q)

Step 3: Truth Table:

| P | Q | P→Q | Q→P | (P→Q)∧(Q→P) | P↔Q | Final |
|---|---|-----|-----|-------------|-----|-------|
| T | T |  T  |  T  |      T      |  T  |   T   |
| T | F |  F  |  T  |      F      |  F  |   T   |
| F | T |  T  |  F  |      F      |  F  |   T   |
| F | F |  T  |  T  |      T      |  T  |   T   |

Step 4: Analysis:
All values in Final column are TRUE.

CONCLUSION: This is a TAUTOLOGY.

This demonstrates that $(P \to Q) \land (Q \to P)$ is logically equivalent to $(P \leftrightarrow Q)$, so the biconditional between them is always true.

---

## Mock Exam Pattern (Q4 Style): Policy/Facts/Claims → CNF → Resolution

### Prompt (Typical)

Given a short scenario, do the following:
1) write **policies**, **facts**, and **claim** in logic form,
2) convert to **CNF / clause form**,
3) use **resolution** to prove the claim (or show contradiction).

### Worked Example: Hospital Access Dispute

**Scenario (short):**
- Policy: If someone is a doctor, they are authorized.
- Policy: If someone is authorized, they can access the medical records.
- Fact: Ali is a doctor.
- Claim: Ali can access the medical records.

**Step 1 — Symbols / Predicates**
- $Doctor(x)$: $x$ is a doctor
- $Authorized(x)$: $x$ is authorized
- $Access(x)$: $x$ can access medical records
- Constant: $a = Ali$

**Step 2 — Policies, Facts, Claim**
- Policy 1: $Doctor(x) \to Authorized(x)$
- Policy 2: $Authorized(x) \to Access(x)$
- Fact: $Doctor(a)$
- Claim: $Access(a)$

**Step 3 — Convert to CNF (Clauses)**
- From $Doctor(x) \to Authorized(x)$: $\neg Doctor(x) \lor Authorized(x)$
- From $Authorized(x) \to Access(x)$: $\neg Authorized(x) \lor Access(x)$
- Fact clause: $Doctor(a)$

To prove $Access(a)$ by contradiction, add $\neg Access(a)$.

**Clauses:**
1. $\neg Doctor(x) \lor Authorized(x)$
2. $\neg Authorized(x) \lor Access(x)$
3. $Doctor(a)$
4. $\neg Access(a)$

**Step 4 — Resolution**
- Resolve (2) with (4) on $Access$ (instantiate $x=a$):
   - $\neg Authorized(a)$
- Resolve (1) with $\neg Authorized(a)$ on $Authorized$ (instantiate $x=a$):
   - $\neg Doctor(a)$
- Resolve $\neg Doctor(a)$ with (3) $Doctor(a)$:
   - $\Box$ (empty clause)

**Conclusion:** Contradiction obtained, therefore $Access(a)$ is entailed.
(P → Q) ∧ (Q → P) is logically equivalent to P ↔ Q
"If P then Q, and if Q then P" means "P if and only if Q"
```

---

## Exam Question 2: FOL Translation

### Problem

Translate the following English sentences into First-Order Logic:

a) "All birds can fly"
b) "Some birds cannot fly"
c) "Penguins are birds that cannot fly"
d) "Every child loves some candy"
e) "There is a person who knows everyone"

### Solution

```
a) "All birds can fly"

   Analysis: For all x, if x is a bird, then x can fly
   
   FOL: ∀x (Bird(x) → CanFly(x))


b) "Some birds cannot fly"

   Analysis: There exists x such that x is a bird AND x cannot fly
   
   FOL: ∃x (Bird(x) ∧ ¬CanFly(x))


c) "Penguins are birds that cannot fly"

   Analysis: For all x, if x is a penguin, then x is a bird AND cannot fly
   
   FOL: ∀x (Penguin(x) → (Bird(x) ∧ ¬CanFly(x)))


d) "Every child loves some candy"

   Analysis: For all x, if x is a child, there exists y such that 
             y is candy AND x loves y
   
   FOL: ∀x (Child(x) → ∃y (Candy(y) ∧ Loves(x, y)))


e) "There is a person who knows everyone"

   Analysis: There exists x such that x is a person AND 
             for all y (if y is a person, x knows y)
   
   FOL: ∃x (Person(x) ∧ ∀y (Person(y) → Knows(x, y)))
   
   Alternative (if "everyone" means all people):
   ∃x ∀y (Person(y) → Knows(x, y))
```

---

## Exam Question 3: Inference Chain

### Problem

Given the following knowledge base, prove that "Tweety is mortal" using logical inference rules.

```
KB:
1. All birds are animals
2. All animals are mortal
3. Tweety is a bird
```

### Solution

```
Step 1: Convert to FOL

1. All birds are animals:     ∀x (Bird(x) → Animal(x))
2. All animals are mortal:    ∀x (Animal(x) → Mortal(x))
3. Tweety is a bird:          Bird(tweety)

Step 2: Apply Universal Instantiation

From (1), instantiate with x = tweety:
4. Bird(tweety) → Animal(tweety)    [UI on 1]

From (2), instantiate with x = tweety:
5. Animal(tweety) → Mortal(tweety)  [UI on 2]

Step 3: Apply Modus Ponens

From (3) and (4):
   Bird(tweety)                      [Given in 3]
   Bird(tweety) → Animal(tweety)     [From 4]
   ─────────────────────────────────
6. Animal(tweety)                    [Modus Ponens]

From (6) and (5):
   Animal(tweety)                    [From 6]
   Animal(tweety) → Mortal(tweety)   [From 5]
   ─────────────────────────────────
7. Mortal(tweety)                    [Modus Ponens]

CONCLUSION: Tweety is mortal ∎

Inference Chain:
┌────────────────────────────────────────────────────────────┐
│ Bird(tweety) → Animal(tweety) → Mortal(tweety)            │
│                                                            │
│ Using: Hypothetical Syllogism gives us:                    │
│ Bird(tweety) → Mortal(tweety)                             │
│                                                            │
│ With Bird(tweety), Modus Ponens gives: Mortal(tweety)     │
└────────────────────────────────────────────────────────────┘
```

---

## Exam Question 4: Resolution Proof

### Problem

Use resolution to prove that R follows from:
- P ∨ Q
- P → R
- Q → R

### Solution

```
Step 1: Convert to CNF

Given:
1. P ∨ Q           (already in CNF)
2. P → R           ≡ ¬P ∨ R
3. Q → R           ≡ ¬Q ∨ R

To prove R, negate it and add to clauses:
4. ¬R

Clauses:
C1: P ∨ Q
C2: ¬P ∨ R
C3: ¬Q ∨ R
C4: ¬R

Step 2: Apply Resolution

Resolve C2 and C4 (on R and ¬R):
   ¬P ∨ R
   ¬R
   ─────────
C5: ¬P

Resolve C3 and C4 (on R and ¬R):
   ¬Q ∨ R
   ¬R
   ─────────
C6: ¬Q

Resolve C1 and C5 (on P and ¬P):
   P ∨ Q
   ¬P
   ─────────
C7: Q

Resolve C6 and C7 (on Q and ¬Q):
   ¬Q
   Q
   ─────────
C8: □ (empty clause)

Step 3: Conclusion

We derived the empty clause □, which means:
{P ∨ Q, ¬P ∨ R, ¬Q ∨ R, ¬R} is UNSATISFIABLE

Therefore, {P ∨ Q, ¬P ∨ R, ¬Q ∨ R} ⊨ R

R is proven! ∎

Resolution Tree:
                   C1: P∨Q    C2: ¬P∨R    C3: ¬Q∨R    C4: ¬R
                       │           │            │          │
                       │           └─────┬──────┘          │
                       │                 │                 │
                       │            C5: ¬P     C6: ¬Q     │
                       │                 │          │      │
                       └────────┬────────┘          └──┬───┘
                                │                      │
                            C7: Q                  C6: ¬Q
                                │                      │
                                └──────────┬───────────┘
                                           │
                                        C8: □
```

## Unification

### Definition

```
UNIFICATION: Finding a substitution that makes two expressions identical.

SUBSTITUTION (θ): A mapping from variables to terms.
  θ = {x/a, y/f(b)}
  Means: Replace x with a, replace y with f(b)

UNIFIER: A substitution θ such that applying θ to both expressions
         makes them identical.

MOST GENERAL UNIFIER (MGU): The unifier that makes the 
         minimum commitment (most general substitution).
```

### Unification Examples

```
Example 1: Unify P(x) and P(a)
  x is a variable, a is a constant
  Substitution: θ = {x/a}
  Result: P(a) and P(a) ✓
  MGU: {x/a}

Example 2: Unify P(x, y) and P(a, b)
  Substitution: θ = {x/a, y/b}
  Result: P(a, b) and P(a, b) ✓
  MGU: {x/a, y/b}

Example 3: Unify P(x, x) and P(a, b)
  x must equal both a and b
  But a ≠ b
  CANNOT UNIFY ✗

Example 4: Unify P(x, f(x)) and P(a, y)
  x must equal a: {x/a}
  y must equal f(x) = f(a): {y/f(a)}
  MGU: {x/a, y/f(a)}
  Result: P(a, f(a)) and P(a, f(a)) ✓

Example 5: Unify P(x, f(y)) and P(g(z), f(a))
  x must equal g(z): {x/g(z)}
  y must equal a: {y/a}
  MGU: {x/g(z), y/a}
  Result: P(g(z), f(a)) and P(g(z), f(a)) ✓

Example 6: Unify P(x) and P(f(x))
  x must equal f(x)
  This is a circular dependency (occurs check fails)
  CANNOT UNIFY ✗
```

### Unification Algorithm

```
UNIFY(E1, E2, θ):
  1. If θ = FAILURE, return FAILURE
  2. If E1 = E2 (already identical), return θ
  3. If E1 is a variable:
     - If E1 occurs in E2, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E1/E2})
  4. If E2 is a variable:
     - If E2 occurs in E1, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E2/E1})
  5. If E1 and E2 are both compound (same functor, same arity):
     - Unify arguments one by one
  6. Otherwise, return FAILURE
```

### Unification Practice Table

| Expression 1 | Expression 2 | MGU | Result |
|--------------|--------------|-----|--------|
| P(x) | P(a) | {x/a} | P(a) |
| P(a) | P(a) | {} (empty) | P(a) |
| P(x) | Q(a) | FAIL | Different predicates |
| P(x,y) | P(y,a) | {x/a, y/a} | P(a,a) |
| P(f(x)) | P(f(a)) | {x/a} | P(f(a)) |
| P(x,x) | P(a,b) | FAIL | x can't be both a and b |
| P(x,f(a)) | P(b,y) | {x/b, y/f(a)} | P(b,f(a)) |
| P(x) | P(f(x)) | FAIL | Occurs check |

---

# Solved Exam Questions

## Exam Question 1: Truth Table and Validity

### Problem

Construct a truth table for the formula: (P → Q) ∧ (Q → P) ↔ (P ↔ Q)

Determine if this is a tautology, contradiction, or contingency.

### Solution

```
Step 1: Identify variables: P, Q (2 variables = 4 rows)

Step 2: Build subformulas:
- P → Q
- Q → P
- (P → Q) ∧ (Q → P)
- P ↔ Q
- (P → Q) ∧ (Q → P) ↔ (P ↔ Q)

Step 3: Truth Table:

| P | Q | P→Q | Q→P | (P→Q)∧(Q→P) | P↔Q | Final |
|---|---|-----|-----|-------------|-----|-------|
| T | T |  T  |  T  |      T      |  T  |   T   |
| T | F |  F  |  T  |      F      |  F  |   T   |
| F | T |  T  |  F  |      F      |  F  |   T   |
| F | F |  T  |  T  |      T      |  T  |   T   |

Step 4: Analysis:
All values in Final column are TRUE.

CONCLUSION: This is a TAUTOLOGY.

This demonstrates that $(P \to Q) \land (Q \to P)$ is logically equivalent to $(P \leftrightarrow Q)$, so the biconditional between them is always true.
(P → Q) ∧ (Q → P) is logically equivalent to P ↔ Q
"If P then Q, and if Q then P" means "P if and only if Q"
```

---

## Exam Question 2: FOL Translation

### Problem

Translate the following English sentences into First-Order Logic:

a) "All birds can fly"
b) "Some birds cannot fly"
c) "Penguins are birds that cannot fly"
d) "Every child loves some candy"
e) "There is a person who knows everyone"

### Solution

```
a) "All birds can fly"

   Analysis: For all x, if x is a bird, then x can fly
   
   FOL: ∀x (Bird(x) → CanFly(x))


b) "Some birds cannot fly"

   Analysis: There exists x such that x is a bird AND x cannot fly
   
   FOL: ∃x (Bird(x) ∧ ¬CanFly(x))


c) "Penguins are birds that cannot fly"

   Analysis: For all x, if x is a penguin, then x is a bird AND cannot fly
   
   FOL: ∀x (Penguin(x) → (Bird(x) ∧ ¬CanFly(x)))


d) "Every child loves some candy"

   Analysis: For all x, if x is a child, there exists y such that 
             y is candy AND x loves y
   
   FOL: ∀x (Child(x) → ∃y (Candy(y) ∧ Loves(x, y)))


e) "There is a person who knows everyone"

   Analysis: There exists x such that x is a person AND 
             for all y (if y is a person, x knows y)
   
   FOL: ∃x (Person(x) ∧ ∀y (Person(y) → Knows(x, y)))
   
   Alternative (if "everyone" means all people):
   ∃x ∀y (Person(y) → Knows(x, y))
```

---

## Exam Question 3: Inference Chain

### Problem

Given the following knowledge base, prove that "Tweety is mortal" using logical inference rules.

```
KB:
1. All birds are animals
2. All animals are mortal
3. Tweety is a bird
```

### Solution

```
Step 1: Convert to FOL

1. All birds are animals:     ∀x (Bird(x) → Animal(x))
2. All animals are mortal:    ∀x (Animal(x) → Mortal(x))
3. Tweety is a bird:          Bird(tweety)

Step 2: Apply Universal Instantiation

From (1), instantiate with x = tweety:
4. Bird(tweety) → Animal(tweety)    [UI on 1]

From (2), instantiate with x = tweety:
5. Animal(tweety) → Mortal(tweety)  [UI on 2]

Step 3: Apply Modus Ponens

From (3) and (4):
   Bird(tweety)                      [Given in 3]
   Bird(tweety) → Animal(tweety)     [From 4]
   ─────────────────────────────────
6. Animal(tweety)                    [Modus Ponens]

From (6) and (5):
   Animal(tweety)                    [From 6]
   Animal(tweety) → Mortal(tweety)   [From 5]
   ─────────────────────────────────
7. Mortal(tweety)                    [Modus Ponens]

CONCLUSION: Tweety is mortal ∎

Inference Chain:
┌────────────────────────────────────────────────────────────┐
│ Bird(tweety) → Animal(tweety) → Mortal(tweety)            │
│                                                            │
│ Using: Hypothetical Syllogism gives us:                    │
│ Bird(tweety) → Mortal(tweety)                             │
│                                                            │
│ With Bird(tweety), Modus Ponens gives: Mortal(tweety)     │
└────────────────────────────────────────────────────────────┘
```

---

## Exam Question 4: Resolution Proof

### Problem

Use resolution to prove that R follows from:
- P ∨ Q
- P → R
- Q → R

### Solution

```
Step 1: Convert to CNF

Given:
1. P ∨ Q           (already in CNF)
2. P → R           ≡ ¬P ∨ R
3. Q → R           ≡ ¬Q ∨ R

To prove R, negate it and add to clauses:
4. ¬R

Clauses:
C1: P ∨ Q
C2: ¬P ∨ R
C3: ¬Q ∨ R
C4: ¬R

Step 2: Apply Resolution

Resolve C2 and C4 (on R and ¬R):
   ¬P ∨ R
   ¬R
   ─────────
C5: ¬P

Resolve C3 and C4 (on R and ¬R):
   ¬Q ∨ R
   ¬R
   ─────────
C6: ¬Q

Resolve C1 and C5 (on P and ¬P):
   P ∨ Q
   ¬P
   ─────────
C7: Q

Resolve C6 and C7 (on Q and ¬Q):
   ¬Q
   Q
   ─────────
C8: □ (empty clause)

Step 3: Conclusion

We derived the empty clause □, which means:
{P ∨ Q, ¬P ∨ R, ¬Q ∨ R, ¬R} is UNSATISFIABLE

Therefore, {P ∨ Q, ¬P ∨ R, ¬Q ∨ R} ⊨ R

R is proven! ∎

Resolution Tree:
                   C1: P∨Q    C2: ¬P∨R    C3: ¬Q∨R    C4: ¬R
                       │           │            │          │
                       │           └─────┬──────┘          │
                       │                 │                 │
                       │            C5: ¬P     C6: ¬Q     │
                       │                 │          │      │
                       └────────┬────────┘          └──┬───┘
                                │                      │
                            C7: Q                  C6: ¬Q
                                │                      │
                                └──────────┬───────────┘
                                           │
                                        C8: □
```

## Unification

### Definition

```
UNIFICATION: Finding a substitution that makes two expressions identical.

SUBSTITUTION (θ): A mapping from variables to terms.
  θ = {x/a, y/f(b)}
  Means: Replace x with a, replace y with f(b)

UNIFIER: A substitution θ such that applying θ to both expressions
         makes them identical.

MOST GENERAL UNIFIER (MGU): The unifier that makes the 
         minimum commitment (most general substitution).
```

### Unification Examples

```
Example 1: Unify P(x) and P(a)
  x is a variable, a is a constant
  Substitution: θ = {x/a}
  Result: P(a) and P(a) ✓
  MGU: {x/a}

Example 2: Unify P(x, y) and P(a, b)
  Substitution: θ = {x/a, y/b}
  Result: P(a, b) and P(a, b) ✓
  MGU: {x/a, y/b}

Example 3: Unify P(x, x) and P(a, b)
  x must equal both a and b
  But a ≠ b
  CANNOT UNIFY ✗

Example 4: Unify P(x, f(x)) and P(a, y)
  x must equal a: {x/a}
  y must equal f(x) = f(a): {y/f(a)}
  MGU: {x/a, y/f(a)}
  Result: P(a, f(a)) and P(a, f(a)) ✓

Example 5: Unify P(x, f(y)) and P(g(z), f(a))
  x must equal g(z): {x/g(z)}
  y must equal a: {y/a}
  MGU: {x/g(z), y/a}
  Result: P(g(z), f(a)) and P(g(z), f(a)) ✓

Example 6: Unify P(x) and P(f(x))
  x must equal f(x)
  This is a circular dependency (occurs check fails)
  CANNOT UNIFY ✗
```

### Unification Algorithm

```
UNIFY(E1, E2, θ):
  1. If θ = FAILURE, return FAILURE
  2. If E1 = E2 (already identical), return θ
  3. If E1 is a variable:
     - If E1 occurs in E2, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E1/E2})
  4. If E2 is a variable:
     - If E2 occurs in E1, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E2/E1})
  5. If E1 and E2 are both compound (same functor, same arity):
     - Unify arguments one by one
  6. Otherwise, return FAILURE
```

### Unification Practice Table

| Expression 1 | Expression 2 | MGU | Result |
|--------------|--------------|-----|--------|
| P(x) | P(a) | {x/a} | P(a) |
| P(a) | P(a) | {} (empty) | P(a) |
| P(x) | Q(a) | FAIL | Different predicates |
| P(x,y) | P(y,a) | {x/a, y/a} | P(a,a) |
| P(f(x)) | P(f(a)) | {x/a} | P(f(a)) |
| P(x,x) | P(a,b) | FAIL | x can't be both a and b |
| P(x,f(a)) | P(b,y) | {x/b, y/f(a)} | P(b,f(a)) |
| P(x) | P(f(x)) | FAIL | Occurs check |

---

(P → Q) ∧ (Q → P) is logically equivalent to P ↔ Q
"If P then Q, and if Q then P" means "P if and only if Q"
```

---

## Exam Question 2: FOL Translation

### Problem

Translate the following English sentences into First-Order Logic:

a) "All birds can fly"
b) "Some birds cannot fly"
c) "Penguins are birds that cannot fly"
d) "Every child loves some candy"
e) "There is a person who knows everyone"

### Solution

```
a) "All birds can fly"

   Analysis: For all x, if x is a bird, then x can fly
   
   FOL: ∀x (Bird(x) → CanFly(x))


b) "Some birds cannot fly"

   Analysis: There exists x such that x is a bird AND x cannot fly
   
   FOL: ∃x (Bird(x) ∧ ¬CanFly(x))


c) "Penguins are birds that cannot fly"

   Analysis: For all x, if x is a penguin, then x is a bird AND cannot fly
   
   FOL: ∀x (Penguin(x) → (Bird(x) ∧ ¬CanFly(x)))


d) "Every child loves some candy"

   Analysis: For all x, if x is a child, there exists y such that 
             y is candy AND x loves y
   
   FOL: ∀x (Child(x) → ∃y (Candy(y) ∧ Loves(x, y)))


e) "There is a person who knows everyone"

   Analysis: There exists x such that x is a person AND 
             for all y (if y is a person, x knows y)
   
   FOL: ∃x (Person(x) ∧ ∀y (Person(y) → Knows(x, y)))
   
   Alternative (if "everyone" means all people):
   ∃x ∀y (Person(y) → Knows(x, y))
```

---

## Exam Question 3: Inference Chain

### Problem

Given the following knowledge base, prove that "Tweety is mortal" using logical inference rules.

```
KB:
1. All birds are animals
2. All animals are mortal
3. Tweety is a bird
```

### Solution

```
Step 1: Convert to FOL

1. All birds are animals:     ∀x (Bird(x) → Animal(x))
2. All animals are mortal:    ∀x (Animal(x) → Mortal(x))
3. Tweety is a bird:          Bird(tweety)

Step 2: Apply Universal Instantiation

From (1), instantiate with x = tweety:
4. Bird(tweety) → Animal(tweety)    [UI on 1]

From (2), instantiate with x = tweety:
5. Animal(tweety) → Mortal(tweety)  [UI on 2]

Step 3: Apply Modus Ponens

From (3) and (4):
   Bird(tweety)                      [Given in 3]
   Bird(tweety) → Animal(tweety)     [From 4]
   ─────────────────────────────────
6. Animal(tweety)                    [Modus Ponens]

From (6) and (5):
   Animal(tweety)                    [From 6]
   Animal(tweety) → Mortal(tweety)   [From 5]
   ─────────────────────────────────
7. Mortal(tweety)                    [Modus Ponens]

CONCLUSION: Tweety is mortal ∎

Inference Chain:
┌────────────────────────────────────────────────────────────┐
│ Bird(tweety) → Animal(tweety) → Mortal(tweety)            │
│                                                            │
│ Using: Hypothetical Syllogism gives us:                    │
│ Bird(tweety) → Mortal(tweety)                             │
│                                                            │
│ With Bird(tweety), Modus Ponens gives: Mortal(tweety)     │
└────────────────────────────────────────────────────────────┘
```

---

## Exam Question 4: Resolution Proof

### Problem

Use resolution to prove that R follows from:
- P ∨ Q
- P → R
- Q → R

### Solution

```
Step 1: Convert to CNF

Given:
1. P ∨ Q           (already in CNF)
2. P → R           ≡ ¬P ∨ R
3. Q → R           ≡ ¬Q ∨ R

To prove R, negate it and add to clauses:
4. ¬R

Clauses:
C1: P ∨ Q
C2: ¬P ∨ R
C3: ¬Q ∨ R
C4: ¬R

Step 2: Apply Resolution

Resolve C2 and C4 (on R and ¬R):
   ¬P ∨ R
   ¬R
   ─────────
C5: ¬P

Resolve C3 and C4 (on R and ¬R):
   ¬Q ∨ R
   ¬R
   ─────────
C6: ¬Q

Resolve C1 and C5 (on P and ¬P):
   P ∨ Q
   ¬P
   ─────────
C7: Q

Resolve C6 and C7 (on Q and ¬Q):
   ¬Q
   Q
   ─────────
C8: □ (empty clause)

Step 3: Conclusion

We derived the empty clause □, which means:
{P ∨ Q, ¬P ∨ R, ¬Q ∨ R, ¬R} is UNSATISFIABLE

Therefore, {P ∨ Q, ¬P ∨ R, ¬Q ∨ R} ⊨ R

R is proven! ∎

Resolution Tree:
                   C1: P∨Q    C2: ¬P∨R    C3: ¬Q∨R    C4: ¬R
                       │           │            │          │
                       │           └─────┬──────┘          │
                       │                 │                 │
                       │            C5: ¬P     C6: ¬Q     │
                       │                 │          │      │
                       └────────┬────────┘          └──┬───┘
                                │                      │
                            C7: Q                  C6: ¬Q
                                │                      │
                                └──────────┬───────────┘
                                           │
                                        C8: □
```

## Unification

### Definition

```
UNIFICATION: Finding a substitution that makes two expressions identical.

SUBSTITUTION (θ): A mapping from variables to terms.
  θ = {x/a, y/f(b)}
  Means: Replace x with a, replace y with f(b)

UNIFIER: A substitution θ such that applying θ to both expressions
         makes them identical.

MOST GENERAL UNIFIER (MGU): The unifier that makes the 
         minimum commitment (most general substitution).
```

### Unification Examples

```
Example 1: Unify P(x) and P(a)
  x is a variable, a is a constant
  Substitution: θ = {x/a}
  Result: P(a) and P(a) ✓
  MGU: {x/a}

Example 2: Unify P(x, y) and P(a, b)
  Substitution: θ = {x/a, y/b}
  Result: P(a, b) and P(a, b) ✓
  MGU: {x/a, y/b}

Example 3: Unify P(x, x) and P(a, b)
  x must equal both a and b
  But a ≠ b
  CANNOT UNIFY ✗

Example 4: Unify P(x, f(x)) and P(a, y)
  x must equal a: {x/a}
  y must equal f(x) = f(a): {y/f(a)}
  MGU: {x/a, y/f(a)}
  Result: P(a, f(a)) and P(a, f(a)) ✓

Example 5: Unify P(x, f(y)) and P(g(z), f(a))
  x must equal g(z): {x/g(z)}
  y must equal a: {y/a}
  MGU: {x/g(z), y/a}
  Result: P(g(z), f(a)) and P(g(z), f(a)) ✓

Example 6: Unify P(x) and P(f(x))
  x must equal f(x)
  This is a circular dependency (occurs check fails)
  CANNOT UNIFY ✗
```

### Unification Algorithm

```
UNIFY(E1, E2, θ):
  1. If θ = FAILURE, return FAILURE
  2. If E1 = E2 (already identical), return θ
  3. If E1 is a variable:
     - If E1 occurs in E2, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E1/E2})
  4. If E2 is a variable:
     - If E2 occurs in E1, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E2/E1})
  5. If E1 and E2 are both compound (same functor, same arity):
     - Unify arguments one by one
  6. Otherwise, return FAILURE
```

### Unification Practice Table

| Expression 1 | Expression 2 | MGU | Result |
|--------------|--------------|-----|--------|
| P(x) | P(a) | {x/a} | P(a) |
| P(a) | P(a) | {} (empty) | P(a) |
| P(x) | Q(a) | FAIL | Different predicates |
| P(x,y) | P(y,a) | {x/a, y/a} | P(a,a) |
| P(f(x)) | P(f(a)) | {x/a} | P(f(a)) |
| P(x,x) | P(a,b) | FAIL | x can't be both a and b |
| P(x,f(a)) | P(b,y) | {x/b, y/f(a)} | P(b,f(a)) |
| P(x) | P(f(x)) | FAIL | Occurs check |

---

# Solved Exam Questions

## Exam Question 1: Truth Table and Validity

### Problem

Construct a truth table for the formula: (P → Q) ∧ (Q → P) ↔ (P ↔ Q)

Determine if this is a tautology, contradiction, or contingency.

### Solution

```
Step 1: Identify variables: P, Q (2 variables =  4 rows)

Step 2: Build subformulas:
- P → Q
- Q → P
- (P → Q) ∧ (Q → P)
- P ↔ Q
- (P → Q) ∧ (Q → P) ↔ (P ↔ Q)

Step 3: Truth Table:

| P | Q | P→Q | Q→P | (P→Q)∧(Q→P) | P↔Q | Final |
|---|---|-----|-----|-------------|-----|-------|
| T | T |  T  |  T  |      T      |  T  |   T   |
| T | F |  F  |  T  |      F      |  F  |   T   |
| F | T |  T  |  F  |      F      |  F  |   T   |
| F | F |  T  |  T  |      T      |  T  |   T   |

Step 4: Analysis:
All values in Final column are TRUE.

CONCLUSION: This is a TAUTOLOGY.

This demonstrates that (completed):
(P → Q) ∧ (Q → P) is logically equivalent to P ↔ Q
"If P then Q, and if Q then P" means "P if and only if Q".
```
(P → Q) ∧ (Q → P) is logically equivalent to P ↔ Q
"If P then Q, and if Q then P" means "P if and only if Q"
```

---

## Exam Question 2: FOL Translation

### Problem

Translate the following English sentences into First-Order Logic:

a) "All birds can fly"
b) "Some birds cannot fly"
c) "Penguins are birds that cannot fly"
d) "Every child loves some candy"
e) "There is a person who knows everyone"

### Solution

```
a) "All birds can fly"

   Analysis: For all x, if x is a bird, then x can fly
   
   FOL: ∀x (Bird(x) → CanFly(x))


b) "Some birds cannot fly"

   Analysis: There exists x such that x is a bird AND x cannot fly
   
   FOL: ∃x (Bird(x) ∧ ¬CanFly(x))


c) "Penguins are birds that cannot fly"

   Analysis: For all x, if x is a penguin, then x is a bird AND cannot fly
   
   FOL: ∀x (Penguin(x) → (Bird(x) ∧ ¬CanFly(x)))


d) "Every child loves some candy"

   Analysis: For all x, if x is a child, there exists y such that 
             y is candy AND x loves y
   
   FOL: ∀x (Child(x) → ∃y (Candy(y) ∧ Loves(x, y)))


e) "There is a person who knows everyone"

   Analysis: There exists x such that x is a person AND 
             for all y (if y is a person, x knows y)
   
   FOL: ∃x (Person(x) ∧ ∀y (Person(y) → Knows(x, y)))
   
   Alternative (if "everyone" means all people):
   ∃x ∀y (Person(y) → Knows(x, y))
```

---

## Exam Question 3: Inference Chain

### Problem

Given the following knowledge base, prove that "Tweety is mortal" using logical inference rules.

```
KB:
1. All birds are animals
2. All animals are mortal
3. Tweety is a bird
```

### Solution

```
Step 1: Convert to FOL

1. All birds are animals:     ∀x (Bird(x) → Animal(x))
2. All animals are mortal:    ∀x (Animal(x) → Mortal(x))
3. Tweety is a bird:          Bird(tweety)

Step 2: Apply Universal Instantiation

From (1), instantiate with x = tweety:
4. Bird(tweety) → Animal(tweety)    [UI on 1]

From (2), instantiate with x = tweety:
5. Animal(tweety) → Mortal(tweety)  [UI on 2]

Step 3: Apply Modus Ponens

From (3) and (4):
   Bird(tweety)                      [Given in 3]
   Bird(tweety) → Animal(tweety)     [From 4]
   ─────────────────────────────────
6. Animal(tweety)                    [Modus Ponens]

From (6) and (5):
   Animal(tweety)                    [From 6]
   Animal(tweety) → Mortal(tweety)   [From 5]
   ─────────────────────────────────
7. Mortal(tweety)                    [Modus Ponens]

CONCLUSION: Tweety is mortal ∎

Inference Chain:
┌────────────────────────────────────────────────────────────┐
│ Bird(tweety) → Animal(tweety) → Mortal(tweety)            │
│                                                            │
│ Using: Hypothetical Syllogism gives us:                    │
│ Bird(tweety) → Mortal(tweety)                             │
│                                                            │
│ With Bird(tweety), Modus Ponens gives: Mortal(tweety)     │
└────────────────────────────────────────────────────────────┘
```

---

## Exam Question 4: Resolution Proof

### Problem

Use resolution to prove that R follows from:
- P ∨ Q
- P → R
- Q → R

### Solution

```
Step 1: Convert to CNF

Given:
1. P ∨ Q           (already in CNF)
2. P → R           ≡ ¬P ∨ R
3. Q → R           ≡ ¬Q ∨ R

To prove R, negate it and add to clauses:
4. ¬R

Clauses:
C1: P ∨ Q
C2: ¬P ∨ R
C3: ¬Q ∨ R
C4: ¬R

Step 2: Apply Resolution

Resolve C2 and C4 (on R and ¬R):
   ¬P ∨ R
   ¬R
   ─────────
C5: ¬P

Resolve C3 and C4 (on R and ¬R):
   ¬Q ∨ R
   ¬R
   ─────────
C6: ¬Q

Resolve C1 and C5 (on P and ¬P):
   P ∨ Q
   ¬P
   ─────────
C7: Q

Resolve C6 and C7 (on Q and ¬Q):
   ¬Q
   Q
   ─────────
C8: □ (empty clause)

Step 3: Conclusion

We derived the empty clause □, which means:
{P ∨ Q, ¬P ∨ R, ¬Q ∨ R, ¬R} is UNSATISFIABLE

Therefore, {P ∨ Q, ¬P ∨ R, ¬Q ∨ R} ⊨ R

R is proven! ∎

Resolution Tree:
                   C1: P∨Q    C2: ¬P∨R    C3: ¬Q∨R    C4: ¬R
                       │           │            │          │
                       │           └─────┬──────┘          │
                       │                 │                 │
                       │            C5: ¬P     C6: ¬Q     │
                       │                 │          │      │
                       └────────┬────────┘          └──┬───┘
                                │                      │
                            C7: Q                  C6: ¬Q
                                │                      │
                                └──────────┬───────────┘
                                           │
                                        C8: □
```

## Unification

### Definition

```
UNIFICATION: Finding a substitution that makes two expressions identical.

SUBSTITUTION (θ): A mapping from variables to terms.
  θ = {x/a, y/f(b)}
  Means: Replace x with a, replace y with f(b)

UNIFIER: A substitution θ such that applying θ to both expressions
         makes them identical.

MOST GENERAL UNIFIER (MGU): The unifier that makes the 
         minimum commitment (most general substitution).
```

### Unification Examples

```
Example 1: Unify P(x) and P(a)
  x is a variable, a is a constant
  Substitution: θ = {x/a}
  Result: P(a) and P(a) ✓
  MGU: {x/a}

Example 2: Unify P(x, y) and P(a, b)
  Substitution: θ = {x/a, y/b}
  Result: P(a, b) and P(a, b) ✓
  MGU: {x/a, y/b}

Example 3: Unify P(x, x) and P(a, b)
  x must equal both a and b
  But a ≠ b
  CANNOT UNIFY ✗

Example 4: Unify P(x, f(x)) and P(a, y)
  x must equal a: {x/a}
  y must equal f(x) = f(a): {y/f(a)}
  MGU: {x/a, y/f(a)}
  Result: P(a, f(a)) and P(a, f(a)) ✓

Example 5: Unify P(x, f(y)) and P(g(z), f(a))
  x must equal g(z): {x/g(z)}
  y must equal a: {y/a}
  MGU: {x/g(z), y/a}
  Result: P(g(z), f(a)) and P(g(z), f(a)) ✓

Example 6: Unify P(x) and P(f(x))
  x must equal f(x)
  This is a circular dependency (occurs check fails)
  CANNOT UNIFY ✗
```

### Unification Algorithm

```
UNIFY(E1, E2, θ):
  1. If θ = FAILURE, return FAILURE
  2. If E1 = E2 (already identical), return θ
  3. If E1 is a variable:
     - If E1 occurs in E2, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E1/E2})
  4. If E2 is a variable:
     - If E2 occurs in E1, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E2/E1})
  5. If E1 and E2 are both compound (same functor, same arity):
     - Unify arguments one by one
  6. Otherwise, return FAILURE
```

### Unification Practice Table

| Expression 1 | Expression 2 | MGU | Result |
|--------------|--------------|-----|--------|
| P(x) | P(a) | {x/a} | P(a) |
| P(a) | P(a) | {} (empty) | P(a) |
| P(x) | Q(a) | FAIL | Different predicates |
| P(x,y) | P(y,a) | {x/a, y/a} | P(a,a) |
| P(f(x)) | P(f(a)) | {x/a} | P(f(a)) |
| P(x,x) | P(a,b) | FAIL | x can't be both a and b |
| P(x,f(a)) | P(b,y) | {x/b, y/f(a)} | P(b,f(a)) |
| P(x) | P(f(x)) | FAIL | Occurs check |

---

(P → Q) ∧ (Q → P) is logically equivalent to P ↔ Q
"If P then Q, and if Q then P" means "P if and only if Q"
```

---

## Exam Question 2: FOL Translation

### Problem

Translate the following English sentences into First-Order Logic:

a) "All birds can fly"
b) "Some birds cannot fly"
c) "Penguins are birds that cannot fly"
d) "Every child loves some candy"
e) "There is a person who knows everyone"

### Solution

```
a) "All birds can fly"

   Analysis: For all x, if x is a bird, then x can fly
   
   FOL: ∀x (Bird(x) → CanFly(x))


b) "Some birds cannot fly"

   Analysis: There exists x such that x is a bird AND x cannot fly
   
   FOL: ∃x (Bird(x) ∧ ¬CanFly(x))


c) "Penguins are birds that cannot fly"

   Analysis: For all x, if x is a penguin, then x is a bird AND cannot fly
   
   FOL: ∀x (Penguin(x) → (Bird(x) ∧ ¬CanFly(x)))


d) "Every child loves some candy"

   Analysis: For all x, if x is a child, there exists y such that 
             y is candy AND x loves y
   
   FOL: ∀x (Child(x) → ∃y (Candy(y) ∧ Loves(x, y)))


e) "There is a person who knows everyone"

   Analysis: There exists x such that x is a person AND 
             for all y (if y is a person, x knows y)
   
   FOL: ∃x (Person(x) ∧ ∀y (Person(y) → Knows(x, y)))
   
   Alternative (if "everyone" means all people):
   ∃x ∀y (Person(y) → Knows(x, y))
```

---

## Exam Question 3: Inference Chain

### Problem

Given the following knowledge base, prove that "Tweety is mortal" using logical inference rules.

```
KB:
1. All birds are animals
2. All animals are mortal
3. Tweety is a bird
```

### Solution

```
Step 1: Convert to FOL

1. All birds are animals:     ∀x (Bird(x) → Animal(x))
2. All animals are mortal:    ∀x (Animal(x) → Mortal(x))
3. Tweety is a bird:          Bird(tweety)

Step 2: Apply Universal Instantiation

From (1), instantiate with x = tweety:
4. Bird(tweety) → Animal(tweety)    [UI on 1]

From (2), instantiate with x = tweety:
5. Animal(tweety) → Mortal(tweety)  [UI on 2]

Step 3: Apply Modus Ponens

From (3) and (4):
   Bird(tweety)                      [Given in 3]
   Bird(tweety) → Animal(tweety)     [From 4]
   ─────────────────────────────────
6. Animal(tweety)                    [Modus Ponens]

From (6) and (5):
   Animal(tweety)                    [From 6]
   Animal(tweety) → Mortal(tweety)   [From 5]
   ─────────────────────────────────
7. Mortal(tweety)                    [Modus Ponens]

CONCLUSION: Tweety is mortal ∎

Inference Chain:
┌────────────────────────────────────────────────────────────┐
│ Bird(tweety) → Animal(tweety) → Mortal(tweety)            │
│                                                            │
│ Using: Hypothetical Syllogism gives us:                    │
│ Bird(tweety) → Mortal(tweety)                             │
│                                                            │
│ With Bird(tweety), Modus Ponens gives: Mortal(tweety)     │
└────────────────────────────────────────────────────────────┘
```

---

## Exam Question 4: Resolution Proof

### Problem

Use resolution to prove that R follows from:
- P ∨ Q
- P → R
- Q → R

### Solution

```
Step 1: Convert to CNF

Given:
1. P ∨ Q           (already in CNF)
2. P → R           ≡ ¬P ∨ R
3. Q → R           ≡ ¬Q ∨ R

To prove R, negate it and add to clauses:
4. ¬R

Clauses:
C1: P ∨ Q
C2: ¬P ∨ R
C3: ¬Q ∨ R
C4: ¬R

Step 2: Apply Resolution

Resolve C2 and C4 (on R and ¬R):
   ¬P ∨ R
   ¬R
   ─────────
C5: ¬P

Resolve C3 and C4 (on R and ¬R):
   ¬Q ∨ R
   ¬R
   ─────────
C6: ¬Q

Resolve C1 and C5 (on P and ¬P):
   P ∨ Q
   ¬P
   ─────────
C7: Q

Resolve C6 and C7 (on Q and ¬Q):
   ¬Q
   Q
   ─────────
C8: □ (empty clause)

Step 3: Conclusion

We derived the empty clause □, which means:
{P ∨ Q, ¬P ∨ R, ¬Q ∨ R, ¬R} is UNSATISFIABLE

Therefore, {P ∨ Q, ¬P ∨ R, ¬Q ∨ R} ⊨ R

R is proven! ∎

Resolution Tree:
                   C1: P∨Q    C2: ¬P∨R    C3: ¬Q∨R    C4: ¬R
                       │           │            │          │
                       │           └─────┬──────┘          │
                       │                 │                 │
                       │            C5: ¬P     C6: ¬Q     │
                       │                 │          │      │
                       └────────┬────────┘          └──┬───┘
                                │                      │
                            C7: Q                  C6: ¬Q
                                │                      │
                                └──────────┬───────────┘
                                           │
                                        C8: □
```

## Unification

### Definition

```
UNIFICATION: Finding a substitution that makes two expressions identical.

SUBSTITUTION (θ): A mapping from variables to terms.
  θ = {x/a, y/f(b)}
  Means: Replace x with a, replace y with f(b)

UNIFIER: A substitution θ such that applying θ to both expressions
         makes them identical.

MOST GENERAL UNIFIER (MGU): The unifier that makes the 
         minimum commitment (most general substitution).
```

### Unification Examples

```
Example 1: Unify P(x) and P(a)
  x is a variable, a is a constant
  Substitution: θ = {x/a}
  Result: P(a) and P(a) ✓
  MGU: {x/a}

Example 2: Unify P(x, y) and P(a, b)
  Substitution: θ = {x/a, y/b}
  Result: P(a, b) and P(a, b) ✓
  MGU: {x/a, y/b}

Example 3: Unify P(x, x) and P(a, b)
  x must equal both a and b
  But a ≠ b
  CANNOT UNIFY ✗

Example 4: Unify P(x, f(x)) and P(a, y)
  x must equal a: {x/a}
  y must equal f(x) = f(a): {y/f(a)}
  MGU: {x/a, y/f(a)}
  Result: P(a, f(a)) and P(a, f(a)) ✓

Example 5: Unify P(x, f(y)) and P(g(z), f(a))
  x must equal g(z): {x/g(z)}
  y must equal a: {y/a}
  MGU: {x/g(z), y/a}
  Result: P(g(z), f(a)) and P(g(z), f(a)) ✓

Example 6: Unify P(x) and P(f(x))
  x must equal f(x)
  This is a circular dependency (occurs check fails)
  CANNOT UNIFY ✗
```

### Unification Algorithm

```
UNIFY(E1, E2, θ):
  1. If θ = FAILURE, return FAILURE
  2. If E1 = E2 (already identical), return θ
  3. If E1 is a variable:
     - If E1 occurs in E2, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E1/E2})
  4. If E2 is a variable:
     - If E2 occurs in E1, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E2/E1})
  5. If E1 and E2 are both compound (same functor, same arity):
     - Unify arguments one by one
  6. Otherwise, return FAILURE
```

### Unification Practice Table

| Expression 1 | Expression 2 | MGU | Result |
|--------------|--------------|-----|--------|
| P(x) | P(a) | {x/a} | P(a) |
| P(a) | P(a) | {} (empty) | P(a) |
| P(x) | Q(a) | FAIL | Different predicates |
| P(x,y) | P(y,a) | {x/a, y/a} | P(a,a) |
| P(f(x)) | P(f(a)) | {x/a} | P(f(a)) |
| P(x,x) | P(a,b) | FAIL | x can't be both a and b |
| P(x,f(a)) | P(b,y) | {x/b, y/f(a)} | P(b,f(a)) |
| P(x) | P(f(x)) | FAIL | Occurs check |

---

(P → Q) ∧ (Q → P) is logically equivalent to P ↔ Q
"If P then Q, and if Q then P" means "P if and only if Q"
```

---

## Exam Question 2: FOL Translation

### Problem

Translate the following English sentences into First-Order Logic:

a) "All birds can fly"
b) "Some birds cannot fly"
c) "Penguins are birds that cannot fly"
d) "Every child loves some candy"
e) "There is a person who knows everyone"

### Solution

```
a) "All birds can fly"

   Analysis: For all x, if x is a bird, then x can fly
   
   FOL: ∀x (Bird(x) → CanFly(x))


b) "Some birds cannot fly"

   Analysis: There exists x such that x is a bird AND x cannot fly
   
   FOL: ∃x (Bird(x) ∧ ¬CanFly(x))


c) "Penguins are birds that cannot fly"

   Analysis: For all x, if x is a penguin, then x is a bird AND cannot fly
   
   FOL: ∀x (Penguin(x) → (Bird(x) ∧ ¬CanFly(x)))


d) "Every child loves some candy"

   Analysis: For all x, if x is a child, there exists y such that 
             y is candy AND x loves y
   
   FOL: ∀x (Child(x) → ∃y (Candy(y) ∧ Loves(x, y)))


e) "There is a person who knows everyone"

   Analysis: There exists x such that x is a person AND 
             for all y (if y is a person, x knows y)
   
   FOL: ∃x (Person(x) ∧ ∀y (Person(y) → Knows(x, y)))
   
   Alternative (if "everyone" means all people):
   ∃x ∀y (Person(y) → Knows(x, y))
```

---

## Exam Question 3: Inference Chain

### Problem

Given the following knowledge base, prove that "Tweety is mortal" using logical inference rules.

```
KB:
1. All birds are animals
2. All animals are mortal
3. Tweety is a bird
```

### Solution

```
Step 1: Convert to FOL

1. All birds are animals:     ∀x (Bird(x) → Animal(x))
2. All animals are mortal:    ∀x (Animal(x) → Mortal(x))
3. Tweety is a bird:          Bird(tweety)

Step 2: Apply Universal Instantiation

From (1), instantiate with x = tweety:
4. Bird(tweety) → Animal(tweety)    [UI on 1]

From (2), instantiate with x = tweety:
5. Animal(tweety) → Mortal(tweety)  [UI on 2]

Step 3: Apply Modus Ponens

From (3) and (4):
   Bird(tweety)                      [Given in 3]
   Bird(tweety) → Animal(tweety)     [From 4]
   ─────────────────────────────────
6. Animal(tweety)                    [Modus Ponens]

From (6) and (5):
   Animal(tweety)                    [From 6]
   Animal(tweety) → Mortal(tweety)   [From 5]
   ─────────────────────────────────
7. Mortal(tweety)                    [Modus Ponens]

CONCLUSION: Tweety is mortal ∎

Inference Chain:
┌────────────────────────────────────────────────────────────┐
│ Bird(tweety) → Animal(tweety) → Mortal(tweety)            │
│                                                            │
│ Using: Hypothetical Syllogism gives us:                    │
│ Bird(tweety) → Mortal(tweety)                             │
│                                                            │
│ With Bird(tweety), Modus Ponens gives: Mortal(tweety)     │
└────────────────────────────────────────────────────────────┘
```

---

## Exam Question 4: Resolution Proof

### Problem

Use resolution to prove that R follows from:
- P ∨ Q
- P → R
- Q → R

### Solution

```
Step 1: Convert to CNF

Given:
1. P ∨ Q           (already in CNF)
2. P → R           ≡ ¬P ∨ R
3. Q → R           ≡ ¬Q ∨ R

To prove R, negate it and add to clauses:
4. ¬R

Clauses:
C1: P ∨ Q
C2: ¬P ∨ R
C3: ¬Q ∨ R
C4: ¬R

Step 2: Apply Resolution

Resolve C2 and C4 (on R and ¬R):
   ¬P ∨ R
   ¬R
   ─────────
C5: ¬P

Resolve C3 and C4 (on R and ¬R):
   ¬Q ∨ R
   ¬R
   ─────────
C6: ¬Q

Resolve C1 and C5 (on P and ¬P):
   P ∨ Q
   ¬P
   ─────────
C7: Q

Resolve C6 and C7 (on Q and ¬Q):
   ¬Q
   Q
   ─────────
C8: □ (empty clause)

Step 3: Conclusion

We derived the empty clause □, which means:
{P ∨ Q, ¬P ∨ R, ¬Q ∨ R, ¬R} is UNSATISFIABLE

Therefore, {P ∨ Q, ¬P ∨ R, ¬Q ∨ R} ⊨ R

R is proven! ∎

Resolution Tree:
                   C1: P∨Q    C2: ¬P∨R    C3: ¬Q∨R    C4: ¬R
                       │           │            │          │
                       │           └─────┬──────┘          │
                       │                 │                 │
                       │            C5: ¬P     C6: ¬Q     │
                       │                 │          │      │
                       └────────┬────────┘          └──┬───┘
                                │                      │
                            C7: Q                  C6: ¬Q
                                │                      │
                                └──────────┬───────────┘
                                           │
                                        C8: □
```

## Unification

### Definition

```
UNIFICATION: Finding a substitution that makes two expressions identical.

SUBSTITUTION (θ): A mapping from variables to terms.
  θ = {x/a, y/f(b)}
  Means: Replace x with a, replace y with f(b)

UNIFIER: A substitution θ such that applying θ to both expressions
         makes them identical.

MOST GENERAL UNIFIER (MGU): The unifier that makes the 
         minimum commitment (most general substitution).
```

### Unification Examples

```
Example 1: Unify P(x) and P(a)
  x is a variable, a is a constant
  Substitution: θ = {x/a}
  Result: P(a) and P(a) ✓
  MGU: {x/a}

Example 2: Unify P(x, y) and P(a, b)
  Substitution: θ = {x/a, y/b}
  Result: P(a, b) and P(a, b) ✓
  MGU: {x/a, y/b}

Example 3: Unify P(x, x) and P(a, b)
  x must equal both a and b
  But a ≠ b
  CANNOT UNIFY ✗

Example 4: Unify P(x, f(x)) and P(a, y)
  x must equal a: {x/a}
  y must equal f(x) = f(a): {y/f(a)}
  MGU: {x/a, y/f(a)}
  Result: P(a, f(a)) and P(a, f(a)) ✓

Example 5: Unify P(x, f(y)) and P(g(z), f(a))
  x must equal g(z): {x/g(z)}
  y must equal a: {y/a}
  MGU: {x/g(z), y/a}
  Result: P(g(z), f(a)) and P(g(z), f(a)) ✓

Example 6: Unify P(x) and P(f(x))
  x must equal f(x)
  This is a circular dependency (occurs check fails)
  CANNOT UNIFY ✗
```

### Unification Algorithm

```
UNIFY(E1, E2, θ):
  1. If θ = FAILURE, return FAILURE
  2. If E1 = E2 (already identical), return θ
  3. If E1 is a variable:
     - If E1 occurs in E2, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E1/E2})
  4. If E2 is a variable:
     - If E2 occurs in E1, return FAILURE (occurs check)
     - Return UNIFY(rest, rest, θ ∪ {E2/E1})
  5. If E1 and E2 are both compound (same functor, same arity):
     - Unify arguments one by one
  6. Otherwise, return FAILURE
```

### Unification Practice Table

| Expression 1 | Expression 2 | MGU | Result |
|--------------|--------------|-----|--------|
| P(x) | P(a) | {x/a} | P(a) |
| P(a) | P(a) | {} (empty) | P(a) |
| P(x) | Q(a) | FAIL | Different predicates |
| P(x,y) | P(y,a) | {x/a, y/a} | P(a,a) |
| P(f(x)) | P(f(a)) | {x/a} | P(f(a)) |
| P(x,x) | P(a,b) | FAIL | x can't be both a and b |
| P(x,f(a)) | P(b,y) | {x/b, y/f(a)} | P(b,f(a)) |
| P(x) | P(f(x)) | FAIL | Occurs check |

---

# Solved Exam Questions

## Exam Question 1: Truth Table and Validity

### Problem

Construct a truth table for the formula: (P → Q) ∧ (Q → P) ↔ (P ↔ Q)

Determine if this is a tautology, contradiction, or contingency.

### Solution

```
Step 1: Identify variables: P, Q (2 variables = 4 rows)

Step 2: Build subformulas:
- P → Q
- Q → P
- (P → Q) ∧ (Q → P)
- P ↔ Q
- (P → Q) ∧ (Q → P) ↔ (P ↔ Q)

Step 3: Truth Table:

| P | Q | P→Q | Q→P | (P→Q)∧(Q→P) | P↔Q | Final |
|---|---|-----|-----|-------------|-----|-------|
| T | T |  T  |  T  |      T      |  T  |   T   |
| T | F |  F  |  T  |      F      |  F  |   T   |
| F | T |  T  |  F  |      F      |  F  |   T   |
| F | F |  T  |  T  |      T      |  T  |   T   |

Step 4: Analysis:
All values in Final column are TRUE.

CONCLUSION: This is a TAUTOLOGY.

This demonstrates that:
(P → Q) ∧ (Q → P) is logically equivalent to P ↔ Q
"If P then Q, and if Q then P" means "P if and only if Q".
```