### ValueIterationAgent

The `ValueIterationAgent` implements the **Value Iteration** algorithm to solve a Markov Decision Process (MDP). It computes an optimal policy by iteratively updating the values of states until convergence. This agent is particularly useful for finite-state MDPs.

---

#### Key Features

1. **Initialization**:

   - The agent is initialized with the given MDP, discount factor, and the number of iterations.
   - It stores values of states in a `util.Counter`, a dictionary-like structure with a default value of 0.

2. **Iterative Updates**:

   - The agent performs a fixed number of iterations, updating the state values using the Bellman equation:
     \[
     V(s) = \max*a \sum*{s'} P(s'|s,a) \left[ R(s, a, s') + \gamma \cdot V(s') \right]
     \]
     Where:
     - \( P(s'|s,a) \): Transition probability of reaching \( s' \) from \( s \) by taking action \( a \).
     - \( R(s, a, s') \): Reward for transitioning from \( s \) to \( s' \) with action \( a \).
     - \( \gamma \): Discount factor for future rewards.
     - \( V(s') \): Value of the next state.

3. **Policy Derivation**:
   - After value iteration, the agent derives the optimal policy by selecting the action that maximizes the Q-value for each state.

---

### Solution Walkthrough

#### 1. **Value Iteration Process**:

- For each state, the agent evaluates all possible actions and computes their Q-values using:
  \[
  Q(s, a) = \sum\_{s'} P(s'|s,a) \left[ R(s, a, s') + \gamma \cdot V(s') \right]
  \]
- The state value \( V(s) \) is updated to the maximum Q-value:
  \[
  V(s) = \max_a Q(s, a)
  \]
- Terminal states always have a value of 0, as there are no further actions to take.

#### 2. **Compute Q-Value from Values**:

- The Q-value of an action \( a \) in state \( s \) is calculated by summing over all possible transitions to the next states:
  \[
  Q(s, a) = \sum\_{s'} P(s'|s,a) \cdot \left[ R(s, a, s') + \gamma \cdot V(s') \right]
  \]

#### 3. **Policy Extraction**:

- For each state, the optimal action is determined by finding the action that maximizes the Q-value:
  \[
  \pi(s) = \arg\max_a Q(s, a)
  \]
- If no legal actions exist (e.g., in terminal states), the policy returns `None`.

#### 4. **Functions Overview**:

- `runValueIteration`: Executes the value iteration algorithm over the specified number of iterations.
- `computeQValueFromValues`: Computes the Q-value of a state-action pair using the current values of states.
- `computeActionFromValues`: Determines the best action for a state based on the current values.
- `getPolicy`: Returns the optimal policy (best action) for a given state.
- `getValue`: Retrieves the computed value of a state.

---

### Key Concepts and Methods

#### Markov Decision Process (MDP)

- A framework for decision-making where outcomes are partly random and partly under the control of an agent.
- Defined by:
  - **States (S)**: Possible configurations of the environment.
  - **Actions (A)**: Choices available to the agent.
  - **Transitions (P)**: Probabilities of reaching the next state given a state and action.
  - **Rewards (R)**: Immediate feedback for taking an action in a state.

#### Bellman Equation

- Fundamental to Value Iteration, it relates the value of a state to the values of successor states:
  \[
  V(s) = \max*a \sum*{s'} P(s'|s,a) \left[ R(s, a, s') + \gamma \cdot V(s') \right]
  \]

#### Discount Factor (\( \gamma \))

- Determines the importance of future rewards.
- \( \gamma \in [0, 1] \):
  - A value closer to 1 emphasizes long-term rewards.
  - A value closer to 0 prioritizes immediate rewards.

#### Terminal States

- States where no further actions are possible.
- The value of terminal states is set to 0.

---

### Advantages of Value Iteration

- Guarantees convergence to the optimal value function for finite-state MDPs.
- Straightforward implementation using the Bellman equation.

### Limitations

- Computationally expensive for large state spaces due to the need to evaluate all state-action pairs.
- Assumes a fully known MDP, which might not always be practical.
