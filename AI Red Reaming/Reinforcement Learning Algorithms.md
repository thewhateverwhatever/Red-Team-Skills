# Reinforcement Learning (RL) (The "Trainer")

**Definition:** Reinforcement Learning is a type of machine learning where an **Agent** learns how to behave in an **Environment** by performing actions and seeing the results.

**The Goal:** Maximize the total **Reward** over time (not just immediate gains).

> **Analogy: Dog Training**
> You don't tell a dog exactly how to move its muscles to "sit."
> * **Action:** The dog sits. -> **Reward:** You give it a treat. (Dog learns: "Sitting is good.")
> * **Action:** The dog chews the sofa. -> **Penalty:** You say "No!" (Dog learns: "Chewing sofa is bad.")
> Through trial and error, the dog figures out the best strategy to get the most treats.

---

## 1. The Core Loop
RL is a cycle that repeats over and over:
1.  **Observation:** The Agent sees the current **State** of the world.
2.  **Decision:** The Agent chooses an **Action** based on its Policy.
3.  **Feedback:** The Environment reacts, giving the Agent a **Reward** (or penalty) and a **New State**.
4.  **Learning:** The Agent updates its strategy based on the result.



---

## 2. Core Concepts: The Vocabulary

### The Players
* **Agent (The Learner):** The entity making decisions (e.g., the robot, the Mario player, the self-driving car).
* **Environment (The World):** Everything outside the agent. It sets the rules and provides feedback (e.g., the maze, the game board, the highway).

### The Mechanics
* **State ($S$):** A snapshot of the current situation.
    * *Example:* "I am at coordinates (2,3) and facing North."
* **Action ($A$):** The move the agent makes.
    * *Example:* "Move Forward" or "Turn Left."
* **Reward ($R$):** The immediate feedback score.
    * *Positive:* (+10 points) Good job!
    * *Negative:* (-100 points) You hit a wall!
* **Policy ($\pi$):** The Strategy. It maps "States" to "Actions."
    * *Definition:* "If I am in State X, I should do Action Y."

---

## 3. The "Brain" of the Agent

How does the agent know if a state is "good"?

### The Value Function ($V$)
This predicts the **long-term** reward.
* **Concept:** Standing on a trap door might offer a piece of candy *now* (Reward +1), but it leads to falling in a pit *later* (Reward -100). The **Value Function** warns you: "This state looks good, but it has low long-term value."

### Types of Knowledge
* **State-Value:** How good is it to be in this position?
* **Action-Value (Q-Value):** How good is it to take *this specific action* right now?

---

## 4. Types of Algorithms

### Model-Based RL (The Planner)
The agent tries to build a map of the world in its head.
* **Analogy:** Navigating a maze with a map. You can simulate steps in your head ("If I go left, I hit a wall") before you actually move.
* **Pros:** Efficient planning.
* **Cons:** Hard to build a perfect model of complex worlds.

### Model-Free RL (The Trial-and-Error Learner)
The agent doesn't care how the world works; it just learns what to do.
* **Analogy:** Navigating a maze in the dark. You bump into a wall and think, "Ouch, won't do that again." You learn strictly from experience.
* **Pros:** Easier to implement in complex environments.
* **Cons:** Requires lots of trial and error (data).

---

## 5. Key Parameters

### Discount Factor ($\gamma$ Gamma)
Determines how much the agent cares about the future.
* **Range:** 0 to 1.
* **$\gamma = 0$ (Impulsive):** "I want the reward NOW." (Only cares about immediate reward).
* **$\gamma = 1$ (Visionary):** "I care about the future just as much as today." (Values long-term cumulative reward).

### Task Types
* **Episodic:** The task has a clear ending (Game Over).
    * *Example:* A game of Chess.
* **Continuous:** The task goes on forever.
    * *Example:* Regulating the temperature in a server room.


# Q-Learning (The "Cheat Sheet" Learner)

**Definition:** Q-Learning is a **Model-Free** Reinforcement Learning algorithm. It finds the best path by learning the "value" of every possible action in every possible state.

**Why "Model-Free"?**
The agent does not need a map or a physics book to understand the world. It learns purely by trial and error—bumping into walls and finding exits—just like learning to ride a bike by falling over.

<img width="660" height="330" alt="image" src="https://github.com/user-attachments/assets/95127191-dc40-46ce-9f46-b806de62d478" />

> **Analogy: The Self-Driving Car**
> A car starts with zero knowledge of the city.
> * It tries **accelerating** -> Crashes (Penalty).
> * It tries **stopping** at a red light -> Safe (Reward).
> Over time, it memorizes which actions lead to safety and speed without ever needing to read a traffic law book.

---

## 1. The Brain: The Q-Table

The "Brain" of this algorithm is a simple lookup table called the **Q-Table**.
* **Rows:** Every possible State (Where am I?)
* **Columns:** Every possible Action (What can I do?)
* **Cells:** The **Q-Value** (How good is this action in this state?)

**Example Q-Table (Grid World Robot)**

| State / Action | Up | Down | Left | Right |
| :--- | :--- | :--- | :--- | :--- |
| **S1** | -1.0 | 0.0 | -0.5 | **0.2** |
| **S2** | 0.0 | 1.0 | 0.0 | -0.3 |
| **S3** | 0.5 | -0.5 | 1.0 | 0.0 |
| **S4** | -0.2 | 0.0 | -0.3 | 1.0 |

* *Interpretation:* If I am in **S1** and go **Right**, the expected value is **0.2**.

---

## 2. The Math: The Bellman Equation

How do we update the numbers in the table? We use the **Q-Learning Update Rule**:

$$Q(s, a) = Q(s, a) + \alpha \cdot [r + \gamma \cdot \max(Q(s', a')) - Q(s, a)]$$

**The Variables:**
* $Q(s, a)$: The **current** value (what we thought was true).
* $\alpha$ (**Alpha**): **Learning Rate** (0 to 1). How much do we trust new info?
* $r$: **Reward**. What did we just get?
* $\gamma$ (**Gamma**): **Discount Factor** (0 to 1). How much do we care about the future?
* $\max(Q(s', a'))$: **Best Future Value**. What is the best possible score from the *next* state?

### Calculation Example
* **Current State:** S1
* **Action:** Move Right (to S2)
* **Reward ($r$):** 0.5
* **Old Q-Value:** 0.2
* **Best Future Q (in S2):** 1.0 (from moving Down)
* **Parameters:** $\alpha = 0.1$, $\gamma = 0.9$

$$Q(S1, Right) = 0.2 + 0.1 \cdot [0.5 + 0.9 \cdot 1.0 - 0.2]$$
$$Q(S1, Right) = 0.2 + 0.1 \cdot [1.2]$$
$$Q(S1, Right) = 0.32$$

*Result: The agent learned that moving Right is even better than it thought (0.2 $\to$ 0.32).*

---

## 3. The Algorithm Loop

1.  **Initialize:** Create a Q-Table full of zeros.
2.  **Choose:** Pick an action (using Exploration vs. Exploitation).
3.  **Act:** Perform the action in the environment.
4.  **Observe:** See the new State ($s'$) and get the Reward ($r$).
5.  **Update:** Use the math formula above to update the Q-Value in the table.
6.  **Repeat:** Go back to step 2 until the table stabilizes (converges).

---

## 4. The Dilemma: Exploration vs. Exploitation

The agent constantly faces a choice:

* **Exploitation:** Do what I *know* works to get a reward now. (Go to your favorite restaurant).
* **Exploration:** Try something random to see if it's better. (Try the new sushi place; it might be terrible, or it might be the best food ever).

### The Solution: Epsilon-Greedy Strategy ($\epsilon$-Greedy)
We use a parameter called **Epsilon ($\epsilon$)** to decide.

* **Roll a die:**
    * If roll < $\epsilon$: **Explore** (Pick a random action).
    * If roll > $\epsilon$: **Exploit** (Pick the best action in the table).
* **Decay:** Usually, we start with high $\epsilon$ (0.9 - Explore a lot) and slowly lower it to 0.1 (Exploit more) as the agent gets smarter.

---

## 5. Data Assumptions

1.  **Markov Property:** The history doesn't matter. The decision depends *only* on where you are right now (State), not how you got there.
2.  **Stationary Environment:** The rules of the game don't change while you are playing (e.g., "North" always moves you Up).


# SARSA (The "Cautious" Learner)

**Definition:** SARSA (**S**tate-**A**ction-**R**eward-**S**tate-**A**ction) is a **Model-Free** Reinforcement Learning algorithm.

**The Core Difference:**
* **Q-Learning (Off-Policy):** The "Optimist." When learning, it assumes that in the *next* step, it will take the *best possible* action (even if it's currently exploring randomly). It learns the "perfect" path, ignoring the risks of exploration.
* **SARSA (On-Policy):** The "Realist." It learns based on the action it *actually* plans to take next (including any random slips or exploration). It learns the "safe" path that accounts for its own behavior.

> **Analogy: The Cliff Walk**
> Imagine walking along a narrow path on a cliff edge.
> * **Q-Learning:** Walks right on the edge because that is the shortest path. It assumes "I will never trip." (Optimal but risky).
> * **SARSA:** Walks a few feet away from the edge. It realizes "I might trip or take a random step (exploration), so I better stay safe." (Safer but slightly longer).

---

## 1. The Name: S-A-R-S-A

The name comes from the sequence of data the algorithm uses in one loop:
1.  **S:** Current **State**.
2.  **A:** Current **Action**.
3.  **R:** **Reward** received.
4.  **S':** Next **State**.
5.  **A':** Next **Action**.

---

## 2. The Math: The Update Rule

$$Q(s, a) \leftarrow Q(s, a) + \alpha \cdot [r + \gamma \cdot Q(s', a') - Q(s, a)]$$

**Compare this to Q-Learning:**
* *Q-Learning:* Uses $\max(Q(s', a'))$ -> "The best possible future action."
* *SARSA:* Uses $Q(s', a')$ -> "The actual action I am going to take next."

**Variables:**
* $Q(s, a)$: Current value.
* $\alpha$ (**Alpha**): Learning Rate.
* $r$: Reward.
* $\gamma$ (**Gamma**): Discount Factor.
* $Q(s', a')$: The value of the *actual* next move.

---

## 3. The Algorithm Loop

1.  **Initialize:** Create a Q-Table full of zeros.
2.  **Choose:** Pick an action **$A$** for state **$S$** (using $\epsilon$-greedy).
3.  **Act:** Do action **$A$**.
4.  **Observe:** See the reward **$R$** and the new state **$S'$**.
5.  **Choose Next:** Pick the next action **$A'$** for state **$S'$** (using $\epsilon$-greedy) **before updating**.
6.  **Update:** Use the formula above with $S, A, R, S', A'$.
7.  **Shift:** Current State becomes $S'$, Current Action becomes $A'$.
8.  **Repeat.**

---

## 4. On-Policy vs. Off-Policy

This is the most critical concept to understand about SARSA.

| Feature | SARSA (On-Policy) | Q-Learning (Off-Policy) |
| :--- | :--- | :--- |
| **Learning Source** | Learns from the **Current Policy** (including mistakes/exploration). | Learns from the **Optimal Policy** (ignoring mistakes/exploration). |
| **Behavior** | **Conservative / Safe.** Avoids penalties that might happen during exploration. | **Aggressive / Optimal.** Finds the fastest path, assuming perfect play. |
| **Use Case** | Robots, physical machinery, or high-risk scenarios where "learning the hard way" is dangerous. | Simulations or games where crashing doesn't cost money. |

---

## 5. Exploration Strategies

How does SARSA pick actions?

### A. Epsilon-Greedy ($\epsilon$-Greedy)
* **Method:** Roll a die. Small chance ($\epsilon$) to pick a random action; otherwise, pick the best one.
* **SARSA Effect:** Because SARSA knows it might pick a random action next, it naturally stays away from "dangerous" states (like the cliff edge) where a random move would be catastrophic.

### B. Softmax
* **Method:** Assign probabilities based on values. Better actions have higher probabilities, but bad actions still have *some* chance.
* **SARSA Effect:** Leads to smoother learning than strict Epsilon-Greedy.

---

## 6. Convergence & Tuning

To make SARSA work, you need to tune two main knobs:

1.  **Learning Rate ($\alpha$):**
    * *High:* Learns fast but jitters (unstable).
    * *Low:* Stable but learns slowly.
2.  **Discount Factor ($\gamma$):**
    * *High (near 1):* Cares about long-term safety.
    * *Low (near 0):* Cares only about immediate rewards.

**Convergence Condition:**
SARSA will eventually find the optimal policy, **IF** you slowly decrease the exploration rate ($\epsilon$) to zero over time. This turns it from a "learner" into a "master."

---

## 7. Data Assumptions

1.  **Markov Property:** The future depends only on the present state, not the past.
2.  **Stationary Environment:** The rules (physics/rewards) don't change while learning.
