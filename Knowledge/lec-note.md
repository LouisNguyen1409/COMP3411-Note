[**Lecture Note**](https://cs50.harvard.edu/ai/2024/notes/1/)

**Terminology**:

- Knowledfe-based agent: an agent that reasons by operating on internal representations of knowledge.
- Sentences: an assertion about the world in a knowledge representation language.
- Model: assignment of truth values to every propsoitional symbol.
- Knowledge base: a set of sentences known to be true by knowledge-based agent.
- Entailment (&models;): sentence `P` entails sentence `Q` if and only if `P` being true implies `Q` being true.

**Propositional Logic**:

- Propsitional symbols: P, Q, R, ...
- Connectives: and (^), or (v), not (¬), implies (->), if and only if (<->).

**Truth Table**:

- A table that shows the truth value of a compound proposition for every possible combination of truth values of its components.

Truth Table for `Not`: 
| P | ¬P |
|---|----|
| T | F  |
| F | T  |

Truth Table for `And`:
| P | Q | P ^ Q |
|---|---|-------|
| T | T | T     |
| T | F | F     |
| F | T | F     |
| F | F | F     |

Truth Table for `Or`:
| P | Q | P v Q |
|---|---|-------|
| T | T | T     |
| T | F | T     |
| F | T | T     |
| F | F | F     |

Truth Table for `Implies`:
| P | Q | P -> Q |
|---|---|--------|
| T | T | T      |
| T | F | F      |
| F | T | T      |
| F | F | T      |

Truth Table for `Iff`:
| P | Q | P <-> Q |
|---|---|---------|
| T | T | T       |
| T | F | F       |
| F | T | F       |
| F | F | T       |

**Inference**
- Inference: the process of deriving new sentences from old ones.

P: It is a Tuesday.  
Q: It is raining.  
R: Harry will go for a run.  
P, ¬Q  

KB: (P ^ ¬Q) -> R  
means that if it is a Tuesday and it is not raining, then Harry will go for a run.

P is true, ¬Q is true, so (P ^ ¬Q) is true. Therefore, R is true.  
Inference: R is true.

**Model Checking**
- Model checking: the process of verifying that a given model satisfies a given sentence.
- Time complexity: O(2^n), where n is the number of propositional symbols.

- To determine if KB |= α:
	- Enumerate all possible models.
	- If in model where KB is true, α is true, then KB |= α.
	- Otherwise, KB does not entail α.

Example:
P: It is a Tuesday.  
Q: It is raining.  
R: Harry will go for a run.  
KB: (P ^ ¬Q) -> R

| P | Q | R | (P ^ ¬Q) -> R |
|---|---|---|---------------|
| T | T | T | T             |
| T | T | F | T             |
| T | F | T | T             |
| T | F | F | F             |
| F | T | T | T             |
| F | T | F | T             |
| F | F | T | T             |
| F | F | F | T             |

KB |= R

**Knowledge Engineering**
- Knowledge engineering: the process of designing a knowledge base for a knowledge-based agent.

**Modus Ponens**
- Modus ponens: if P then Q, P, therefore Q.

**And-elimination**
- And-elimination: if P and Q, then P.

**Double Negation Elimination**
- Double negation: not not P, therefore P.

**Implication Elimination**
- Implication elimination: if P -> Q, then not P or Q.

**Bi-conditional Elimination**
- Bi-conditional elimination: if P <-> Q, then (P -> Q) ^ (Q -> P).

**De Morgan's Law**
- De Morgan's Law: not (P ^ Q) = (not P) v (not Q), not (P v Q) = (not P) ^ (not Q).

**Distributive Law**
- Distributed Law: P ^ (Q v R) = (P ^ Q) v (P ^ R), P v (Q ^ R) = (P v Q) ^ (P v R).

**Theorem Proving**
- Initial State: start with the knowledge base.
- Actions: inference rules
- Transition Model: new knowledge base after applying an inference rule.
- Goal Test: check statement we want to prove.
- Path Cost: number of steps in the proof.

**Resolution**
- Resolution:
	- P v Q, not P, therefore Q.
	- P v Q, not P v R, therefore Q v R.

**Clause**
- Clause: a disjunction of literals. (connection of `OR`)
Example: P v Q v R

**Conjunctive Normal Form (CNF)**
- Conjunctive Normal Form: a conjunction of clauses. (connection of `AND` with `OR` inside)
Example: (P v Q) ^ (¬P v R)

**Conversion to CNF**
- Eliminate biconditionals. (P <-> Q) = (P -> Q) ^ (Q -> P)
- Eliminate implications. (P -> Q) = ¬P v Q
- Move not inwards. ¬(P ^ Q) = ¬P v ¬Q, ¬(P v Q) = ¬P ^ ¬Q
- Distribute or over and. P v (Q ^ R) = (P v Q) ^ (P v R)

**Empty Clause**
- Empty clause: a clause with no literals. Example: p, ¬p, therefore empty clause.

**Inference in Resolution**
*Idea*
- To determine if KB |= α:
	- Change KB and not α is a contradiction.
	- Use resolution to show that KB and not α is unsatisfiable.
		- If so, then KB |= α.
		- Otherwise, KB does not entail α.

*Using CNF*
- To determine if KB |= α:
	- Convert KB and not α to CNF.
	- Keep applying resolution until no new clauses can be inferred.
		- If empty clause is inferred, then KB |= α.
		- Otherwise, KB does not entail α.

Example:
(A v B) ^ (¬B V C) ^ (¬C) entails A.
(A v B) ^ (¬B V C) ^ (¬C) ^ (¬A)
(¬B V C) ^ (¬C) = ¬B
(A v B) ^ (¬A) = B
¬B ^ B = empty clause

**First-Order Logic**
- First-order logic: a language for reasoning about objects and relations among them.
- Constants: a specific object.
- Predicates: a property or relation.
Example:
- Constant: Minerva, Pomona, Horace, Gilderoy, Gryffindor, Slytherin, Ravenclaw, Hufflepuff.
- Predicate: Person, House, BelongsTo
- Sentence: Person(Minerva), House(Gryffindor), BelongsTo(Minerva, Gryffindor), ¬House(Minerva)

**Universal Quantification**
- Universal quantification: for all x, P(x).
- Example: for all x, BelongsTo(x, Gryffindor) -> ¬BelongsTo(x, Slytherin)

**Existential Quantification**
- Existential quantification: there exists x, P(x).
- Example: there exists x, Person(x) ^ BelongsTo(x, Slytherin)