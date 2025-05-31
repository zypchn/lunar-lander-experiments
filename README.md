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

I tried three Deep-RL algorithms defined in `Stable-Baselines3`, the deep RL library:
1) **Proximal Policy Optimization (PPO):**
2) **Deep Q Network (DQN)**
3) xxx

<br/>

## Methods
I trained all three algorithms for a minimum of 1 million timesteps. For `Lunar Lander`, action space is a 8-dimensional vector so our input to the algorithms is a vector. Thus, I used Multi Layer Perceptron Policy (`MlpPolicy`) for all three of them. Final fixed hyperparameter is the gamma being *0.999*, which makes the cumulative reward fairly undiscounted. Here is why: our task is to safely land the Lunar Lander, which means out agents should care about the long-term reward. Below are the other hyperparameters for each algorithm: 

**PPO** <br/>
n_envs | n_steps | batch_size | n_epochs | gae_lambda | ent_coef | total_timesteps
--- | --- | --- | --- | --- | --- | --- 
8 | 1024 | 32 | 5 | 0.98 | 0.01 | 1000000

**DQN**
