---
title: Finite Markov Decision Processes
date: 2017-06-17 14:24:08
categories: Programming/AI/Reinforcement Learning/Reinforcement Learning: An Introduction
tags: [book,AI,Reinforcement Learning,FMDP,MDP]
---
# Finite Markov Decision Processes
* Book Name: Reinforcement Learning: An Introduction
* Authors: Richard S.Sutton, Andrew G.Barto
* Version: Second edition, 2016
* Chapter 3

## The Mathematical Structure of the RL Problem
### The Agent-Environment Interface
RL Problem: learning from interaction to achieve goal.
Interaction between:
* agent: learner and decision maker
* environment

over a sequence of steps.

The specification of their interface defines a particular task:
* states: some representation of the env (Notice that this representation depends on how env looks like, how agent observes the env (probably limited) and how agent concludes the received information(see MDP).)
* actions: The set of feasible acitons at a state, $A(S_t)$
* rewards: Specified by env.

policy: Probability of actions at states s, $\pi_t(a|s)$.

> Everything inside the agent is *completely* known and controllable; everything outside (env) is *incomepletely* controllable but may or may not be complete known.

The agent affects environment through its actions.

Notice all of there is how we model the world.

### Goals, Rewards and Returns
Reward hypothesis: Goals = maxmization of the *expected cumulative sum* of reward.

> It is crucial that the rewards we set up truly indicate what we want accomplished.

* Rewards = intermediate feedback from env.
* Returns = the sum of rewards. (A long-run term)
  * Episodic tasks: $G_T=R_{t+1}+R_{t+2}+...+R_T$
  * Continuing tasks: $G_T=R_{t+1}+\gamma R_{t+2}+\gamma^2R_{t+3}+...$ (discounted return, could be applied to episodic tasks as well.)

To unify episodic tasks and continuing tasks, we can consider episode termination to be the entering of a special absorbing state that transitions only to itself with zero rewards.

### Markov Property
If the state signal has the *Markov property*, env's dynamics can be defined by specifying only
 $$p(s',r|s,a)=Pr\{S_{t+1}=s',R_{t+1}=r|S_t=s,A_t=a\}\quad(1)$$

 State signals are a complete (and compact) summary of all history.

### Markov Decision Processes (MDP)
If action and state spaces are finite, then it is called a *finite MDP*.

The dynamics of env as described above can be specified only by $p(s',r|s,a)$.

Some other useful values:
* Expected rewards for state-action pairs, $r(s,a)$
* State-transition probabilities, $p(s'|s,a)$
* Expected rewards for state-action-next-state triples $r(s,a,s')$

**Transition graph**

### Value Functions
> A *policy's value function* assign to each state or state-action pair, the expected return from that state or state-action pair, given that agent uses the policy.

They are evaluated for comparing the policies.
* State-value function
$$V_{\pi}(s)=E_\pi[G_t|S_t=s]=E_\pi[\sum^\inf_{k=0}\gamma^kR_{t+1+k}|S_t=s]$$
* Action-value function
$$Q_\pi(s,a)=E_\pi[G_t|S_t=s,A_t=a]=E_\pi[\sum^\inf_{k=0}\gamma^kR_{t+1+k}|S_t=s,A_t=a]$$

They can be estimated:
* Mento Carlo methods + average
* parameterization

#### Bellman Equation
$$
\begin{align}
v_\pi(s)&=E_\pi[R_{t+1}+\gamma v_\pi(S_{t+1})|S_t=s]\\
&=\sum_a\pi(a|s)\sum_{s',r|s,a}p(s',r|s,a)[r+\gamma v_\pi(s')]\\
q_\pi(s,a)&=E_\pi[R_{t+1}+\gamma q_\pi(S_{t+1},A_{t+1})|S_t=s,A_t=a]\\
&=\sum_{s',r}p(s',r|s,a)(r+\gamma\sum_{a'}\pi(a'|s')v_\pi(s',a'))
\end{align}
$$

The value function is the unique solution to its Bellman equation.

**Backup diagrams**
#### Optimal Value Functions
Policy $\pi$ is better than or equal to Policy $\pi'$:
$$v_\pi(s)\geq v_{\pi'}(s)$$
for *all* $s\in S$.

We have a *unique* optimal value function (state $v_*$; action $q_*$, but possibly multiple optimal policies ($\pi_*$).

#### Bellman Optimality Equation
First of all, we pick the best action for each state. So we have:
$$V_*(s)=\max_{a\in A(s)}q_{\pi_*}(s,a)$$

We have two equations as well:
$$
\begin{align}
v_*(s)&=\max_aE[R_{t+1}+\gamma v_*(S_{t+1})|S_t=s,A_t=a]\\
&=\max_{a\in A(s)}\sum_{s',r}p(s',r|s,a)(r+\gamma v_*(s'))\\
q_*(s,a)&=E[R_{t+1}+\gamma \max_{a'}q_*(S_{t+1},a')|S_t=s,A_t=a]\\
&=\sum_{s',r}p(s',r|s,a)[r+\gamma \max_{a'}q_*(s',a')]\\
\end{align}
$$

**Backup diagrams**

Notice that at cost of representing a function of state-action pairs instead of just of states, the optimal action-value funciton $q_*$ allows optimal actions to be selected without having to know anything about the env's dynamics.
