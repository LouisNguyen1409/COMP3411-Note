[**Lecture Note**](https://cs50.harvard.edu/ai/2024/notes/3/)

**Terminology**:
- **Optimization**: Choosing the best solution from some set of available options.
- **Local Search**: search algorithm that maintains a single node and searches by moving to a neighboring node.

**Hill Climbing**:
- **Hill Climbing**: A local search algorithm that starts with some initial solution and iteratively makes small changes to it, with the goal of eventually reaching a solution that can’t be improved upon by making small changes.

*Algorithm*:
```python
def hill_climbing(problem):
	current = problem.initial_state
	while True:
		neighbors = current.neighbors()
		if not neighbors:
			break
		neighbor = argmax(neighbors, key=problem.value)
		if problem.value(neighbor) <= problem.value(current):
			break
		current = neighbor
	return current
```

**Hill Climbing Variants**:
Variant | Definition
--- | ---
Steepest Ascent Hill Climbing | Always choose the neighbor that has the highest value.
Stochastic Hill Climbing | Randomly choose a neighbor to move to, with the probability of choosing a neighbor being proportional to its value.
First-Choice Hill Climbing | Randomly choose a neighbor to move to, and then move to it only if it has a higher value than the current node.
Random-Restart Hill Climbing | Randomly generate a new initial state and then run hill climbing from there, repeating this process until some stopping condition is met.
Local Beam Search | Maintain k states at each iteration, where k is a parameter, and then move to the k best states from the combined set of neighbors of all k states.

**Simulated Annealing**:
- Early on, high temperatures allow the algorithm to accept worse solutions, which can help it escape local optima.
- As the temperature decreases, the algorithm becomes less likely to accept worse solutions, which can help it converge to a global optimum.

*Algorithm*:
```python
def simulated_annealing(problem, maximum):
	current = problem.initial_state
	for t in range(maximum):
		T = temperature(maximum, t)
		if T == 0:
			return current
		neighbor = random.choice(current.neighbors())
		delta_e = problem.value(neighbor) - problem.value(current)
		if delta_e > 0 or probability(math.exp(delta_e / T)):
			current = neighbor
	return current
```

**Linear Programming**:
- Algorithm: Simplex Algorithm, Interior-Point Method
- Minimuze a cost function c<sub>1</sub>x<sub>1</sub> + c<sub>2</sub>x<sub>2</sub> + ... + c<sub>n</sub>x<sub>n</sub>.
- With constraints of form a<sub>1</sub>x<sub>1</sub> + a<sub>2</sub>x<sub>2</sub> + ... + a<sub>n</sub>x<sub>n</sub> ≤ b or a<sub>1</sub>x<sub>1</sub> + a<sub>2</sub>x<sub>2</sub> + ... + a<sub>n</sub>x<sub>n</sub> = b.
- With bounds for each variable, l<sub>i</sub> ≤ x<sub>i</sub> ≤ u<sub>i</sub>.

Example:
- Two machines X and. X costs $50 per hour to run and Y costs $80 per hour to run. Goal is to minimize the cost.  
Constraints:
	- X requires 5 units of labor per hour and Y requires 2 units of labor per hour. Total labor available is 20 units.
	- X produces 10 units of product per hour and Y produces 12 units of product per hour. Total product required is 90 units.

Solve:
- Cost function: 50x + 80y
- Constraints: 5x + 2y ≤ 20, 10x + 12y ≥ 90

**Constraint Satisfaction Problems**:
- Set of variables {X<sub>1</sub>, X<sub>2</sub>, ..., X<sub>n</sub>}.
- Set of domains {D<sub>1</sub>, D<sub>2</sub>, ..., D<sub>n</sub>}.
- Set of constraints {C<sub>1</sub>, C<sub>2</sub>, ..., C<sub>n</sub>}.

**Hard and Soft Constraints**:
- **Hard Constraints**: Must be satisfied.
- **Soft Constraints**: Should be satisfied, but not required.

**Unary Constraints**:
- Constraints that involve a single variable. Example: X<sub>1</sub> ≠ 3.

**Binary Constraints**:
- Constraints that involve two variables. Example: X<sub>1</sub> ≠ X<sub>2</sub>.

**Node Consistency**:
- A variable is node consistent if every value in its domain satisfies the variable’s unary constraints.

**Arc Consistency**:
- A variable is arc consistent with respect to another variable if every value in its domain satisfies the variable’s binary constraints with respect to the other variable.
- To make a variable arc consistent with respect to another variable, we can remove values from its domain that don’t satisfy the binary constraints with respect to the other variable.

*Algorithm*:
```python
def Revise(csp, X, Y):
	revised = False
	for x in X.domain:
		if all(not csp.constraints(X, x, Y, y) for y in Y.domain):
			X.domain.remove(x)
			revised = True
	return revised
```

- AC-3: An algorithm that makes all variables arc consistent with respect to each other.

*Algorithm*:
```python
def AC3(csp):
	queue = [(X, Y) for X in csp.variables for Y in csp.neighbors(X)]
	while queue:
		(X, Y) = queue.pop()
		if Revise(csp, X, Y):
			if not X.domain:
				return False
			for Z in csp.neighbors(X) - {Y}:
				queue.append((Z, X))
	return True
```

**Backtracking Search**:
- A search algorithm that tries to build a solution incrementally, one piece at a time, while backtracking when it reaches a dead end.

*Algorithm*:
```python
def backtracking_search(assignments, csp):
	if len(assignments) == len(csp.variables):
		return assignments
	var = select_unassigned_variable(assignments, csp)
	for value in order_domain_values(var, assignments, csp):
		if is_consistent(var, value, assignments, csp):
			assignments[var] = value
			result = backtracking_search(assignments, csp)
			if result:
				return result
			del assignments[var]
	return None
```

**Maintaining Arc Consistency**:
- A search algorithm that enforces arc consistency after each new assignment.

Example: When we make a new assignment, we can run AC-3 to make all variables arc consistent with respect to each other.

*Algorithm*:
```python
def backtracking_search(assignments, csp):
	if len(assignments) == len(csp.variables):
		return assignments
	var = select_unassigned_variable(assignments, csp)
	for value in order_domain_values(var, assignments, csp):
		if is_consistent(var, value, assignments, csp):
			assignments[var] = value
			inferences = Inference(csp, var, value)
			if inferences:
				assignments.update(inferences)
				result = backtracking_search(assignments, csp)
				if result:
					return result
				for variable in inferences:
					del assignments[variable]
	return None
```
** Minimum Remaining Values (MRV) Heuristic**:
- A variable ordering heuristic that chooses the variable with the fewest legal values.

*Algorithm*:
```python
def select_unassigned_variable(assignments, csp):
	return min([var for var in csp.variables if var not in assignments], key=lambda var: len(csp.domains[var]))
```

** Highest Degree Heuristic**:
- A variable ordering heuristic that chooses the variable with the most constraints on remaining variables.

*Algorithm*:
```python
def select_unassigned_variable(assignments, csp):
	return max([var for var in csp.variables if var not in assignments], key=lambda var: len(csp.neighbors(var)))
```

** Least Constraining Value Heuristic**:
- A value ordering heuristic that chooses the value that rules out the fewest values for the neighboring variables.

*Algorithm*:
```python
def order_domain_values(var, assignments, csp):
	return sorted(csp.domains[var], key=lambda value: count_conflicts(var, value, assignments, csp))
```
