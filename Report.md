# Reacher Agent

# Project's Goal

In this project, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of your agent is to maintain its position at the target location for as many time steps as possible.

![alt text](https://github.com/saj122/ReacherAgent/blob/master/images/reacher.gif)

# Environment Details

The environment is based on Unity ML-agents. The project environment provided by Udacity is similar to the Reacher environment on the Unity ML-Agents GitHub page.

    The Unity Machine Learning Agents Toolkit (ML-Agents) is an open-source Unity plugin that enables games and 
    simulations to serve as environments for training intelligent agents. Agents can be trained using reinforcement 
    learning, imitation learning, neuroevolution, or other machine learning methods through a simple-to-use Python API.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

Option 1: Solve the First Version
   * The task is episodic, and in order to solve the environment, your agent must get an average score of +30 over 100 consecutive episodes.

Option 2: Solve the Second Version
   * The barrier for solving the second version of the environment is slightly different, to take into account the presence of many agents. In particular, your agents must get an average score of +30 (over 100 consecutive episodes, and over all agents). Specifically,

After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 20 (potentially different) scores. We then take the average of these 20 scores.
This yields an average score for each episode (where the average is over all 20 agents).

In my implementation, I have chosen to solve the second version of the environment using the DDPG algorithm. 

# Algorithm

The algorithm used in solving the environment is the Deep Deterministic Policy Gradient (DDPG). DDPG is an algorithm which learns a Q-function and a policy.

![alt text](https://github.com/saj122/ReacherAgent/blob/master/images/ddpg.png)

The algorithm image was taken from a Towards Data Science [article](https://towardsdatascience.com/introduction-to-various-reinforcement-learning-algorithms-i-q-learning-sarsa-dqn-ddpg-72a5e0cb6287).

# Code Implementation

The code used here is derived from the DDPG bipedal tutorial from the Deep Reinforcement Learning Nanodegree.

The code is written in Python 3.6 and is relying on PyTorch 0.4.0 framework.

The code implements an Actor and Critic classes.

The Actor and Critic classes each implement a Target and a Local Neural Networks used for the training.

Agent implements the DDPG agent and a Replay Buffer memory.

The learn method updates the policy and value parameters using given batch of experience tuples.

Methodology:
   * Increasing the number of steps per episode seemed to increase the starting learning rate.
   * The algorithm and environment was very sensitive to noise. A low sigma and seed helped in learning.
   * Using similar learning rates for the actor and critic networks helped.
   * As suggested in the project guidelines, only updating every 20 episodes 10 times helped stabilize learning.
   * Along with larger update intervals, gradient clipping of the critics parameters was the final step in stable learning.
   
# DDPG Hyperparameters

GAMMA = 0.99            
TAU = 1e-3             
LR_ACTOR = 1e-3        
LR_CRITIC = 1e-3        
WEIGHT_DECAY = 0.0     
BATCH_SIZE = 128       

#### Actor Neural Network
Input nodes (33) -> Fully Connected Layer (256 nodes, Relu activation) -> Fully Connected Layer (256 nodes, Relu activation) -> Ouput nodes (4, tanh activation)

#### Critic Neural Network
Input nodes (33) -> Fully Connected Layer (256 nodes, Relu activation) -> Fully Connected Layer (256 + 4 nodes, Relu activation) -> Fully Connected Layer (256 nodes, Relu activation) - > Fully Connected Layer (256 nodes, Relu activation) - > Fully Connected Layer (256 nodes, Relu activation) - > Fully Connected Layer (128 nodes, Relu activation) - >Ouput nodes (1, tanh activation)

# Results
Given the hyperparameters and neural network the agent was able to achieve an average reward of 30.28 in 128 episodes.

![alt text](https://github.com/saj122/ReacherAgent/blob/master/images/scores.png)

# Future Work

It might be better to use another algorithm like PPO, A3C, and D4PG that use multiple parallel copies of the same agent to distribute the task of gathering experience. As stated, the PPO algorithm performs well on continuous action environments.
