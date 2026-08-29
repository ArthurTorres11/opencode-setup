---
name: optimization-engineer
description: >
  Formulate, solve, validate, and improve mathematical decision optimization
  problems involving objectives, constraints, allocation, scheduling, routing,
  assignment, planning, resource allocation, parameter optimization, and
  constrained decision-making. Use only when the task involves explicit decision
  variables, an objective to minimize or maximize, and constraints, such as
  linear programming, mixed-integer optimization, constraint programming,
  nonlinear or convex optimization, combinatorial optimization, multi-objective
  optimization, heuristics, simulation optimization, or converting predictions
  and business requirements into optimal decisions. Do not invoke for code
  performance tuning, prompt optimization, model accuracy improvement, latency
  reduction, or general "make it faster" requests.
---

# Optimization Engineer

Use this skill when the problem is fundamentally about choosing the best action
under objectives and constraints.

Typical cases include:

* allocation;
* scheduling;
* routing;
* assignment;
* planning;
* resource allocation;
* capacity planning;
* portfolio allocation;
* cost minimization;
* profit maximization;
* throughput maximization;
* constrained parameter selection;
* combinatorial decisions;
* multi-objective tradeoffs;
* simulation optimization;
* decision optimization using ML predictions.

Do not invoke this skill merely because someone wants to "optimize" software,
model accuracy, prompts, latency, or code.

In this skill, optimization primarily means mathematical or algorithmic
decision optimization.

## Main Goal

Convert a decision problem into a clear optimization formulation and select the
simplest method capable of solving it reliably.

The core structure is:

```text id="i9az2q"
real-world decision
       ↓
decision variables
       ↓
objective
       ↓
constraints
       ↓
mathematical model
       ↓
solver / algorithm
       ↓
candidate solution
       ↓
validation
       ↓
decision
```

Do not start with a solver.

Start with the decision.

## Problem Definition

Before formulating equations, determine:

* What decision must be made?
* What can actually be controlled?
* What should be optimized?
* What constraints must always hold?
* What tradeoffs exist?
* What information is known?
* What information is uncertain?
* What time horizon matters?

Distinguish:

```text id="f1wyqk"
decision variables
```

from:

```text id="6fdyfn"
observed inputs
```

and:

```text id="6mz4p3"
predicted inputs
```

Do not optimize variables that cannot actually be controlled.

## Decision Variables

Decision variables represent choices the optimizer can make.

Examples:

```text id="5h3wqe"
x_i = quantity assigned to resource i

y_ij = whether job i is assigned to machine j

z_t = production level at time t
```

Variables may be:

* continuous;
* integer;
* binary;
* categorical through encoded decisions.

Define:

* meaning;
* units;
* valid domain;
* indices.

Avoid ambiguous variables.

## Parameters

Parameters represent known or estimated inputs.

Examples:

* costs;
* capacities;
* demand;
* travel time;
* prices;
* probabilities;
* forecasts;
* resource availability.

Separate parameters from decision variables explicitly.

This distinction becomes especially important when predictions from ML models
enter an optimization problem.

## Units

Track units throughout the formulation.

Examples:

```text id="50gl9o"
cost = R$
time = hours
capacity = units/hour
demand = units
```

Dimensional inconsistencies often reveal formulation errors.

Do not combine quantities with incompatible units without an explicit
conversion.

## Objective Function

Define what should be optimized.

Examples:

```text id="7ehzqi"
minimize cost

maximize profit

minimize travel distance

minimize lateness

maximize throughput
```

The objective should represent the real decision goal.

Do not use an easily measurable proxy unless its relationship to the real
objective is defensible.

## Objective Scale

Inspect the scale of objective components.

If the objective combines:

```text id="dqknqw"
cost
+
delay penalty
+
risk penalty
```

ensure the relative magnitudes reflect intended tradeoffs.

Arbitrary weights can produce arbitrary decisions.

## Hard Constraints

Hard constraints must never be violated.

Examples:

* physical limits;
* legal requirements;
* capacity;
* inventory balance;
* resource availability;
* safety requirements;
* logical dependencies.

Represent them explicitly.

## Soft Constraints

Soft constraints represent preferences that may be violated at a cost.

Typical formulation:

```text id="kic4wn"
constraint
   ↓
slack variable
   ↓
penalty in objective
```

Use soft constraints when strict feasibility is less important than allowing a
controlled violation.

Do not silently convert a hard requirement into a soft constraint.

## Feasibility

Before optimizing performance, determine whether a feasible solution exists.

Ask:

```text id="mhzw2a"
Does any solution satisfy all hard constraints?
```

An infeasible optimization model cannot be repaired by choosing a better
objective.

When infeasible:

1. identify conflicting constraints;
2. inspect data;
3. inspect bounds;
4. inspect units;
5. inspect business assumptions;
6. consider whether some constraints should actually be soft.

Do not immediately relax constraints until the source of infeasibility is
understood.

## Constraint Validation

For each important constraint, understand:

* business meaning;
* mathematical expression;
* units;
* expected binding behavior;
* edge cases.

A solver returning "optimal" only means optimal relative to the formulation.

It does not prove the formulation represents reality.

## Problem Classification

Before choosing an algorithm, classify the problem.

Possible classes include:

* linear programming;
* mixed-integer linear programming;
* quadratic programming;
* nonlinear programming;
* convex optimization;
* constraint programming;
* combinatorial optimization;
* network optimization;
* stochastic optimization;
* robust optimization;
* simulation optimization;
* multi-objective optimization.

Problem structure often determines which methods are appropriate.

## Linear Programming

Use linear programming when:

* variables can be continuous;
* objective is linear;
* constraints are linear.

General form:

```text id="y5skld"
minimize    cᵀx

subject to  Ax ≤ b
            Aeq x = beq
            l ≤ x ≤ u
```

LP problems can often be solved efficiently even at substantial scale.

Do not introduce integer variables unless the decision genuinely requires
discreteness.

## Mixed-Integer Linear Programming

Use MILP when some decisions are discrete.

Examples:

```text id="k00w5n"
open facility or not
assign job or not
select supplier or not
number of vehicles
```

Binary variables are especially useful for logical decisions.

MILP is powerful but computational complexity can grow rapidly.

Avoid unnecessary binary variables.

## Logical Constraints

Binary variables can encode logical relationships.

Examples:

```text id="wj3d01"
if A then B

either X or Y

at most one

exactly one

activate resource only if selected
```

Formulate these carefully.

Big-M formulations should use the smallest defensible value of M.

Excessively large Big-M constants can cause numerical problems and weak
relaxations.

Prefer indicator constraints when the solver supports them and they improve the
formulation.

## Constraint Programming

Constraint programming can be effective for discrete scheduling and logical
problems.

Typical cases include:

* employee scheduling;
* timetabling;
* job-shop scheduling;
* assignment;
* complex logical constraints.

Constraint programming may be preferable to forcing every discrete problem into
MILP.

## Nonlinear Optimization

Use nonlinear optimization when the objective or constraints contain nonlinear
relationships.

Examples:

```text id="4ejr4g"
x²
log(x)
exp(x)
x × y
nonlinear physical relationships
```

Determine whether the problem is:

* convex;
* non-convex;
* differentiable;
* constrained;
* continuous;
* mixed discrete-continuous.

These properties strongly influence solver choice and guarantees.

## Convex Optimization

When the problem is convex, local optima are also global optima under standard
conditions.

Recognizing convexity can provide:

* stronger guarantees;
* more reliable algorithms;
* efficient solvers.

Do not treat all nonlinear problems as equally difficult.

## Non-Convex Optimization

Non-convex problems may contain multiple local optima.

Possible approaches include:

* multi-start optimization;
* global optimization;
* evolutionary algorithms;
* heuristics;
* problem-specific decomposition.

Do not claim global optimality unless the algorithm and conditions support that
claim.

## Quadratic Optimization

Quadratic objectives arise in problems such as:

* portfolio optimization;
* least-squares objectives;
* risk minimization;
* control problems.

Determine whether the quadratic form is convex.

Convex quadratic programs can often be solved efficiently.

## Network Optimization

Recognize network structure when present.

Common problems include:

* shortest path;
* maximum flow;
* minimum-cost flow;
* transportation;
* assignment.

Specialized algorithms may be substantially more efficient than generic
optimization formulations.

## Routing

Routing problems may involve:

* shortest path;
* traveling salesperson;
* vehicle routing;
* capacities;
* time windows;
* multiple depots;
* pickup and delivery.

Start with the simplest routing variant matching the actual requirements.

Do not immediately formulate a full vehicle-routing problem when shortest path
or assignment is sufficient.

## Scheduling

Scheduling problems may involve:

* jobs;
* machines;
* workers;
* start times;
* durations;
* precedence;
* deadlines;
* resource conflicts.

Define explicitly:

```text id="yfw9s3"
what is scheduled
which resources exist
when activities may occur
which activities depend on others
```

Scheduling models can become combinatorial quickly.

Use problem structure aggressively.

## Assignment

Assignment problems map entities to resources.

Examples:

```text id="l2fz4o"
worker → shift
job → machine
customer → representative
task → server
```

Define:

* eligibility;
* capacity;
* assignment limits;
* cost or utility.

Use specialized assignment or flow algorithms when applicable.

## Resource Allocation

Resource allocation problems distribute limited resources across competing
uses.

Examples:

* budget;
* compute;
* workforce;
* inventory;
* production capacity.

Model both:

```text id="6uy87e"
resource limits
```

and:

```text id="yz2mrn"
value produced by allocation
```

## Multi-Period Optimization

When decisions interact over time, define:

* time index;
* state transitions;
* inventory balance;
* carry-over;
* future commitments.

Example:

```text id="5qwg5g"
inventory(t+1)
=
inventory(t)
+
production(t)
-
demand(t)
```

Do not optimize each period independently when decisions have future
consequences.

## Multi-Objective Optimization

Real problems often have competing objectives.

Examples:

```text id="y29wmu"
minimize cost
maximize service level
minimize risk
```

Possible approaches include:

* weighted objectives;
* lexicographic optimization;
* epsilon constraints;
* Pareto frontier analysis.

Do not collapse multiple objectives into arbitrary weights without explaining
the tradeoff.

## Pareto Analysis

When no single solution dominates all objectives, inspect the Pareto frontier.

A solution is Pareto-dominated when another solution is at least as good in all
objectives and better in at least one.

Use Pareto analysis when stakeholders need to choose between meaningful
tradeoffs.

## Lexicographic Optimization

When objectives have strict priorities, optimize sequentially.

Example:

```text id="6j3k57"
1. satisfy service level
2. minimize cost
3. minimize operational complexity
```

This may represent business priorities better than arbitrary weighted sums.

## Uncertainty

Optimization inputs are often uncertain.

Examples:

* demand;
* prices;
* travel times;
* forecasts;
* probabilities;
* resource availability.

A deterministic optimum based on uncertain inputs may be fragile.

Ask:

```text id="x4ncb7"
What happens if these inputs are wrong?
```

## Sensitivity Analysis

Evaluate how the solution changes when important parameters change.

Possible analyses include:

* objective sensitivity;
* capacity changes;
* demand changes;
* cost changes;
* constraint changes.

A slightly worse but stable solution may be preferable to a fragile optimum.

## Scenario Analysis

Evaluate the solution under multiple plausible scenarios.

Example:

```text id="z5y4yv"
low demand
expected demand
high demand
```

Compare:

* feasibility;
* objective;
* decisions;
* constraint violations.

Scenario analysis is often a practical first step before introducing more
complex uncertainty methods.

## Robust Optimization

Robust optimization seeks solutions that remain feasible or effective across a
defined uncertainty set.

Use it when uncertainty is important and distributions are unreliable or
unavailable.

Robust solutions may sacrifice nominal performance for greater stability.

Do not make uncertainty sets arbitrarily conservative.

## Stochastic Optimization

Stochastic optimization explicitly models uncertain variables using probability
distributions or scenarios.

Possible forms include:

* expected-value optimization;
* chance constraints;
* two-stage stochastic programming;
* multi-stage stochastic programming.

Use when probabilistic uncertainty materially affects decisions and sufficient
information exists to model it.

## Simulation Optimization

When system behavior cannot be represented conveniently with analytical
equations, simulation may evaluate candidate decisions.

Structure:

```text id="4dfc4a"
candidate decision
       ↓
simulation
       ↓
outcome
       ↓
search / optimizer
```

Use when:

* dynamics are complex;
* queues matter;
* stochastic processes dominate;
* objective evaluation is a black box.

Simulation noise must be considered when comparing candidates.

## Heuristics

Heuristics may be appropriate when:

* exact optimization is too slow;
* the problem is extremely large;
* approximate solutions are acceptable;
* good domain-specific strategies exist.

Examples:

* greedy methods;
* constructive heuristics;
* local search;
* neighborhood search.

Always compare heuristics against a meaningful baseline.

When possible, compare against exact solutions on smaller instances.

## Metaheuristics

Possible methods include:

* genetic algorithms;
* simulated annealing;
* tabu search;
* particle swarm optimization;
* differential evolution.

Use metaheuristics when problem structure justifies them.

Do not choose them merely because they are easy to apply to arbitrary objective
functions.

Prefer exploiting mathematical structure when possible.

## Greedy Algorithms

Greedy methods can be excellent when:

* they are optimal for the problem structure;
* approximate solutions are acceptable;
* speed matters.

Do not assume greedy solutions are globally optimal without proof.

## Local Search

Local search improves a candidate solution through neighborhood moves.

Important choices include:

* initial solution;
* neighborhood;
* acceptance rule;
* stopping condition.

Use multiple starts when local optima are a concern.

## Decomposition

Large optimization problems may benefit from decomposition.

Examples include:

* problem partitioning;
* Benders decomposition;
* column generation;
* Lagrangian relaxation;
* rolling-horizon optimization.

Use decomposition only when scale or structure requires it.

Do not introduce advanced decomposition before establishing a correct baseline
formulation.

## Scaling

Optimization difficulty depends on more than row count.

Track:

* number of variables;
* number of integer variables;
* number of constraints;
* sparsity;
* symmetry;
* time periods;
* scenario count.

Large numbers of binary variables can dominate computational complexity.

## Numerical Stability

Watch for:

* coefficients differing by many orders of magnitude;
* excessively large Big-M values;
* near-zero tolerances;
* badly scaled objectives.

Numerical problems may produce unstable or misleading solver behavior.

Scale formulations sensibly.

## Solver Selection

Choose the solver after understanding the formulation.

Consider:

* problem class;
* scale;
* integer variables;
* nonlinear structure;
* optimality requirements;
* runtime;
* licensing;
* deployment environment.

Possible Python tools include:

* `scipy.optimize`;
* `scipy.optimize.milp`;
* `PuLP`;
* `OR-Tools`;
* `Pyomo`;
* `CVXPY`.

Commercial solvers such as Gurobi or CPLEX may be appropriate when available and
justified.

Use `context7` when current solver APIs or capabilities need verification.

## Solver Status

Never inspect only the returned variable values.

Check solver status explicitly.

Possible outcomes include:

* optimal;
* feasible;
* infeasible;
* unbounded;
* time limit;
* iteration limit;
* numerical failure;
* unknown.

A returned solution is not necessarily optimal.

## Optimality Gap

For mixed-integer optimization, inspect the optimality gap when the solver stops
before proving optimality.

A feasible solution with a small gap may be operationally sufficient.

Report the gap rather than claiming exact optimality.

## Time Limits

Production optimization often needs explicit time budgets.

Define:

```text id="1w0aw8"
solution quality
vs
solve time
```

A near-optimal solution in seconds may be more useful than a proven optimum
after hours.

## Warm Starts

When solving similar problems repeatedly, previous solutions may provide useful
warm starts.

Use when supported by the solver and formulation.

Do not assume warm starts always improve runtime.

## Symmetry

Equivalent solutions can make combinatorial optimization unnecessarily hard.

When relevant, add symmetry-breaking constraints or reformulate the model.

Do not add complex symmetry handling without evidence that it matters.

## ML + Optimization

Machine learning often predicts what will happen.

Optimization decides what to do.

Typical architecture:

```text id="txmbqb"
data
  ↓
ML model
  ↓
prediction
  ↓
optimization
  ↓
decision
```

Examples:

* demand prediction → inventory optimization;
* probability prediction → resource allocation;
* travel-time prediction → routing;
* risk prediction → portfolio decisions.

Do not confuse predictive accuracy with decision quality.

A more accurate model does not automatically produce better optimized
decisions.

## Prediction Uncertainty

When optimization depends on ML predictions, consider uncertainty in those
predictions.

Potential approaches include:

* scenario analysis;
* prediction intervals;
* robust optimization;
* stochastic optimization.

Do not treat uncertain predictions as perfectly known parameters when errors
could materially change decisions.

## Decision-Focused Evaluation

When predictions feed an optimizer, evaluate downstream decisions as well as
predictive metrics.

Example:

```text id="68rnnj"
Model A
MAE = 10

Model B
MAE = 9
```

does not guarantee:

```text id="ll7t0c"
Optimization using B
produces better decisions.
```

Measure the actual objective or decision outcome when practical.

## Constraints From Business Rules

Translate business rules carefully.

A statement such as:

> Each employee should preferably work no more than five consecutive days.

requires determining whether this is:

```text id="f2opbl"
hard constraint
```

or:

```text id="s4dfoq"
soft preference
```

Do not encode ambiguous natural-language requirements without identifying their
meaning.

## Constraint Traceability

For important systems, map constraints back to their source.

Example:

```text id="6lpnv6"
constraint_17
→ workforce policy section 4.2
```

This helps:

* validation;
* auditing;
* debugging;
* future modifications.

## Validation

Optimization validation should occur at multiple levels.

### Mathematical Validation

Check:

* equations;
* signs;
* bounds;
* units;
* variable domains.

### Feasibility Validation

Verify every hard constraint.

### Business Validation

Check whether solutions make practical sense.

### Comparative Validation

Compare against:

* current process;
* heuristic;
* historical decisions;
* simple baseline.

### Stress Validation

Test unusual but plausible scenarios.

An optimizer passing solver checks is not sufficient evidence of correctness.

## Small Synthetic Cases

Before trusting a large formulation, test small cases where the expected answer
is obvious.

Example:

```text id="ktob4v"
2 jobs
2 workers
simple costs
known optimal assignment
```

These tests can reveal formulation mistakes much faster than debugging a large
production instance.

## Edge Cases

Test when relevant:

* zero demand;
* maximum demand;
* no available resources;
* exactly sufficient capacity;
* impossible constraints;
* one resource;
* one decision;
* tied solutions;
* missing parameter values.

Optimization code often fails at boundaries rather than typical cases.

## Infeasibility Debugging

When the model is infeasible:

1. confirm input validity;
2. inspect variable bounds;
3. inspect units;
4. isolate constraint groups;
5. identify conflicting requirements;
6. use solver infeasibility tools when available.

Some solvers can compute an IIS or similar conflict representation.

Use these tools when supported.

Do not randomly delete constraints until the model solves.

## Unbounded Problems

An unbounded model often indicates:

* missing bounds;
* missing constraints;
* incorrect signs;
* incomplete resource limits.

Investigate the formulation before changing the solver.

## Baselines

Always compare optimization approaches against meaningful alternatives.

Possible baselines include:

* current business rule;
* greedy heuristic;
* historical decisions;
* previous optimizer;
* simple allocation strategy.

Optimization value should be measured relative to what would otherwise happen.

## Experimentation

When comparing optimization methods, control:

* problem instances;
* compute budget;
* time limits;
* hardware when relevant;
* stopping criteria.

Measure:

* objective value;
* feasibility;
* optimality gap;
* runtime;
* stability.

Use `experiment-designer` for rigorous comparative experiments.

## Statistical Evaluation

When optimization uses stochastic simulations or noisy objectives, account for
uncertainty.

Use:

* repeated simulations;
* confidence intervals;
* paired comparisons.

Use `statistician` when deeper uncertainty analysis is needed.

## Performance

Profile before optimizing implementation.

Potential bottlenecks include:

* model construction;
* solver runtime;
* data preparation;
* scenario generation;
* result extraction.

Do not assume the solver itself is always the bottleneck.

## Implementation Architecture

Separate when practical:

```text id="5jndt1"
input preparation
      ↓
model formulation
      ↓
solver execution
      ↓
solution extraction
      ↓
validation
      ↓
business output
```

Avoid embedding all optimization logic inside one function or notebook cell.

## Reproducibility

Track when relevant:

* input dataset;
* parameter values;
* solver;
* solver version;
* solver settings;
* time limit;
* random seed;
* formulation version.

This is especially important when solver behavior or heuristic search is
nondeterministic.

## Explainability

Optimization outputs should be understandable to decision makers.

Useful explanations include:

* objective value;
* selected decisions;
* binding constraints;
* resource utilization;
* tradeoffs;
* comparison against baseline.

Avoid exposing raw solver internals when they do not help the decision.

## Shadow Mode

For high-impact optimization systems, consider running recommendations without
executing them initially.

Compare:

```text id="jj9rmm"
optimizer recommendation
vs
actual decision
vs
observed outcome
```

This can reveal formulation errors before automated decision-making.

## Common Failure Modes

### Solver-First Thinking

Choosing a solver before understanding the problem.

### Wrong Objective

Optimizing a measurable proxy instead of the actual goal.

### Missing Constraints

The optimizer finds mathematically valid but impossible solutions.

### Over-Constraining

Valid solutions are accidentally excluded.

### Arbitrary Weights

Multi-objective tradeoffs do not reflect real priorities.

### Ignoring Units

The formulation mixes incompatible quantities.

### Fragile Optimum

Small parameter changes produce radically different decisions.

### False Optimality Claims

A time-limited feasible solution is reported as globally optimal.

### Ignoring Uncertainty

Predicted inputs are treated as exact.

### Overengineering

Advanced algorithms are introduced when a simple formulation is sufficient.

## When Other Skills Should Be Combined

Examples:

* `optimization-engineer` + `python-engineer` for implementation;
* `optimization-engineer` + `experiment-designer` for algorithm comparison;
* `optimization-engineer` + `statistician` for stochastic uncertainty;
* `optimization-engineer` + `ml-engineer` for prediction-to-decision systems;
* `optimization-engineer` + `model-evaluator` when predictive models feed the
  optimizer;
* `optimization-engineer` + `debugging` for infeasible or incorrect models;
* `optimization-engineer` + `eda-specialist` for understanding input data.

Do not invoke additional skills unless they materially improve the task.

## Required Output

For substantial optimization work, report:

### Decision Problem

What decision is being optimized.

### Decision Variables

Variables, domains, and units.

### Parameters

Known or estimated inputs.

### Objective

What is minimized or maximized.

### Constraints

Hard and soft requirements.

### Problem Class

LP, MILP, CP, nonlinear, convex, combinatorial, stochastic, or another relevant
class.

### Method

Recommended solver or algorithm and why.

### Validation

How mathematical correctness, feasibility, and business validity should be
verified.

### Baseline

What the optimized solution should be compared against.

### Risks

Uncertainty, scalability, formulation assumptions, numerical issues, or
operational limitations.

### Implementation

A practical implementation approach when requested.

For simple optimization questions, keep output proportional to the request.

## Rules

* Formulate the decision before writing solver code.
* Separate decision variables from parameters.
* Define units.
* Separate hard constraints from soft constraints.
* Check feasibility before optimizing quality.
* Validate the formulation, not only solver execution.
* Do not claim global optimality without evidence.
* Inspect solver status explicitly.
* Report optimality gaps when relevant.
* Avoid unnecessarily large Big-M constants.
* Prefer mathematical structure over generic heuristics.
* Use heuristics when exact optimization is impractical, not by default.
* Start with the simplest formulation capable of representing the problem.
* Compare against a meaningful baseline.
* Test small synthetic cases.
* Consider uncertainty when inputs are uncertain.
* Evaluate downstream decision quality when ML predictions feed optimization.
* Prefer robust decisions over fragile nominal optima when the application
  requires it.
* Do not automate high-impact decisions without appropriate validation and
  safeguards.
