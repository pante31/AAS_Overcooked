<h1 align="center">Overcooked Optimization via A2C 🍳🤖</h1>

<h4 align="center">Autonomous And Adaptive Systems - Project Report</h4>
<p align="center"><i>Developed by Alessandro Tutone, Alma Mater Studiorum - University of Bologna</i></p>

---

## 📖 Overview

This repository contains the implementation of a reinforcement learning algorithm designed to learn cooperative behavior in a multi-agent system. The target environment is **Overcooked-AI**, a benchmark based on the popular video game where two agents must coordinate their actions (gathering onions, cooking, and plating) to maximize common cumulative rewards by delivering soups within a time limit.

While simple Deep Q-Networks (DQN) or one-step Actor-Critic methods struggle to achieve stable performance in this complex sequential task, this project successfully implements an **n-step Actor-Critic (A2C)** algorithm capable of consistently delivering high performance.

## ✨ Key Features & Methodology

* **Algorithm (n-step Actor-Critic):** Evolved from a baseline one-step Actor-Critic to an n-step update. This allows the agents to credit individual actions that contribute to distant future rewards. The n-step return and Temporal Difference (TD) error are defined as:

  $$G_{t:t+n}\doteq R_{t+1}+\gamma R_{t+2}+...+\gamma^{n-1}R_{t+n}+\gamma^{n}\hat{v}(S_{t+n},w)$$

  $$\delta\doteq G_{t:t+n}-\hat{v}(S_{t},w)$$
* **Observation Design & CNNs:** Transitioned from a basic featurized state encoding to a **lossless state encoding** (consisting of 26 binary matrices capturing spatial data like chefs' positions and dish locations). To process this, the architecture utilizes a Convolutional Neural Network (CNN) with 3 convolutional layers and 3 fully-connected layers.
* **Reward Shaping:** Overcame the challenge of sparse final rewards by introducing intermediate rewards. Examples include a **+0.2 reward** for each step the soup is in the "cooking" state and a **-0.05 penalty** when the soup is "ready" to encourage immediate delivery.
* **Mitigating Catastrophic Forgetting:** The agents were trained on Kaggle for 3300 episodes to reach near-optimal policy. The model weights were strategically saved prior to the onset of catastrophic forgetting, which occurred with continued training.

## 📂 Repository Structure

```text
├── overcooked_ai/             # Environment module and dependencies
├── weights/                   # Saved model weights (optimal policy around episode 3300)
├── AAS_Overcooked_A2C.ipynb   # Main Jupyter Notebook containing the A2C implementation and training loop
└── AAS___Project_Report.pdf   # Comprehensive project report detailing the theoretical approach and experiments
```

## 📊 Results

The implementation required extensive training to overcome initial convergence issues. Following the integration of reward shaping and the lossless state CNN architecture, the agents successfully learned a stable cooperative policy. The highest performance was recorded at approximately 3300 training episodes before the agents exhibited signs of catastrophic forgetting.

*Note: Due to the fixed-dimension input of the CNN architecture designed specifically for the `cramped_room` layout, the current model's generalization to other layouts remains an area for future improvement.*

---
<p align="center">
  <i>For a deep dive into the theoretical framework, network architectures, and full experimental results, please refer to the attached <code>AAS___Project_Report.pdf</code>.</i>
</p>
