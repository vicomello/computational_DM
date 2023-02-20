# computational_DM
Computational Models for Decision Making (Final project)

This repo contais the files of the final project that I did for the course "Computational Models for Decision Making".

In this final project, I wanted to model the process of deciding what to share on social media. 

The goal of the study was to examine how the structure of rewards can shape what criteria are prioritized in deciding what to share and how the structure of rewards can change the proportion of true/false information in a network. To that end, reinforcement learning models were used to model how people decide what criterion to use. Specifically, agents had to decide if they wanted to do one of two tasks: the True/False task, in which they decided whether a piece of information was true or false, and the Interest task, in which they decided whether they found a piece of information interesting or not. Once they selected the task, they then performed it and received a probabilistic reward. By manipulating average rewards and odds of backfiring, we found that agents learned to use the criteria that maximized rewards and only with generalizable learning did the amount of false information decreased in the network.

## Computational Models
Two computational models were used to describe the possible ways in which agents learn what to share on social media. The models assume that, when using social media, people encounter information and judge it according to different criteria. In each model, the agents selects the criterion it wants to use to judge a piece of information (also called a task). The task is to either determine if a piece of information (stimulus) is a) true or false or b) interesting or not. Agents select one of the tasks (a or b) based on their values under a SoftMax policy. After selecting one of the tasks, the agents then perform the task (they either judge the information as true/false or judge the information as interesting or not). Whenever the agent (correctly or incorrectly) judges the information as true or as interesting, they will receive a reward associated with that judgment. In other words, the models assume that once they find a piece of information true or interesting, they will share it and they will receive some form of reward for it. Whenever the judge the information as false or uninteresting, the reward is zero. 
In the first model, the agent simply learns how to choose the task (true/false or interesting) that maximizes the rewards. The second model differs from the first in that it also uses the reward as information to improve its accuracy in detecting false information. 
 Model 1: Task-choosing only model. The agent selects one of the two tasks using a policy determined by a SoftMax function. Values for each task start at zero and are updated according to a reward function. The agent choices are determined by:
 
<img width="220" alt="image" src="https://user-images.githubusercontent.com/43184812/220168250-1e374379-ae9d-4d09-ac41-37312d54d368.png">

Where P_t (a) is the probability of selecting task a at time t, 〖ActionValues〗_t are the values for the tasks at time t, and tau is the inverse temperature parameter. The action values are updated according to the reward found, such that:

<img width="563" alt="image" src="https://user-images.githubusercontent.com/43184812/220168294-86e13136-1d82-4632-8467-b7f48f68e50b.png">

The value for action a at time t is determined by the expected values for that action, the Learning Rate (LR) and the prediction error, determined by the difference between the observed reward at time t (R_t) and the expected values.
The rewards depend on the task selected, on the stimulus presented, and on the agent accuracy. If the True/False task is selected, the agent has 3 potential rewards. If the stimulus is False and the agent correctly classifies it as false or if the stimulus is True, but the agent classifies it False, then the agent decides to not to share it and the reward is zero. If the stimulus is True and the agent accurately judges it as True, reward is determined by R ~ N(μ ,σ^2) where N is a normal distribution of mean μ and sd σ. If the stimulus is false but agent classifies it as true, reward is given by:
 
 R ~ N(μ ,σ^2)- B(1,p)* N(μc ,σ^2) 

Where reward R is determined by sampling from normal distribution of mean μ and sd σ minus the product of a Bernoulli distribution (0 or 1) with probability p and a normal distribution where the mean μ is multiplied by constant c.
If the agent selects the Interest task, the reward is determined by the stimulus vector, the interests vector, and a random multiplier. Each agent has an interests vector (î), in which 10 topics are rated as interesting in a scale from 0 to 3. Each stimulus is a vector (s ̂), in which each cell represents the presence/absence (1 or 0) of a topic. The distribution of topics in the stimulus are given by B(1,.25). The reward associated with judging an information interesting is then given by:

R ~ î*s ̂* N(μ ,σ^2 )-B(1,p)*N(μc ,σ^2)

Where î*s ̂ is the product between the interests vector and the stimulus vector, B(1,p) is a Bernoulli distribution with probability p, and N(μc ,σ^2) is a normal distribution where mean μ is multiplied by constant c.
Simulations were performed so that the mean values of all rewards had initially similar magnitudes (average of 1). SDs varied across different rewards, as the reward for sharing fake and interesting content should have more variability than the reward for sharing true content (assuming that fake content viralizes more easily but it is also more likely to backfire).
Model 2: Learning Model. Model 2 has the same policy and reward distributions of model 1. The only difference is that one of Model 2 parameters changes according to the reward distribution. p_acc is the probability of the agent correctly guessing the accuracy of the stimulus. In model 1, this parameter is fixed at .05. In model 2, this parameter is adjusted (+.01) every time the agent receives a negative reward for sharing false information.

A report of the results can be found in the word (.doc) file. 
