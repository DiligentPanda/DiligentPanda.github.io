---
title: Dynamic Programming
date: 2017-06-24 21:35:11
categories: Programming/AI/Reinforcement Learning/Reinforcement Learning An Introduction
tags: [book,AI,Reinforcement Learning,FMDP,MDP,Dynamic Programming]
mathjax: true
---

# Dynamic Programming

* Book Name: Reinforcement Learning: An Introduction
* Authors: Richard S.Sutton, Andrew G.Barto
* Version: Second edition, 2016
* Chapter 4

DP algorithm are obtained by turning Bellman equaltions into update rules for improving *approximations* of the desired value functions.

## Policy Evaluation
Policy Evaluation: given a policy $\pi$, we compute the value functions for this policy.

The existence and uniqueness of $v_\pi$ ($q_\pi$) are guaranteed as long as either $\gamma \leq 1$ or this task is episodic(I'm not sure if my understanding is correct. See book.).

$$
\begin{align}
v_\pi(s)&=\sum_a\pi(a|s)\sum_{s',r|s,a}p(s',r|s,a)[r+\gamma v_\pi(s')]\\
q_\pi(s,a)&=\sum_{s',r}p(s',r|s,a)(r+\gamma\sum_{a'}\pi(a'|s')v_\pi(s',a'))
\end{align}
$$

We have above Bellman equations. If the dynamic of environment is completely known, then we actually have a series of linear equations. We can solver them by linear programming, for example.

Iterative Policy Evaluation: we use the right part of equation to update the left part. The sequence $\{v_k (q_k)\}$ will finally converge to the true value function.

Full Backup: All backups, such as in DP, are based on all possible next states rather than on sampled next states.

### Algorithm: Iterative Policy Evaluation
NOTICE: We always use in-place version algorithm whenever it ensure correctness. It is usually faster, besides. We usually pick state-value function as example. But it is similar for action-value function.

[to do]

Termination of algorithm:

* obtain small differences (see $\Delta$) between successive estimations.
* exceed an upper bound of the number of iterations.


## Policy Improvement

How could we obtain a better policy $\pi '$ from current policy $\pi$, given its value function? (We think all policy are deterministic without lose of generality: we can always find a deterministic optimal policy. It may not optimal for the real world problem but it is optimal theoretically to the MDP.)

Policy Improvement Theorem:
Let $\pi'$ be a deterministic policy, if
$$
q_\pi(s,\pi'(s))\geq v_\pi(s)
$$
for all $s\in S$, then
$$
v_\pi'(s)\geq v_\pi(s)
$$
for all $s\in S$. Namely $\pi'$ is a better policy than $\pi$. If the inequality is satisfied strictly at any（任何一个） state in the condition, then the inequality would be satisfied at at least one state in the result.

The proof of this theorem is rather easy. We can simply expand $q_\pi$ repeatly.

A natual choice for $\pi'$ is the *greedy* policy w.r.t the value function of $\pi$.
$$
\pi'(s)=\mathop{\arg\max}_a q_\pi(s,a)
$$
for all $s \in S$.

## Policy Iteration





## Value Iteration: a Special Case of Policy Iteration

## Asynchronous Dynamic Programming

## Generalized Policy Iteration & Efficienct of Dynamic Programming
