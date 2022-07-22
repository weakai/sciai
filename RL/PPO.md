---
title: PPO
date:
tags: []
---

强化学习可以按照方法学习策略来划分成基于值和基于策略两种。而在深度强化学习领域将深度学习与基于值的Q-Learning算法相结合产生了DQN算法，通过经验回放池与目标网络成功的将深度学习算法引入了强化学习算法。其中最具代表性分别是Q-Learning与Policy Gradient算法，将Q-Learning算法与深度学习相结合产生了Deep Q Network，而后又出现了将两种方式的优势结合在一起的更为优秀Actor Critic，DPG, DDPG，A3C，TRPO，PPO等算法。而本文所采用的是目前效果较好的近端策略优化算法PPO。

PPO 算法是一种新型的 Policy Gradient 算法，Policy Gradient 算法对步长十分敏感，但是又难以选择合适的步长，在训练过程中新旧策略的的变化差异如果过大则不利于学习。
PPO 提出了新的目标函数可以再多个训练步骤实现小批量的更新，解决了 Policy Gradient 算法中步长难以确定的问题。
其实 TRPO 也是为了解决这个思想但是相比于 TRPO 算法 PPO 算法更容易求解。

PolicyGradient 回顾

重新回顾一下 PolicyGradient 算法，Policy Gradient 不通过误差反向传播，它通过观测信息选出一个行为直接进行反向传播，当然出人意料的是他并没有误差，而是利用 reward 奖励直接对选择行为的可能性进行增强和减弱，好的行为会被增加下一次被选中的概率，不好的行为会被减弱下次被选中的概率。

## Reference

- [【强化学习】PPO(Proximal Policy Optimization)近端策略优化算法](https://blog.csdn.net/qq_30615903/article/details/86308045)