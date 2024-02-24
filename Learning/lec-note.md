[**Lecture Note**](https://cs50.harvard.edu/ai/2024/notes/4/)

**Superivised Learning**
- Given a dataset of inputs and outputs, learn a function that maps inputs to outputs.

**Classification**
- Supervised learning task of predicting which category an input belongs to.

**K-Nearest Neighbors Classification**
- Predict the label of a data point by looking at the `k` closest labeled data points and taking a majority vote.

Example:  
X<sub>1</sub> = Humidity

X<sub>2</sub> = Temperature

h(X<sub>1</sub>, X<sub>2</sub>) = 1 if W<sub>1</sub> + W<sub>2</sub> X<sub>1</sub> + W<sub>3</sub> X<sub>2</sub> >= 0, else 0.

Weight Vector **w**: [W<sub>1</sub>, W<sub>2</sub>, W<sub>3</sub>]

Input Vector **x**: [1, X<sub>1</sub>, X<sub>2</sub>]

Dot Product: **w**.**x** = W<sub>1</sub> + W<sub>2</sub> X<sub>1</sub> + W<sub>3</sub> X<sub>2</sub>

h<sub>w</sub>(**x**) = 1 if **w**.**x** >= 0, else 0.

**Perceptron Learning Rule**
- Given dat a point (**x**, y), update the weight vector **w** as follows:

w<sub>i</sub> = w<sub>i</sub> + α(y - h<sub>w</sub>(**x**))x<sub>i</sub>

where α is the learning rate.

w<sub>i</sub> = w<sub>i</sub> + α(actual - predicted)x<sub>i</sub>

**Supoort Vector Machine (SVM)**
- Find the hyperplane that best separates the data into different classes.

**Maximum Margin Separator**
- Boundary that maximizes the distance between the closest data points of different classes.

**Regression**
- Supervised learning task of predicting a continuous value.

**Loss Function**
- Measures the difference between the predicted value and the actual value.

**0-1 Loss**
L(actual, predicted) = 0 if (actual = predicted), else 1.

**L1 Loss**
L(actual, predicted) = |actual - predicted|

**L2 Loss**
L(actual, predicted) = (actual - predicted)<sup>2</sup>

**Overfitting**
- Model performs well on the training data but poorly on the test data.

**Regularization**
- Technique to prevent overfitting by adding a penalty for complexity.
cost = loss + λ complexity  
cost = loss + λ||w||<sup>2</sup>

**Holdout Cross-Validation**
- Split the data into a training set and a test set.

**K-Fold Cross-Validation**
- Split the data into `k` subsets and train the model `k` times, each time leaving out a different subset.

sci-kit learn:
- A library for machine learning in Python.

**Reinforcement Learning**
- Gien a set of rewards and penalties, learn a policy to maximize the total reward.

**Markov Decision Process (MDP)**
- Model for decision making where an agent interacts with an environment.

**Q-Learning**
- Method for learning a function Q(s, a) that estimates the total reward of taking action a in state s.

Overview:
- Start with Q(s, a) = 0 for all s, a.
- When we taken an action and receive a reward:
	- Estimate the value of Q(s, a) based on the current reward and expected future rewards.
	- Update Q(s, a) based on the new estimate.

*Algorithm*:
- Initialize Q(s, a) = 0 for all s, a.
- Every time we take an action a in state s and receive a reward r:
	- Update Q(s, a) = Q(s, a) + α(new estimate - old estimate) = Q(s, a) + α(r + γ max<sub>a'</sub>Q(s', a') - Q(s, a))

**Greedy Decision Making**
- Always choose the action with the highest estimated reward. When in state s, choose the action a that maximizes Q(s, a).

**Exploration vs. Exploitation**
- Trade-off between choosing the action with the highest estimated reward and choosing a random action to learn more about the environment.

**Epsilon-Greedy Decision Making**
- With probability ε, choose a random action. With probability 1 - ε, choose the action with the highest estimated reward.

**Function Approximation**
- Use a function to estimate the value of Q(s, a) instead of storing a table of values.

**Unsupervised Learning**
- Given input data without labeled outputs, learn patterns and structure.

**Clustering**
- Organize data into groups based on similarity.

**K-Means Clustering**
- Partition data into `k` clusters by minimizing the sum of squared distances between data points and their cluster's centroid.