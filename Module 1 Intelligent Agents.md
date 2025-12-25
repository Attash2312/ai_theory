# Module 1: Intelligent Agents - Complete Guide

## Table of Contents
1. [Introduction to AI](#introduction-to-ai)
2. [Intelligent Agents](#intelligent-agents)
3. [PEAS Framework](#peas-framework)
4. [Task Environment (PAGE Properties)](#task-environment-page-properties)
5. [Agent Architectures](#agent-architectures)
6. [Solved Exam Questions](#solved-exam-questions)

---

# Introduction to AI

## Definition of Artificial Intelligence

**Artificial Intelligence (AI)** is the branch of computer science that aims to create machines capable of performing tasks that typically require human intelligence.

### Four Approaches to AI

| Approach | Description |
|----------|-------------|
| **Thinking Humanly** | Machines that think like humans (cognitive modeling) |
| **Thinking Rationally** | Machines that think logically (laws of thought) |
| **Acting Humanly** | Machines that act like humans (Turing Test) |
| **Acting Rationally** | Machines that act to achieve best outcome (Rational Agents) |

> **Modern AI Focus:** Acting Rationally - Building rational agents that maximize goal achievement.

## AI as Rational Decision-Making Systems

A **rational agent** is one that:
- Perceives its environment through sensors
- Acts through actuators
- Chooses actions that maximize expected performance measure
- Makes decisions based on percept sequence and built-in knowledge

```
Rationality depends on:
├── Performance measure (defines success)
├── Prior knowledge of environment
├── Possible actions available
└── Percept sequence to date
```

---

# Intelligent Agents

## Definition

An **Intelligent Agent** is anything that:
1. **Perceives** its environment through **sensors**
2. **Acts** upon the environment through **actuators**
3. Operates **autonomously**
4. Has **goals** or preferences

## Components of an Intelligent Agent

```
┌─────────────────────────────────────────────────┐
│                 INTELLIGENT AGENT               │
├─────────────────────────────────────────────────┤
│                                                 │
│   ┌─────────┐     ┌─────────┐     ┌─────────┐  │
│   │ SENSORS │────▶│  AGENT  │────▶│ACTUATORS│  │
│   └─────────┘     │ PROGRAM │     └─────────┘  │
│        ▲          └─────────┘          │       │
│        │                               │       │
│        │         ENVIRONMENT           │       │
│        └───────────────────────────────┘       │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Key Components

| Component | Description | Examples |
|-----------|-------------|----------|
| **Sensors** | Devices that perceive environment | Cameras, microphones, thermometers, GPS |
| **Actuators** | Devices that act on environment | Motors, displays, speakers, signals |
| **Agent Function** | Maps percept sequence to action | f: P* → A |
| **Agent Program** | Implementation of agent function | Software/Hardware |

## Types of Agents

### 1. Autonomous Agents
- Operate without direct human intervention
- Learn and adapt from experience
- Make independent decisions
- Example: Self-driving cars, robot vacuum

### 2. Multi-Agent Systems (MAS)
- Multiple agents interacting in shared environment
- Can be cooperative or competitive
- Coordinate to achieve common or individual goals
- Example: Traffic management, drone swarms

```
Multi-Agent System Structure:

    Agent 1 ◄────► Agent 2
       ▲              ▲
       │              │
       ▼              ▼
    Agent 3 ◄────► Agent 4
       
All agents share the ENVIRONMENT
```

---

# PEAS Framework

## Definition

**PEAS** is a framework to describe the task environment of an intelligent agent:

| Letter | Component | Question Answered |
|--------|-----------|-------------------|
| **P** | Performance Measure | How is success measured? |
| **E** | Environment | Where does the agent operate? |
| **A** | Actuators | How does the agent act? |
| **S** | Sensors | How does the agent perceive? |

## PEAS Examples

### Example 1: Self-Driving Car

| Component | Description |
|-----------|-------------|
| **Performance** | Safety, reaching destination, comfort, fuel efficiency, obeying traffic laws |
| **Environment** | Roads, traffic, pedestrians, weather, other vehicles |
| **Actuators** | Steering, accelerator, brake, signals, horn, display |
| **Sensors** | Cameras, LIDAR, GPS, speedometer, odometer, microphone |

### Example 2: Medical Diagnosis System

| Component | Description |
|-----------|-------------|
| **Performance** | Correct diagnosis, minimizing costs, patient health |
| **Environment** | Patients, hospital, medical records, test results |
| **Actuators** | Display recommendations, order tests, prescriptions |
| **Sensors** | Keyboard input, patient monitoring devices, lab results |

### Example 3: Chess Playing Agent

| Component | Description |
|-----------|-------------|
| **Performance** | Winning games, maximizing points |
| **Environment** | Chessboard, opponent, clock |
| **Actuators** | Move pieces (robotic arm or screen) |
| **Sensors** | Camera to see board, or digital input |

### Example 4: Robot Vacuum Cleaner

| Component | Description |
|-----------|-------------|
| **Performance** | Cleanliness, battery efficiency, coverage area |
| **Environment** | Room, floor, obstacles, furniture, dust |
| **Actuators** | Wheels, brushes, suction motor |
| **Sensors** | Bump sensors, infrared, camera, dirt detector |

---

# Task Environment (PAGE Properties)

## Definition

**PAGE** properties classify the task environment characteristics:

| Property | Options | Description |
|----------|---------|-------------|
| **P** - Partially/Fully Observable | Fully / Partially | Can agent see complete environment state? |
| **A** - Agents | Single / Multi | One agent or multiple agents? |
| **G** - Deterministic/Stochastic | Deterministic / Stochastic | Is next state predictable? |
| **E** - Episodic/Sequential | Episodic / Sequential | Are actions independent or dependent? |

### Additional Properties

| Property | Options | Description |
|----------|---------|-------------|
| **Static/Dynamic** | Static / Dynamic | Does environment change while agent thinks? |
| **Discrete/Continuous** | Discrete / Continuous | Finite or infinite states/actions? |
| **Known/Unknown** | Known / Unknown | Does agent know environment rules? |

## Detailed Explanation of Each Property

### 1. Fully Observable vs Partially Observable

```
FULLY OBSERVABLE                    PARTIALLY OBSERVABLE
┌─────────────────┐                ┌─────────────────┐
│ Agent can see   │                │ Agent has       │
│ COMPLETE state  │                │ LIMITED view    │
│ of environment  │                │ of environment  │
└─────────────────┘                └─────────────────┘
Example: Chess                     Example: Poker
(all pieces visible)               (cards hidden)
```

| Type | Definition | Examples |
|------|------------|----------|
| Fully Observable | Agent's sensors give complete state | Chess, Tic-tac-toe |
| Partially Observable | Some information hidden or noisy | Poker, Self-driving car, Medical diagnosis |

### 2. Single Agent vs Multi-Agent

| Type | Definition | Examples |
|------|------------|----------|
| Single Agent | Only one agent in environment | Crossword puzzle, Sudoku |
| Multi-Agent | Multiple agents (cooperative/competitive) | Chess (competitive), Traffic (cooperative) |

**Multi-Agent Sub-types:**
- **Cooperative:** Agents work together (drone swarm)
- **Competitive:** Agents work against each other (chess)
- **Mixed:** Some cooperation, some competition (traffic)

### 3. Deterministic vs Stochastic

| Type | Definition | Examples |
|------|------------|----------|
| Deterministic | Next state completely determined by current state + action | Chess, Puzzle |
| Stochastic | Randomness involved; next state has probability | Dice games, Weather, Traffic |

```
DETERMINISTIC                      STOCHASTIC
State + Action = Next State        State + Action = Probability(Next States)
     (certain)                           (uncertain)
```

### 4. Episodic vs Sequential

| Type | Definition | Examples |
|------|------------|----------|
| Episodic | Each action is independent; no memory needed | Image classification, Spam detection |
| Sequential | Current action affects future actions | Chess, Navigation, Planning |

```
EPISODIC                           SEQUENTIAL
┌───┐ ┌───┐ ┌───┐                 ┌───┐───▶┌───┐───▶┌───┐
│ A │ │ B │ │ C │                 │ A │    │ B │    │ C │
└───┘ └───┘ └───┘                 └───┘    └───┘    └───┘
(independent)                      (A affects B affects C)
```

### 5. Static vs Dynamic

| Type | Definition | Examples |
|------|------------|----------|
| Static | Environment doesn't change while agent thinks | Chess with clock stopped |
| Dynamic | Environment changes during deliberation | Self-driving car, Real-time games |
| Semi-dynamic | Environment static but performance score changes | Chess with running clock |

### 6. Discrete vs Continuous

| Type | Definition | Examples |
|------|------------|----------|
| Discrete | Finite number of states/actions/percepts | Chess, Digital systems |
| Continuous | Infinite states/actions | Self-driving (steering angle), Robot arm |

## PAGE Classification Table

| Environment | Observable | Agents | Deterministic | Episodic | Static | Discrete |
|-------------|------------|--------|---------------|----------|--------|----------|
| Chess | Fully | Multi | Deterministic | Sequential | Semi | Discrete |
| Poker | Partially | Multi | Stochastic | Sequential | Static | Discrete |
| Self-driving | Partially | Multi | Stochastic | Sequential | Dynamic | Continuous |
| Medical Diagnosis | Partially | Single | Stochastic | Sequential | Static | Continuous |
| Image Classification | Fully | Single | Deterministic | Episodic | Static | Continuous |
| Crossword | Fully | Single | Deterministic | Sequential | Static | Discrete |

---

# Agent Architectures

## Overview of 5 Agent Types

```
COMPLEXITY AND CAPABILITY
─────────────────────────────────────────────────────────▶

┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
│  SIMPLE  │  │  MODEL   │  │   GOAL   │  │ UTILITY  │  │ LEARNING │
│  REFLEX  │─▶│  BASED   │─▶│  BASED   │─▶│  BASED   │─▶│  AGENT   │
└──────────┘  └──────────┘  └──────────┘  └──────────┘  └──────────┘
   Basic        Memory        Goals        Best        Adapts
  reactions     of world      drive       outcomes    over time
```

---

## 1. Simple Reflex Agent

### Definition
Acts based on **current percept only**, ignoring history. Uses **condition-action rules** (if-then rules).

### Structure

```
┌─────────────────────────────────────────┐
│          SIMPLE REFLEX AGENT            │
├─────────────────────────────────────────┤
│                                         │
│  Sensors ──▶ ┌─────────────────┐        │
│              │  CONDITION-     │        │
│              │  ACTION RULES   │        │
│              │  (If-Then)      │        │
│              └────────┬────────┘        │
│                       │                 │
│                       ▼                 │
│              ┌─────────────────┐        │
│              │     ACTION      │──▶ Actuators
│              └─────────────────┘        │
│                                         │
└─────────────────────────────────────────┘
```

### Algorithm
```
function SIMPLE-REFLEX-AGENT(percept):
    rules ← a set of condition-action rules
    state ← INTERPRET-INPUT(percept)
    rule ← RULE-MATCH(state, rules)
    action ← rule.ACTION
    return action
```

### Example
- Thermostat: If temperature < 20°C → Turn ON heater
- Automatic door: If person detected → Open door

### Limitations
- No memory of past
- Fails in partially observable environments
- Cannot handle complex decisions

### Best For
- Fully observable, simple environments
- Quick reactive responses

---

## 2. Model-Based Reflex Agent

### Definition
Maintains an **internal model** of the world to handle partial observability. Tracks how world evolves and how actions affect it.

### Structure

```
┌─────────────────────────────────────────────────────┐
│            MODEL-BASED REFLEX AGENT                 │
├─────────────────────────────────────────────────────┤
│                                                     │
│  Sensors ──▶ ┌─────────────────┐                    │
│              │  How world      │                    │
│              │  evolves        │                    │
│              └────────┬────────┘                    │
│                       │                             │
│                       ▼                             │
│              ┌─────────────────┐                    │
│  State ─────▶│  INTERNAL STATE │◀──── What my       │
│              │  (World Model)  │      actions do    │
│              └────────┬────────┘                    │
│                       │                             │
│                       ▼                             │
│              ┌─────────────────┐                    │
│              │ CONDITION-ACTION│                    │
│              │     RULES       │                    │
│              └────────┬────────┘                    │
│                       │                             │
│                       ▼                             │
│                    ACTION ──────────────▶ Actuators │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Algorithm
```
function MODEL-BASED-REFLEX-AGENT(percept):
    state ← UPDATE-STATE(state, action, percept, model)
    rule ← RULE-MATCH(state, rules)
    action ← rule.ACTION
    return action
```

### Example
- Self-driving car tracking other vehicles' positions
- Robot remembering obstacle locations

### Advantages over Simple Reflex
- Handles partial observability
- Maintains history/context

### Best For
- Partially observable environments
- When past information matters

---

## 3. Goal-Based Agent

### Definition
Has explicit **goals** and chooses actions that achieve those goals. Uses **search and planning** to find action sequences.

### Structure

```
┌─────────────────────────────────────────────────────┐
│              GOAL-BASED AGENT                       │
├─────────────────────────────────────────────────────┤
│                                                     │
│  Sensors ──▶ ┌─────────────────┐                    │
│              │  STATE          │                    │
│              │  (World Model)  │                    │
│              └────────┬────────┘                    │
│                       │                             │
│         ┌─────────────┼─────────────┐               │
│         ▼             ▼             ▼               │
│  ┌───────────┐ ┌───────────┐ ┌───────────┐         │
│  │   GOALS   │ │  What if  │ │  What     │         │
│  │           │ │  I do X?  │ │  actions  │         │
│  └───────────┘ └───────────┘ │  possible │         │
│         │             │      └───────────┘         │
│         └─────────────┼─────────────┘               │
│                       ▼                             │
│              ┌─────────────────┐                    │
│              │  SEARCH/PLAN    │                    │
│              └────────┬────────┘                    │
│                       ▼                             │
│                    ACTION ──────────────▶ Actuators │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Example
- Navigation system finding route to destination
- Game-playing agent trying to win

### Advantages
- Flexible - goals can change
- Can find novel solutions through search
- Handles complex multi-step problems

### Best For
- Problems requiring planning
- When goals are clearly defined

---

## 4. Utility-Based Agent

### Definition
Uses a **utility function** to measure "happiness" or desirability of states. Chooses actions that **maximize expected utility**.

### Structure

```
┌─────────────────────────────────────────────────────┐
│             UTILITY-BASED AGENT                     │
├─────────────────────────────────────────────────────┤
│                                                     │
│  Sensors ──▶ ┌─────────────────┐                    │
│              │  STATE          │                    │
│              │  (World Model)  │                    │
│              └────────┬────────┘                    │
│                       │                             │
│         ┌─────────────┼─────────────┐               │
│         ▼             ▼             ▼               │
│  ┌───────────┐ ┌───────────┐ ┌───────────┐         │
│  │  UTILITY  │ │  What if  │ │  What     │         │
│  │ FUNCTION  │ │  I do X?  │ │  actions  │         │
│  │ (Happiness)│ └───────────┘ │  possible │         │
│  └───────────┘       │       └───────────┘         │
│         │            │              │               │
│         └────────────┼──────────────┘               │
│                      ▼                              │
│          ┌────────────────────┐                     │
│          │ MAXIMIZE EXPECTED  │                     │
│          │     UTILITY        │                     │
│          └─────────┬──────────┘                     │
│                    ▼                                │
│                 ACTION ─────────────────▶ Actuators │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### Utility Function
```
U: State → Real Number

Higher utility = More desirable state
Agent chooses action that leads to highest expected utility
```

### Example
- Self-driving car balancing speed, safety, comfort, fuel
- Investment agent maximizing returns while minimizing risk

### Advantages over Goal-Based
- Handles conflicting goals
- Deals with uncertainty (expected utility)
- Makes trade-offs between objectives

### Best For
- Multiple conflicting objectives
- Uncertain environments
- When "how well" matters, not just "whether"

---

## 5. Learning Agent

### Definition
Can **improve performance** over time through experience. Has four conceptual components.

### Structure

```
┌─────────────────────────────────────────────────────────────┐
│                    LEARNING AGENT                           │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────┐                      ┌─────────────┐       │
│  │   CRITIC    │◀──── feedback ──────│ PERFORMANCE │       │
│  │ (evaluates  │                      │  STANDARD   │       │
│  │  learning)  │                      └─────────────┘       │
│  └──────┬──────┘                                            │
│         │                                                   │
│         │ learning                                          │
│         │ goals                                             │
│         ▼                                                   │
│  ┌─────────────┐    knowledge    ┌─────────────────┐        │
│  │  LEARNING   │────────────────▶│   PERFORMANCE   │        │
│  │  ELEMENT    │                 │    ELEMENT      │        │
│  └─────────────┘                 └────────┬────────┘        │
│         ▲                                 │                 │
│         │                                 │ actions         │
│         │ experiments                     ▼                 │
│  ┌─────────────┐                     Environment            │
│  │  PROBLEM    │◀── sensors ──── Sensors                    │
│  │  GENERATOR  │                                            │
│  └─────────────┘                                            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Four Components

| Component | Role | Function |
|-----------|------|----------|
| **Performance Element** | Takes actions | The "agent" that acts in environment |
| **Learning Element** | Makes improvements | Modifies performance element |
| **Critic** | Provides feedback | Evaluates performance against standard |
| **Problem Generator** | Suggests exploration | Proposes new experiences to learn from |

### Example
- Chess program learning from past games
- Recommendation system improving with user feedback
- Self-driving car learning from driving data

### Best For
- Unknown or changing environments
- When optimal behavior is not known initially
- Long-term operation with improvement

---

## Agent Architecture Comparison

| Feature | Simple Reflex | Model-Based | Goal-Based | Utility-Based | Learning |
|---------|---------------|-------------|------------|---------------|----------|
| Memory | No | Yes | Yes | Yes | Yes |
| Goals | No | No | Yes | Yes | Yes |
| Utility | No | No | No | Yes | Yes |
| Learning | No | No | No | No | Yes |
| Complexity | Low | Medium | Medium-High | High | Highest |
| Flexibility | Low | Medium | High | Highest | Highest |
| Best Environment | Fully Observable, Simple | Partially Observable | Goal-driven | Multiple objectives | Unknown/Changing |

---

## Choosing the Right Agent Architecture

```
Decision Tree for Agent Selection:

                    Does agent need to LEARN?
                           /          \
                         Yes           No
                          |             |
                    LEARNING       Does agent need to
                      AGENT        handle TRADE-OFFS?
                                      /          \
                                    Yes           No
                                     |             |
                               UTILITY-       Does agent need
                                BASED            GOALS?
                                                /      \
                                              Yes       No
                                               |         |
                                          GOAL-     Is environment
                                          BASED     fully observable?
                                                       /      \
                                                     Yes       No
                                                      |         |
                                                  SIMPLE    MODEL-
                                                  REFLEX    BASED
```

---

# Solved Exam Questions

## Question 1: Smart Traffic Management System

> A smart traffic management system is deployed in a metropolitan city. Autonomous traffic signal controllers collect data from cameras, road sensors, and GPS-enabled vehicles to control signal timing, reduce congestion, and prioritize emergency vehicles. The system must adapt to accidents, weather changes, and fluctuating traffic density while coordinating multiple intersections.

### Part (a): Define the PEAS description

| Component | Description |
|-----------|-------------|
| **Performance Measure** | • Minimize average waiting time at signals |
|  | • Reduce traffic congestion |
|  | • Prioritize emergency vehicles (minimize their delay) |
|  | • Maximize traffic flow (vehicles per hour) |
|  | • Minimize accidents at intersections |
|  | • Reduce fuel consumption / emissions |
| **Environment** | • Roads and intersections |
|  | • Other vehicles (cars, buses, trucks, emergency vehicles) |
|  | • Pedestrians |
|  | • Weather conditions (rain, fog, snow) |
|  | • Time of day (rush hour, night) |
|  | • Accidents and road blockages |
|  | • Other traffic controllers (multi-agent) |
| **Actuators** | • Traffic signal lights (Red, Yellow, Green) |
|  | • Display boards for messages |
|  | • Communication system (to other controllers) |
|  | • Emergency vehicle preemption signals |
|  | • Variable message signs |
| **Sensors** | • Cameras (for vehicle detection, counting) |
|  | • Road sensors (inductive loops, pressure sensors) |
|  | • GPS data from vehicles |
|  | • Weather sensors |
|  | • Emergency vehicle detectors (sirens, RF signals) |
|  | • Communication from other controllers |

### Part (b): Classify the task environment using PAGE properties

| Property | Classification | Justification |
|----------|----------------|---------------|
| **Observable** | **Partially Observable** | Agent cannot see complete traffic state; some vehicles may be occluded, sensors may have noise, and weather affects visibility. Cannot know driver intentions. |
| **Agents** | **Multi-Agent** | Multiple traffic controllers at different intersections must coordinate. Other vehicles are also agents with their own goals. Both cooperative (with other controllers) and competitive (managing conflicting traffic flows). |
| **Deterministic** | **Stochastic** | Traffic flow is unpredictable. Accidents happen randomly. Weather changes unexpectedly. Driver behavior is uncertain. Cannot predict exact outcomes of actions. |
| **Episodic** | **Sequential** | Current signal decisions affect future traffic buildup. Clearing one intersection affects adjacent intersections. Past decisions impact current state. |
| **Static/Dynamic** | **Dynamic** | Environment changes continuously while the agent deliberates. Traffic flow, accidents, and weather conditions change in real-time. |
| **Discrete/Continuous** | **Continuous** | Traffic density and timing are continuous values. Signal timing can be adjusted continuously. Traffic flow rate is continuous. |

### Part (c): Recommend the most suitable agent architecture

**Recommended Architecture: Utility-Based Agent with Learning Capabilities**

**Justification:**

1. **Why Utility-Based?**
   - Multiple conflicting objectives: minimize wait time, prioritize emergencies, reduce congestion
   - Must make trade-offs (e.g., let emergency vehicle pass vs. disrupting traffic flow)
   - Utility function can weigh different objectives appropriately
   - Can handle uncertainty through expected utility calculations

2. **Why Learning Element?**
   - Traffic patterns change over time (new buildings, events)
   - Needs to adapt to changing conditions
   - Can learn optimal timings from historical data
   - Improves performance through experience

3. **Why Not Others?**
   - Simple Reflex: Cannot handle partial observability or complex decisions
   - Model-Based: No goal optimization
   - Goal-Based: Cannot handle conflicting objectives and trade-offs

```
Utility Function Example:

U = w1 × (1/avg_wait_time) 
  + w2 × (emergency_response_speed)
  + w3 × (vehicles_per_hour)
  - w4 × (congestion_level)
  - w5 × (accident_risk)

Where w1, w2, w3, w4, w5 are weights learned over time
```

---

## Question 2: Smart Healthcare Monitoring System

> A smart healthcare monitoring system uses AI agents to track patient vitals, predict emergencies, and notify doctors in real time under uncertain sensor readings.

### Part (a): Define PEAS

| Component | Description |
|-----------|-------------|
| **Performance Measure** | • Accuracy of emergency prediction |
|  | • Response time for alerts |
|  | • Minimize false alarms |
|  | • Minimize missed emergencies |
|  | • Patient survival rate |
|  | • Doctor notification speed |
| **Environment** | • Hospital wards / ICU |
|  | • Patients with varying conditions |
|  | • Medical equipment |
|  | • Doctors and nurses |
|  | • Medical records and history |
|  | • Time constraints |
| **Actuators** | • Alert systems (alarms, notifications) |
|  | • Display screens |
|  | • Automated messages to doctors |
|  | • Drug infusion pumps (if automated) |
|  | • Emergency call systems |
| **Sensors** | • Heart rate monitors |
|  | • Blood pressure sensors |
|  | • Oxygen saturation (SpO2) |
|  | • Temperature sensors |
|  | • ECG monitors |
|  | • Patient input devices |

### Part (b): Classify task environment

| Property | Classification | Justification |
|----------|----------------|---------------|
| **Observable** | **Partially Observable** | Sensor readings may be noisy or incomplete. Cannot observe internal patient state directly. Some conditions not detectable by sensors. |
| **Agents** | **Multi-Agent** | Multiple monitoring agents for different patients. Coordination with doctors (human agents). Cooperative environment. |
| **Deterministic** | **Stochastic** | Patient conditions change unpredictably. Sensor noise. Cannot predict exact outcomes. Medical emergencies are probabilistic. |
| **Episodic** | **Sequential** | Current readings depend on past treatments. Medical history affects predictions. Previous alerts affect current decisions. |
| **Static/Dynamic** | **Dynamic** | Patient vitals change continuously. Environment changes while agent processes. Real-time monitoring required. |
| **Discrete/Continuous** | **Continuous** | Vital signs are continuous values. Time is continuous. Predictions are probabilistic (continuous). |

### Part (c): Select suitable agent architecture

**Recommended: Utility-Based Learning Agent**

**Justification:**
1. **Utility-Based:** Must balance false alarms vs. missed emergencies (trade-off)
2. **Learning:** Must improve predictions based on patient history and outcomes
3. **Model-Based:** Needs internal model of normal vs. abnormal patterns
4. Handles uncertainty in sensor readings through probabilistic reasoning

---

## Question 3: Autonomous Drone System for Forest Fire Monitoring

> An autonomous drone system monitors forest fires and coordinates with other drones under changing weather and sensor noise.

### Part (a): Define PEAS

| Component | Description |
|-----------|-------------|
| **Performance Measure** | • Area coverage for fire detection |
|  | • Speed of fire detection |
|  | • Accuracy of fire location reporting |
|  | • Battery efficiency |
|  | • Coordination effectiveness |
|  | • Survival (avoid crashes) |
| **Environment** | • Forest terrain |
|  | • Weather conditions (wind, smoke, rain) |
|  | • Fire zones |
|  | • Other drones |
|  | • Obstacles (trees, power lines) |
|  | • Communication range |
| **Actuators** | • Propellers (movement) |
|  | • Camera controls |
|  | • Communication transmitter |
|  | • Alert systems |
|  | • Water/retardant release (if equipped) |
| **Sensors** | • Thermal cameras |
|  | • Smoke detectors |
|  | • GPS |
|  | • Altimeter |
|  | • Wind sensors |
|  | • Battery level indicator |
|  | • Communication receiver |

### Part (b): Task Environment Classification

| Property | Classification | Justification |
|----------|----------------|---------------|
| **Observable** | **Partially Observable** | Smoke obscures vision. Sensors have range limits. Cannot see entire forest at once. Weather affects sensor accuracy. |
| **Agents** | **Multi-Agent (Cooperative)** | Multiple drones coordinating. Must share information and divide coverage areas. Avoid collisions. |
| **Deterministic** | **Stochastic** | Wind affects flight path. Fire spread is unpredictable. Sensor noise. Weather changes. |
| **Episodic** | **Sequential** | Flight path decisions affect future battery. Fire detection affects future coordination. |
| **Static/Dynamic** | **Dynamic** | Fire spreads. Weather changes. Other drones move. Highly dynamic environment. |
| **Discrete/Continuous** | **Continuous** | Position, speed, altitude are continuous. Fire intensity is continuous. |

### Part (c): Agent Architecture

**Recommended: Utility-Based Learning Agent with Multi-Agent Coordination**

**Justification:**
1. Must balance multiple objectives (coverage vs. battery vs. safety)
2. Needs to learn optimal patrol patterns
3. Requires coordination protocol with other drones
4. Must adapt to changing weather and fire conditions

---

## Question 4: Smart Home System

> A smart home system autonomously controls lighting, heating, and security with multiple interacting agents.

### Part (a): Define PEAS

| Component | Description |
|-----------|-------------|
| **Performance Measure** | • Energy efficiency |
|  | • User comfort (temperature, lighting) |
|  | • Security level |
|  | • User satisfaction |
|  | • Cost savings |
| **Environment** | • Rooms in the house |
|  | • Weather outside |
|  | • Time of day |
|  | • Occupants and their preferences |
|  | • Appliances |
| **Actuators** | • Light switches/dimmers |
|  | • Thermostat controls |
|  | • Door locks |
|  | • Alarm systems |
|  | • Window blinds |
| **Sensors** | • Motion detectors |
|  | • Temperature sensors |
|  | • Light sensors |
|  | • Door/window sensors |
|  | • Cameras |
|  | • Voice input |

### Part (b): Task Environment

| Property | Classification | Justification |
|----------|----------------|---------------|
| **Observable** | **Partially Observable** | Cannot know exact user preferences at all times. Some rooms may lack sensors. |
| **Agents** | **Multi-Agent** | Separate agents for lighting, heating, security. Must coordinate (security affects lights). |
| **Deterministic** | **Stochastic** | User behavior unpredictable. Weather changes. |
| **Episodic** | **Sequential** | Past comfort settings affect current. Learning user patterns. |
| **Static/Dynamic** | **Dynamic** | Users move around. Weather changes. Time-sensitive. |
| **Discrete/Continuous** | **Mixed** | Lights can be discrete (on/off) or continuous (dimmer). Temperature is continuous. |

### Part (c): Agent Architecture

**Recommended: Learning Agent with Utility-Based Components**

**Justification:**
1. Must learn user preferences over time
2. Balance comfort vs. energy efficiency (utility trade-offs)
3. Coordinate multiple sub-systems
4. Adapt to changing patterns (weekday vs. weekend)

---

## Quick Reference: PEAS and PAGE

### PEAS Template

```
┌─────────────────────────────────────────────────────┐
│  PEAS for: ____________________                     │
├──────────────┬──────────────────────────────────────┤
│ Performance  │ • Goal 1                             │
│              │ • Goal 2                             │
│              │ • Goal 3                             │
├──────────────┼──────────────────────────────────────┤
│ Environment  │ • Element 1                          │
│              │ • Element 2                          │
│              │ • Element 3                          │
├──────────────┼──────────────────────────────────────┤
│ Actuators    │ • Action device 1                    │
│              │ • Action device 2                    │
├──────────────┼──────────────────────────────────────┤
│ Sensors      │ • Perception device 1                │
│              │ • Perception device 2                │
└──────────────┴──────────────────────────────────────┘
```

### PAGE Quick Decision Guide

| Question | Partially | Fully |
|----------|-----------|-------|
| Can agent see everything? | No → Partially Observable | Yes → Fully Observable |

| Question | Multi | Single |
|----------|-------|--------|
| Are there other agents? | Yes → Multi-Agent | No → Single Agent |

| Question | Stochastic | Deterministic |
|----------|------------|---------------|
| Is there randomness? | Yes → Stochastic | No → Deterministic |

| Question | Sequential | Episodic |
|----------|------------|----------|
| Do past actions matter? | Yes → Sequential | No → Episodic |

### Agent Selection Quick Guide

| Situation | Best Agent |
|-----------|------------|
| Simple environment, clear rules | Simple Reflex |
| Need to track hidden state | Model-Based |
| Clear goal to achieve | Goal-Based |
| Multiple objectives, trade-offs | Utility-Based |
| Need to improve over time | Learning Agent |
| Complex real-world problem | Learning + Utility-Based |

---

## Mock Exam Pattern (Q1 Style): PEAS + PAGE + Agent Architecture

### What the Examiner Usually Wants

1) **PEAS** (Performance, Environment, Actuators, Sensors)
2) **PAGE properties** (partially/fully observable, single/multi-agent, deterministic/stochastic, episodic/sequential)
3) **Best agent type** (simple reflex, model-based, goal-based, utility-based, learning) with 2–3 lines of justification

### Answer Skeleton (Fast)

- **PEAS:** list 3–5 items per box
- **PAGE:** pick one side for each property and give one reason
- **Architecture:** name the agent + why it fits constraints (uncertainty, hidden state, trade-offs, learning)

## Practice Questions

1. **Autonomous Warehouse Robot:** Define PEAS and classify environment for a robot that picks and places packages in a warehouse with human workers.

2. **Online Exam Proctoring System:** Define PEAS and recommend agent architecture for a system that monitors students during online exams.

3. **Agricultural Drone:** Define PEAS, classify PAGE, and select agent for a drone that monitors crop health and identifies irrigation needs.

4. **Chatbot Customer Service:** Define PEAS and justify whether it should be goal-based or utility-based.

5. **Stock Trading Agent:** Complete PEAS, PAGE classification, and explain why learning is essential.