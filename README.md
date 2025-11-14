**Assignment 3  
Yogesh Kumar Gopal**

**8996403**

**Git Hub Link :-** [**https://github.com/Yogesh-333/Assignment3-Reinforcement-Learning-Programming.git**](https://github.com/Yogesh-333/Assignment3-Reinforcement-Learning-Programming.git)

## Deep Q-Network (DQN) on PongDeterministic-v4: Experiment Report

### 1\. Final Network Architecture

The implemented Deep Q-Network (DQN) model is a convolutional neural network architecture optimized for visual input. It processes **four consecutive preprocessed Pong frames** (each 84 x 80 pixels) to predict Q-values for the six possible actions in the PongDeterministic-v4 environment.

- **Conv1**: 32 filters, 8 x 8 kernel, stride 4, ReLU activation
- **Conv2**: 64 filters, 4 x 4 kernel, stride 2, ReLU activation
- **Conv3**: 64 filters, 3 x 3 kernel, stride 1, ReLU activation
- **FC1**: 512 fully connected neurons, ReLU activation
- **FC2**: Output layer producing Q-values for **6 actions**

### 2\. Experimental Setup

| **Parameter** | **Baseline Value** | **Test Value** |
| --- | --- | --- |
| **Environment** | PongDeterministic-v4 (Atari) | \-  |
| --- | --- | --- |
| **Total Episodes** | **20** | \-  |
| --- | --- | --- |
| **Batch Size** | 8 (Default) | 16  |
| --- | --- | --- |
| **Target Update Rate** | 10 episodes (Default) | 3 episodes |
| --- | --- | --- |
| **Replay Buffer Capacity** | 20,000 | \-  |
| --- | --- | --- |
| **Exploration Policy** | ε-greedy with ε_init=1.0, ε_min=0.05, decay=0.995 | \-  |
| --- | --- | --- |

### 3\. Observations and Results

The 20-episode run represents the **early exploration phase** of the DQN algorithm, where the agent is primarily taking random actions (\\epsilon only decayed to 0.951). All scores are negative, which is expected before the network learns effective paddle movement.

<img width="1092" height="586" alt="image" src="https://github.com/user-attachments/assets/f6bcac43-d926-4f4c-ab55-061bad274fa2" />

<img width="1092" height="587" alt="image" src="https://github.com/user-attachments/assets/439d186a-78bb-4f9b-9a91-1513eaa78d93" />


#### A. Batch Size Comparison (\[8 (default), 16\])

| **Configuration** | **Metric @ Episode 20** | **Observation** |
| --- | --- | --- |
| **Batch 8** | Avg Return **\-20.2** | Shows minor improvement from initial -21, but results are random-walk dominant. The smaller batch size allows for quicker weight updates per step, but any effect is currently masked by the high \\epsilon. |
| --- | --- | --- |
| **Batch 16** | Avg Return **\-20.4** | Slightly worse performance than Batch 8 at this stage. A larger batch requires a more diverse buffer for stable gradients, which the initial 20 episodes do not provide. |
| --- | --- | --- |

**Commentary:** In this early phase, the differences are primarily statistical noise. However, Batch 8 is marginally superior, suggesting that **smaller batches still offer a slight learning advantage** due to their higher frequency of weight updates, even when exploration is high.

#### B. Target Update Rate Comparison

| **Configuration** | **Metric @ Episode 20** | **Observation** |
| --- | --- | --- |
| **Update 10** | Avg Return **\-20.4** | The default, stable behavior. The target network has only been updated twice, so stability effects are minimal. |
| --- | --- | --- |
| **Update 3** | Avg Return **\-20.6** | Slightly lower average reward. **More frequent updates (3 episodes)** often lead to instability and oscillation in later training. At this stage, it likely contributes to noisier targets that hinder the small learning signal. |
| --- | --- | --- |

**Commentary:** While frequent updates are expected to cause oscillations in the future, the **default Update 10** is preferred as it maintains stable target Q-values, which is a core principle for DQN stability.

#### C. Learning Trend (General)

- **Score per Episode:** Scores fluctuate between -19 and -21 as expected, since the agent is still largely performing random actions, which rarely result in a win.
- **Epsilon Decay:** The exploration factor \\epsilon has only decayed from 1.0 to \\approx 0.951, confirming that the agent is spending most of its time **exploring randomly** to fill the replay buffer.

### 4\. Best Hyperparameter Combination

Based on the general principles of DQN and the limited empirical evidence, the best combination is the **Baseline Configuration**:

- **Batch Size: 8**
- **Target Update Rate: 10 episodes**

**Justification:** The primary goal is stability and efficient learning once exploitation begins.

- **Batch Size 8:** It provides a better balance by allowing for more frequent weight updates per step compared to Batch 16, which is generally better for dynamic environments like Atari.
- **Update Rate 10:** This infrequent rate **decorrelates the target values** from the current policy network, preventing the "chasing" of the target and ensuring a more stable, non-oscillatory learning process necessary for successful DQN convergence.

### 5\. Conclusion

The DQN agent was successfully implemented, and the three required experiments were executed over 20 episodes. The model validates the DQN framework for visual reinforcement learning, showing its **initial exploration phase** is on track. Although 20 episodes are insufficient to observe significant improvement (which requires 100+ episodes), the choice of **Batch=8** and **Update=10** aligns with best practices for achieving long-term stability and consistent performance.
