[**Lecture Note**](https://cs50.harvard.edu/ai/2024/notes/2/)

**Terminology**:
- Unconditional Probability: degree of belief in a oriosition in the absence of any other evidence. P(A)
- Conditional Probability: degree of belief in a proposition given some other evidence that has been observed. P(A|B) means the probability of A given B.  
P(A|B) = P(A ^ B) / P(B)
- Joint Probability: P(A ^ B) = P(B) * P(A|B) = P(A) * P(B|A)  
P(C|rain) = P(C ^ rain) / P(rain) = α * P(C ^ rain)
- Random Variable: a variable in probability theory with a domain of possible values it can take on.
- Probability Distribution: a function that describes the likelihood of obtaining the possible values that a random variable can take on. Example: P(Flight Delayed) = 0.2, P(Flight Not Delayed) = 0.8. This can also written as **P**(Flight) = <0.2, 0.8>
- Independence: the knowledge of one event does not affect the probability of another event. P(A|B) = P(A)


**Bayes' Theorem**:
- P(A|B) = P(B|A) * P(A) / P(B)
- P(B|A) = P(A|B) * P(B) / P(A)

**Negation**:
- P(A) = 1 - P(~A)

**Inclusion-Exclusion Principle**:
- P(A ∪ B) = P(A) + P(B) - P(A ∩ B)

**Marginalization**:
- P(A) = Σ P(A ^ B) for all B
Example: P(A) = P(A ^ B) + P(A ^ ~B)

**Conditioning**:
- P(A) = Σ P(A|B) * P(B) for all B
Example: P(A) = P(A|B) * P(B) + P(A|~B) * P(~B)

**Bayesian Networks**:
- Data structure that represents the dependencies among random variables.
- Bayesion Network contains:
	- Directed Acyclic Graph (DAG):
	- Each node represents a random variable
	- An edge from node A to node B means that A is a parent of B
	- Each node has probability distribution **P**(A|parents(A))

**Inference**:
- Query **X**: variable we want to know the probability distribution of
- Evidence **E**: observed variables for which we know the probability distribution
- Hidden variables **H**: variables for which we don't know the probability distribution
- Goal: calucalte **P**(X|E)

**Inference by Enumeration**:
- **P**(X|E) = α * **P**(X, E) = α * Σ **P**(X, E, H)

**Sampling**:
- Approximate inference algorithm
- Generate samples from the joint distribution
- Use the samples to estimate the probability distribution of the query variable

**Likelihood Weighting**:
- Approximate inference algorithm
- Start by fixing the evidence variables.
- Sample the non-evidence variables using the conditional probability in the Bayesian network.
- Weight each sample by the likelihood of the evidence variables given the non-evidence variables.

**Markov Assumption**:
- The assumption that the current state depends only on a finite fixed number of previous states.

**Markov Chain**:
- A sequence of random variables where the probability distribution of each variable depends only on the previous variable.

**Hidden Markov Model**:
- A Markov model in which the state of the system is only partially observable.

**Sensor Markov Model**:
- The assumption that the sensor reading at time t depends only on the state at time t.

Task | Definition
--- | ---
**Filtering** | Estimate the current state of the system given all the evidence so far.
**Prediction** | Estimate the future state of the system given all the evidence so far.
**Smoothing** | Estimate the past state of the system given all the evidence so far.
**Most Likely Explanation** | Find the most likely sequence of states given all the evidence so far.