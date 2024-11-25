# Q-Learning Agents Documentation

This document provides a detailed explanation of the `qlearningAgents.py` module, which is part of the Pacman AI projects by UC Berkeley. It includes implementations of Q-Learning and Approximate Q-Learning agents, with explanations of their functionality, algorithms, and solutions.

---

## Table of Contents

1. [Overview](#overview)
2. [Q-Learning Algorithm Explained](#q-learning-algorithm-explained)
3. [Classes](#classes)
   - [QLearningAgent](#qlearningagent)
   - [PacmanQAgent](#pacmanqagent)
   - [ApproximateQAgent](#approximateqagent)
4. [Key Concepts and Methods](#key-concepts-and-methods)
5. [Licensing and Attribution](#licensing-and-attribution)

---

## Overview

Reinforcement Learning is a branch of AI where agents learn optimal behaviors by interacting with an environment. The `qlearningAgents.py` module implements:

- **Q-Learning**, a model-free algorithm that learns the value of actions in specific states (Q-values).
- **Approximate Q-Learning**, which uses feature-based approximations to generalize across states.

The agents are designed to operate in a grid-world environment like Pacman, where they decide actions based on learning from rewards and punishments.

---

## Q-Learning Algorithm Explained

Q-Learning is an off-policy learning algorithm that updates Q-values (state-action values) iteratively based on rewards and estimates of future rewards. Here's how it works:

### Problem Statement

The agent interacts with an environment by:

1. Observing the current state \( s \).
2. Taking an action \( a \) from the set of **legal actions**.
3. Receiving a reward \( r \) and transitioning to a new state \( s' \).

The goal is to learn an **optimal policy** \( \pi(s) \), which tells the agent the best action to take in any state, maximizing cumulative rewards.

### Core Formula

To learn the optimal policy, the Q-learning update rule is applied:
\[
Q(s, a) \leftarrow (1 - \alpha) \cdot Q(s, a) + \alpha \cdot \left[ r + \gamma \cdot \max_{a'} Q(s', a') \right]
\]

- **\( Q(s, a) \)**: Current Q-value for state \( s \) and action \( a \).
- **\( \alpha \)**: Learning rate (controls how quickly the agent updates its beliefs).
- **\( \gamma \)**: Discount factor (weighs future rewards against immediate rewards).
- **\( r \)**: Reward obtained after taking action \( a \).
- **\( \max\_{a'} Q(s', a') \)**: Maximum Q-value of the next state \( s' \).

### Exploration vs. Exploitation

- **Exploration**: Choose random actions to discover new possibilities.
- **Exploitation**: Choose the best-known action to maximize rewards.

The balance is controlled by the **epsilon-greedy strategy**, where:

- With probability \( \epsilon \), take a random action (exploration).
- Otherwise, choose the action with the highest Q-value (exploitation).

---

## Classes

### QLearningAgent

The `QLearningAgent` is the foundational implementation of the Q-learning algorithm. It learns Q-values for state-action pairs by interacting with the environment.

#### Key Responsibilities

1. Maintain Q-values for state-action pairs.
2. Decide actions based on learned Q-values and exploration probability.
3. Update Q-values using the Q-learning formula.

#### Key Methods

- **`getQValue(state, action)`**: Returns the Q-value of a specific state-action pair.
- **`computeValueFromQValues(state)`**: Calculates the maximum Q-value for all legal actions in a state.
- **`computeActionFromQValues(state)`**: Finds the best action (one with the highest Q-value) in a state.
- **`getAction(state)`**: Decides which action to take using an epsilon-greedy strategy.
- **`update(state, action, nextState, reward)`**: Updates Q-values based on observed rewards and estimated future values.

#### Solution Walkthrough

1. **Initialization**: The agent starts with all Q-values set to zero.
2. **Action Selection**: For a given state, it either:
   - Explores: Picks a random action with probability \( \epsilon \).
   - Exploits: Chooses the action with the highest Q-value.
3. **Q-value Update**: After observing the reward \( r \) and the new state \( s' \), the Q-value for \( (s, a) \) is updated using the Q-learning formula.

---

### PacmanQAgent

The `PacmanQAgent` is a subclass of `QLearningAgent`, specifically tuned for the Pacman environment. It uses predefined parameters suitable for the game and tracks Pacman’s actions for smoother integration with the environment.

#### Key Parameters

- **`epsilon`**: Controls exploration rate (default: 0.05).
- **`alpha`**: Learning rate (default: 0.2).
- **`gamma`**: Discount factor (default: 0.8).
- **`numTraining`**: Number of episodes dedicated to training (default: 0).

#### Example

To customize exploration rate:

```bash
python pacman.py -p PacmanQLearningAgent -a epsilon=0.1
```

### ApproximateQAgent

The `ApproximateQAgent` is an advanced version of the Q-learning agent, designed to handle environments with large or continuous state spaces. Instead of explicitly storing Q-values for every state-action pair, it uses feature-based representations to approximate the Q-values.

#### Key Features

1. **Feature Extraction**:
   The agent uses a feature extractor (e.g., `IdentityExtractor`) to compute feature vectors that describe a state-action pair. Features are numeric values capturing relevant aspects of the environment.

2. **Weight Updates**:
   The agent maintains a weight for each feature. These weights are adjusted during learning to minimize the error between predicted and observed Q-values.

#### Solution Walkthrough

1. **Q-Value Computation**:
   The Q-value for a state-action pair is calculated as:
   \[
   Q(s, a) = \sum\_{f} w[f] \cdot \text{features}[f]
   \]
   Where:

   - \( w[f] \): Weight associated with feature \( f \).
   - \( \text{features}[f] \): Value of feature \( f \) for the state-action pair.

2. **Weight Update**:
   During learning, the weights are updated based on the difference between the observed and predicted Q-values. The update rule is:
   \[
   w[f] \leftarrow w[f] + \alpha \cdot \text{difference} \cdot \text{features}[f]
   \]
   Where:

   - \( \alpha \): Learning rate.
   - \( \text{difference} = \left( r + \gamma \cdot \max\_{a'} Q(s', a') \right) - Q(s, a) \).
   - \( r \): Reward received after taking action \( a \).
   - \( \gamma \): Discount factor for future rewards.
   - \( Q(s, a) \): Current estimate of the Q-value.

3. **Training Process**:
   - The agent interacts with the environment, updating weights after each step.
   - Over time, the weights converge to values that allow the agent to approximate the optimal Q-values.

---

### Key Concepts and Methods

#### Exploration vs. Exploitation

- **Exploration**: Taking random actions to discover new state-action pairs and improve learning.
- **Exploitation**: Choosing the best-known action to maximize rewards based on current knowledge.
- The agent uses an epsilon-greedy strategy to balance these behaviors.

#### Feature-Based Approximation

- Instead of storing a table of Q-values, the agent uses **features** to generalize across similar states.
- Features are extracted using a **feature extractor**, which defines a mapping from state-action pairs to a fixed-dimensional vector.
- Approximation makes it feasible to scale Q-learning to large state spaces.
