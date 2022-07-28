---
title: RL in GAN
date: 2022-722
---

## 1. 基础：文本生成模型的标准框架
## 2. 问题：GAN 为何不能直接用于文本生成
### 2.1. GAN 基础知识
### 2.2. GAN 面对离散型数据时的困境（啥是离散型数据？）
## 3. 过渡方案：对于 GAN 的直接改进用于文本生成
### Wasserstein-divergence，额外的礼物

相对于更通用的 FGAN 的 F 散度，F 散度还是只能适用于连续。
WGAN 生成的文本虽然远不及当下最牛 X 的文本生成效果，但好歹能以 character 为单位生成出一些看上去稍微正常一点的结果了，对比之下，GAN 关于文本生成的生成结果显然是崩塌的。

### Gumbel-softmax，模拟 Sampling 的 softmax

论文的作者寻找了一种可以高仿出 Sampling 效果的特殊 softmax，使得 softmax 的直接输出既可以保证与真实分布的重叠，又能避免 Sampling 操作对于其可微特征的破坏。

## 4. RL 在 GAN 文本生成中所扮演的作用

### 4.1 关于 Reinforcement Learning 的闲聊闲扯

强化学习（Reinforcement Learning，RL）由于其前卫的学习方式，本不如监督学习那么方便被全自动化地实现，并且在很多现实应用中学习周期太长，一直没有成为万众瞩目的焦点，直到围棋狗的出现，才吸引了众多人的眼球。

**RL**通常是一个马尔科夫决策过程，在各个状态 ![[公式]](https://www.zhihu.com/equation?tex=s_i) 下执行某个动作 ![[公式]](https://www.zhihu.com/equation?tex=x_i) 都将获得奖励（或者是"负奖励"——惩罚） ![[公式]](https://www.zhihu.com/equation?tex=Reward%28s_i%2C+x_i%29) ，而将从头到尾所有的动作连在一起就称为一个**“策略”**或“策略路径” ![[公式]](https://www.zhihu.com/equation?tex=%5Ctheta%5E%5Cpi) ，强化学习的目标就是找出能够获得最多奖励的最优**策略**

自AlphaGo使得强化学习猛然进入大众视野以来，大部分对于强化学习的理论研究都将游戏作为主要实验平台，这一点不无道理，强化学习理论上的推导看似逻辑通顺，但其最大的弱点在于，基于人工评判的奖励 ![[公式]](https://www.zhihu.com/equation?tex=Reward) 的获得，让实验人员守在电脑前对模型吐出来的结果不停地打分看来是不现实的，游戏系统恰恰能会给出正确客观的打分（输/赢 或 游戏Score)。

### 4.2. SeqGAN 和 Conditional SeqGAN

RL + GAN for Text Generation，SeqGAN 站在前人**RL** Text Generation 的肩膀上，可以说是 GAN for Text Generation 中的代表作。上面虽然花了大量篇幅讲述 **RL** ChatBot 的种种机理，其实都是为了它来做铺垫。试想我们使用 GAN 中的判别器 **D** 作为强化学习中奖励 ![[公式]](https://www.zhihu.com/equation?tex=Reward) 的来源，假设需要生成长度为 T 的文本序列，则对于生成文本的奖励值 ![[公式]](https://www.zhihu.com/equation?tex=%5Ctilde%7BR_%7B%5Ctheta%7D%7D) 计算可以转化为如下形式：

![](https://www.zhihu.com/equation?tex=%5Ctilde%7BR_%7B%5Ctheta%7D%7D%5Cleft%28+s_%7B1%3At-1%7D+%5Cright%29+%3D+%5Cfrac%7B1%7D%7BN%7D+%5Csum%5Climits_%7Bi%3D1%7D%5E%7BN%7D+D%5Cleft%28s_%7B1%3At-1%7D+%2B+y%5Ei+%5Cright%29%2C+%5Cquad+y%5Ei+%5Cin+MC_%7B%5Ctheta%7D%5Cleft%28s_%7B1%3At-1%7D+%3B+N+%5Cright%29)

这里要说明几点，假设需要生成的序列总长度为 ![[公式]](https://www.zhihu.com/equation?tex=T) ， ![[公式]](https://www.zhihu.com/equation?tex=s_%7B1%3At-1%7D) 是指先前已经生成的部分序列（在**RL**中可视为当前的状态），通过蒙特卡洛搜索得到 ![[公式]](https://www.zhihu.com/equation?tex=N) 种后续的序列，尽管文本生成依旧是逐词寻找期望奖励最大的Action（下一个词），判别器**D**还是以整句为单位对生成的序列给出得分 ![[公式]](https://www.zhihu.com/equation?tex=Reward) 。

在新一代的判别器 ![[公式]](https://www.zhihu.com/equation?tex=D_%7Bnext%7D) 训练之前，生成器 ![[公式]](https://www.zhihu.com/equation?tex=G) 根据当前判别器 ![[公式]](https://www.zhihu.com/equation?tex=D) 返回的得分不断优化自己：

直到生成器**G**生成的文本足以乱真的时候，就是更新训练新判别器的时候了。一般来说，判别器**D**对生成序列打出的得分既是其判断该序列为真实样本的概率值，按照原版GAN的理论，判别器**D**对于 *real/fake* 样本给出的鉴定结果均为 ![[公式]](https://www.zhihu.com/equation?tex=0.5) 时，说明生成器**G**所生成的样本足以乱真，那么倘若在上面的任务中，判别器屡屡对生成样本打出接近甚至高出 ![[公式]](https://www.zhihu.com/equation?tex=0.5) 的得分时，即说明判别器**D**需要再训练了。在实做中为了方便，一般等待多轮生成器的训练后，进行一次判别器的训练。

![](https://pic3.zhimg.com/80/v2-ffec6641001bca75c3b73574c1202cf6_720w.png)

SeqGAN的提出为GAN用于对话生成（Chatbot）完成了重要的铺垫，同样起到铺垫作用的还有另外一个GAN在图像生成领域的神奇应用——Conditional GAN[18~19]，有条件的GAN，顾名思义就是根据一定的条件生成一定的东西，该工作根据输入的文字描述作为条件，生成对应的图像，比如：

![](https://pic3.zhimg.com/80/v2-05d72e64c6b2096bb10c997f7fe642ea_720w.png)

对话生成可以理解为同样的模式，上一句对话作为条件，下一句应答则为要生成的数据，唯一的不同是需要生成离散的文本数据，而这个问题，SeqGAN已经帮忙解决了。综上，我自己给它起名：Conditional SeqGAN。根据**4.1**节以及本节的推导，Conditional SeqGAN中的优化梯度可写成：

![](https://www.zhihu.com/equation?tex=%5Cnabla%5Ctilde%7BR_%7B%5Ctheta%7D%7D+%3D+%5Cfrac%7B1%7D%7B%5Coverline%7BN%7D%7D+%5Csum%5Climits_%7Bi%3D1%7D%5E%7B%5Coverline%7BN%7D%7D+D%5Cleft%28a%5Ei%2C+x%5Ei+%5Cright%29+%5Cnabla%5Clog+P_%7B%5Ctheta%7D%5Cleft%28x%5Ei+%5Cmid+a%5Ei+%5Cright%29)

不难看出，此式子与**4.1**节中的变化梯度仅一字之差，只是把“打分器”给出的奖励得分 ![[公式]](https://www.zhihu.com/equation?tex=R%28a%5Ei%2C+x%5Ei%29) 换成了鉴别器认为生成对话来自真人的概率得分 ![[公式]](https://www.zhihu.com/equation?tex=D%5Cleft%28a%5Ei%2C+x%5Ei+%5Cright%29) 。看似差别很很小，实际上 **RL + GAN** 的文本生成技术与单纯基于**RL**的文本生成技术有着本质的区别：在原本的强化学习对话生成中，虽然采用了AI互相对话，并设定了 *jugle* 进行打分，但这个 *jugle* 是预训练好的，在对话模型的训练过程当中将不再发生变化；**RL + GAN** 的文本生成乃至对话模型则不同，鉴别器**D**与生成器**G**的训练更新将交替进行，此消彼长，故而给出奖励得分的鉴别器**D**在这里是动态的（dynamic)。

![](https://pic3.zhimg.com/80/v2-fd0734aa7da9a9e0d6974674add48616_720w.png)

RL+ GAN 利用强化学习中的 ![[公式]](https://www.zhihu.com/equation?tex=Reward) 机制以及 **Policy Gradient** 等技术，巧妙地避开了 GAN 面对离散数据时梯度无法 BP 的难题，在使用强化学习的方法训练生成器**G**的间隙，又采用对抗学习的原版方法训练判别器**D**。  Conditional SeqGAN 对话模型的一些精选结果中，RL+ GAN 训练得到的生成器时常能返回一些类似真人的逼真回答（我真有那么一丝丝接近“恐怖谷”的感受)。

## 5. 一些细节 + 一些延伸

上文所述的，只是 RL + GAN 进行文本生成的基本原理，大家知道，GAN 在实际运行过程中任然存在诸多不确定因素，为了尽可能优化 GAN 文本生成的效果，而后发掘更多 GAN 在 NLP 领域的潜力，还有一些值得一提的细节。

### 5.1. Reward Baseline：奖	Bias

## References

- [Role of RL in Text Generation by GAN(强化学习在生成对抗网络文本生成中扮演的角色)](https://zhuanlan.zhihu.com/p/29168803)