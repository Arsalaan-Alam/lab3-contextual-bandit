# Lab 3: Contextual Bandit–Based News Article Recommendation

**Course:** Reinforcement Learning Fundamentals  
**Student:** Arsalaan Alam  
**Roll Number:** U20230064

---

## Key Results

### Recommendation Engine Demo (first 3 test users)

| User   | Predicted context | Selected category | Sampled article (headline) |
|--------|-------------------|-------------------|----------------------------|
| U4058  | user_2            | Crime             | Chuck Schumer To Introduce Resolution To Name Senate Buildin... |
| U1118  | user_1            | Tech              | Alien Space Probes? Physics Paper Considers Craft Other Bein... |
| U6555  | user_2            | Crime             | The GOP Candidates Are Just A Bunch Of Immature Middle Schoo... |

### Expected reward (final Q) per arm

**Epsilon-Greedy (ε = 0.1):**

| Context | Arm 0 (Ent.) | Arm 1 (Edu.) | Arm 2 (Tech) | Arm 3 (Crime) |
|--------|--------------|--------------|--------------|---------------|
| User1  | -1.74        | -4.05        | **5.08**     | 2.90          |
| User2  | -8.43        | -1.29        | 4.45         | **9.22**      |
| User3  | -1.86        | -3.79        | **9.35**     | -4.34         |

**UCB (C = 1.0):**

| Context | Arm 0 (Ent.) | Arm 1 (Edu.) | Arm 2 (Tech) | Arm 3 (Crime) |
|--------|--------------|--------------|--------------|---------------|
| User1  | -1.47        | -4.88        | **5.09**     | 3.62          |
| User2  | -7.92        | -1.20        | 4.96         | **9.21**      |
| User3  | -3.30        | -3.65        | **9.39**     | -3.98         |

**SoftMax (τ = 1):**

| Context | Arm 0 (Ent.) | Arm 1 (Edu.) | Arm 2 (Tech) | Arm 3 (Crime) |
|--------|--------------|--------------|--------------|---------------|
| User1  | -3.45        | -3.96        | **5.10**     | 2.78          |
| User2  | -10.19       | 0.00         | 0.00         | **9.22**      |
| User3  | 0.00         | -3.50        | **9.35**     | -3.19         |

(Bold = best arm per context.)

---

## Final Report: Comparison and Observations

**Comparison of Epsilon-Greedy, UCB, and SoftMax**

- **Epsilon-Greedy** balances exploration (random arm with probability ε) and exploitation (best arm otherwise). It is simple and robust; lower ε (e.g. 0.1) typically yields higher average reward after convergence. The Q values above show clear best arms per context (Tech for User1/User3, Crime for User2).
- **UCB** selects arms by upper confidence bound Q + C√(ln t / N), so less-tried arms get exploration. It is deterministic and often sample-efficient. The final Q values align closely with Epsilon-Greedy (same best arms: Tech for User1/User3, Crime for User2).
- **SoftMax** (τ = 1) chooses arms with probability ∝ exp(Q/τ). For User2 and User3 some arms have zero pulls (Q = 0), so exploration was more uneven; best arms still match the other two (Crime for User2, Tech for User3).

**Effect of hyperparameters**

- **ε (Epsilon-Greedy):** Smaller ε (e.g. 0.1) gives higher average reward after convergence; larger ε (0.2, 0.3) explores more and can be noisier. Hyperparameter comparison plots in the notebook show this.
- **C (UCB):** Moderate C (e.g. 1.0) balances exploration and exploitation; very small C may under-explore, very large C may over-explore. The bar plot in the notebook compares C = 0.5, 1.0, 2.0.

**Strengths and limitations**

- **Epsilon-Greedy:** Easy to implement and tune; exploration is undirected (random).
- **UCB:** Systematic exploration, often sample-efficient; requires choosing C.
- **SoftMax:** Smooth policy; can be slower to converge and is sensitive to τ; some arms may be under-sampled (as seen for User2/User3).

---

## Student Submission Checklist (Lab 3)

Before submitting, ensure **all items below are completed**.

### Repository and branching
- [x] Repository on GitHub; work on branch `firstname_U20230xxx` (e.g. `arsalaan_U20230064`).
- [x] No work pushed to `master`.

### Notebook
- [x] One notebook at repo root: `lab3_results_<roll_number>.ipynb`.
- [x] Runs top to bottom without errors; all outputs (plots, metrics) visible.

### Sampler
- [x] Use `rlcmab_sampler` as provided; initialize with roll number; rewards only via `sampler.sample(j)`.

### Contextual bandit
- [x] Context = user category; arm = news category; arm mapping as in handout.
- [ ] All three algorithms implemented: Epsilon-Greedy, UCB, SoftMax.

### Evaluation and plots
- [x] Classification reported on validation split; T = 10,000 steps; plots: Average Reward vs Time (per context), hyperparameter comparison; axes, legend, title on every plot.

> Submissions that do not follow branch name, notebook naming, or sampler usage may not be evaluated.
