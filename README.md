# lunar-lander-experiments

<br/>

## Introduction
Reinforcement Learning is a computational approach to learning from actions. We build an agent that learns from the environment by interacting with it through trial and error and receiving rewards (negative or positive) as feedback. The goal of any RL agent is to maximize its expected cumulative reward (also called expected return) because RL is based on the reward hypothesis, which is that all goals can be described as the maximization of an expected cumulative reward. To solve an RL problem, you want to find an optimal policy. The policy is the “brain” of your agent, which will tell us what action to take given a state. The optimal policy is the one which gives you the actions that maximize the expected return. There are two ways to find optimal policy:
1) **Policy-Based Methods:** By training your policy directly.
2) **Value-Based Methods:** By training a value function that tells us the expected return the agent will get at each state and use this function to define our policy. 

In this study, I tried to find the optimal policy for `LunarLander-v2` : a classic rocket trajecory optimization problem defined in `Gymnasium`, the environment library. 
<br/>

**Action Space for Lunar Lander:** There are four discrete actions available:
- 0: do nothing
- 1: fire left orientation engine
- 2: fire main engine
- 3: fire right orientation engine

**Observation Space for Lunar Lander:** The state is an 8-dimensional vector: 
- the coordinates of the lander in *x* & *y*
- its linear velocities in *x* & *y*
- its angle, its angular velocity
- two booleans that represent whether each leg is in contact with the ground or not

There are only a number of algorithms can be used for the **Lunar Lander** problem, because of it's action and observation space. I tried three Deep-RL algorithms defined in `Stable-Baselines3`, the deep RL library:
1) **[Proximal Policy Optimization (PPO)](https://stable-baselines3.readthedocs.io/en/master/modules/ppo.html)**
2) **[Deep Q Network (DQN)](https://stable-baselines3.readthedocs.io/en/master/modules/dqn.html#)**
3) **[Advantage Actor-Critic (A2C)](https://stable-baselines3.readthedocs.io/en/master/modules/a2c.html)**

<br/>

## Methods
I trained all three algorithms for a minimum of 1 million timesteps. I choose a threshold of *200* to make my algorithm be on-par with human achieved scores. Until the models reached the threshold, I incremented the total timesteps by 500_000.
For **Lunar Lander**, action space is a 8-dimensional vector so our input to the algorithms is a vector. Thus, I used Multi Layer Perceptron Policy (`MlpPolicy`) for all three of them. Final fixed hyperparameter is the gamma being *0.999*, which makes the cumulative reward fairly undiscounted. Here is why: our task is to safely land the Lunar Lander, which means out agents should care about the long-term reward. Below are the other non-default hyperparameters for each algorithm (the default hyperparameters can be found from the links above): 

**PPO** <br/>
n_envs | n_steps | batch_size | n_epochs | gae_lambda | ent_coef 
--- | --- | --- | --- | --- | --- 
8 | 1024 | 32 | 5 | 0.98 | 0.01 

**DQN** <br/>
batch_size | learning_rate | learning_starts | buffer_size | exploration_final_eps | exploration_fraction | policy_kwargs
--- | --- | --- | --- | --- | --- | ---
128 | 3e-4 | 1_000 | 200_000 | 0.1 | 0.3 | net_arch=[256, 256]

**A2C** <br/>
gae_lambda | n_steps | ent_coef | vf_coef | max_grad_norm | policy_kwargs
--- | --- | --- | --- | --- | ---
0.95 | 16 | 0.05 | 0.25 | 0.5 | net_arch=[256, 256]

<br/>

## Results
A score of 200+ is considered acceptable for Lunar Lander, in other words: successful landing. After training each algorithm, I evaluated the results for 10 episodes by creating an identical `Lunar-Lander-v2` environment. Here are the mean and standard deviation for all rewards of each model:
- **PPO** -> 267.12 +/- 20.190092729862148
- **DQN** -> 276.12 +/- 12.400879722973757
- **A2C** -> 59.95 +/- 123.98633475169582

## Discussions
In case of LunarLander environment, PPO and DQN algorithms yielded similar results and outperformed the A2C algorithm. PPO's biggest advantage is that it improves our agent's training stability by avoiding too large policy updates. It should be noted that PPO yielded the 2nd highest reward without policy optimization (increasing the number of neural nets). When the neural net architecture was not altered, both DQN and A2C scored sub-zero rewards (near -150).
Further experimentation is required to cross-referance environments and hyperparameters. 
A later study can be conducted on an Atari environment like Pac-Man, or we can increase the number of models.
