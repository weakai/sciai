# Towards efficient discovery of green synthetic pathways with Monte Carlo tree search and reinforcement learning

利用蒙特卡罗树搜索和强化学习，实现绿色合成路径的高效发现

## 摘要

Computer aided synthesis planning of synthetic pathways with green process conditions has become of increasing importance in organic chemistry, but the large search space inherent in synthesis planning and the difficulty in predicting reaction conditions make it a significant challenge.
绿色工艺条件下的合成路径计算机辅助合成规划在有机化学中越来越重要，但其固有的大搜索空间和反应条件预测的困难使其成为一个重大挑战。

We introduce a new Monte Carlo Tree Search (MCTS) variant that promotes balance between exploration and exploitation across the synthesis space. Together with a value network trained from reinforcement learning and a solvent-prediction neural network, our algorithm is comparable to the best MCTS variant (PUCT, similar to Google's Alpha Go) in finding valid synthesis pathways within a fixed searching time, and superior in identifying shorter routes with greener solvents under the same search conditions.
我们引入了一种新的蒙特卡罗树搜索(MCTS)变体，它促进了整个合成空间的探索和开发之间的平衡。结合强化学习训练的值网络和溶剂预测神经网络，我们的算法在固定的搜索时间内找到有效的合成路径，可以与最好的MCTS变体(PUCT，类似于谷歌的Alpha Go)相媲美，在相同的搜索条件下，用更环保的溶剂识别短途路线的能力更强。

In addition, with the same root compound visit count, our algorithm outperforms the PUCT MCTS by 16% in terms of determining successful routes. Overall the success rate is improved by 19.7% compared to the upper confidence bound applied to trees (UCT) MCTS method.
此外，在相同的根复合访问次数下，我们的算法在确定成功路径方面比PUCT MCTS算法的性能好16%。总体而言，与应用于树(UCT) MCTS方法的上置信度界限相比，该方法的成功率提高了19.7%。

Moreover, we improve 71.4% of the routes proposed by the PUCT MCTS variant in pathway length and choices of green solvents. The approach generally enables including Green Chemistry considerations in computer aided synthesis planning with potential applications in process development for fine chemicals or pharmaceuticals.
此外，在路径长度和绿色溶剂的选择上，我们对71.4%的PUCT MCTS变种提出的路线进行了改进。这种方法通常能够在计算机辅助合成计划中考虑绿色化学，并在精细化学品或药品的工艺开发中具有潜在的应用。

## Introduction

The goal of computer aided synthesis planning (CASP) has traditionally been to identify synthesis pathways to desired compounds from buyable building-block chemicals through retrosynthesis.
传统上，计算机辅助合成计划(CASP)的目标是通过反合成从可买到的构建块化合物确定合成所需化合物的途径。

Retrosynthetic analysis is a procedure in which a target molecule is broken into precursors recursively until all precursors are commercially available (success) or the process reaches the termination criteria (failure).
反合成分析是一个过程，目标分子递归地分解成前体，直到所有的前体都可以在市场上买到(成功)或过程达到终止标准(失败)。

The idea of reaction codification and CASP was introduced in Vl´eduts's visionary paper in 1963.
反应编码和CASP的想法是在Vl´edts 1963年的一篇有远见的论文中提出的。

Ever since the First logical retro-synthetic route planning based on a computer program demonstrated by Corey et al. in the 1960s, the field of CASP has been evolving rapidly along with the development of large comprehensive reaction databases.
自20世纪60年代Corey等人演示的基于计算机程序的第一个逻辑逆合成路线规划以来，CASP领域随着大型综合反应数据库的发展而迅速发展。

As examples, Synthia (formerly Chematica), has successfully designed synthesis routes for many medicinally important targets, and Coley et al. has demonstrated that with expert chemistry input CASP synthetic routes can be translated successfully into recipes for execution on a robotic platform.
例如，Synthia(前身为Chematica)已经成功地为许多重要的医学目标设计了合成路线，Coley等人已经证明，通过专家的化学输入，CASP合成路线可以成功地转化为配方，在机器人平台上执行。

Existing CASP programs have primarily focused on the success rate and the chemical diversity of routes, but rarely have taken Green Chemistry principles into considerations, e.g., routes with shortest possible length using less toxic and fiammable solvents. However, it is becoming increasingly important to select routes that conform to green chemistry requirement in addition to finding synthetic pathways to buyable starting compounds.
现有的CASP项目主要关注成功率和路线的化学多样性，但很少考虑到绿色化学原理，例如，路线尽可能短的长度，使用较少的有毒和易燃溶剂。然而，除了寻找可购买的起始化合物的合成途径外，选择符合绿色化学要求的途径也变得越来越重要。


One major challenge for an efficient CASP algorithm to include green chemistry considerations is limitations in chemical space search algorithms.
一个包含绿色化学考虑的有效CASP算法的主要挑战是化学空间搜索算法的局限性。

The search algorithm is the core of synthesis planning as it determines the success rate and the route quality of the proposed synthesis pathways. Heuristic best first searches (BFS) or depth first searches (DFS) use handwritten heuristic functions to evaluate positions in the search tree as they explore the synthesis planning search space.
搜索算法是综合规划的核心，它决定了所提出的综合路径的成功率和路径质量。启发式最佳优先搜索(BFS)或深度优先搜索(DFS)在探索综合规划搜索空间时使用手写启发式函数来评估搜索树中的位置。

However, given the large search space of chemical reactions, the searching efficiency of BFS/DFS is not high enough for good CASP performance. In addition, it is difficult to define strong heuristics for BFS/DFS CASP. To tackle this challenge, Segler et al. showed that Monte Carlo Tree Search (MCTS), a search scheme that demonstrated superhuman performance in playing the game of Go, is able to find buyable pathways with much higher success rate than traditional heuristic BFS.
但由于化学反应的搜索空间较大，BFS/DFS的搜索效率不够高，无法实现良好的CASP性能。此外,很难定义的启发式BFS / DFS CASP。应对这一挑战,赛格勒等人表明蒙特卡洛树搜索（MCTS，证明了在围棋游戏中大放光彩的算法），超人的表现能够找到可买的途径与更高的成功率比传统启发式石。

Segler et al. also innovatively demonstrated that it was possible to learn the rules of chemical synthesis for a specific domain by using data.
Segler等人也创新性地证明了通过使用数据来学习特定领域的化学合成规则是可能的。

Additionally, Kishimoto et al. demonstrated that the MCTS variant used by Segler et al. significantly outperformed depth-first proof-number (DFPN) search in terms of success rate in the domain of retrosynthesis. More recently, Chen et al. introduced Retro*, a neural-based tree search method also demonstrating advanced performance.
此外，Kishimoto等人证明，Segler等人使用的MCTS变体在反合成领域的成功率方面显著优于深度优先证明数(DFPN)搜索。最近，Chen等人推出了Retro*，这是一种基于神经的树状搜索方法，也展示了先进的性能。


The MCTS variant used by Segler et al. is similar to Google's Alpha Go algorithm. Though it has great search success rate, the method does not take green chemistry into account.Segler等人使用的MCTS变体类似于谷歌的Alpha Go算法。该方法虽然有很高的搜索成功率，但没有考虑到绿色化学。

Schreck et al. used another variant of MCTS, Upper Confidence bound applied to Trees (UCT), in a reinforcement learning
approach to find synthesis pathways with as few buyable precursors as possible.
Schreck等人使用了MCTS的另一种变体（将上置信度约束应用于树UCT），在一种强化学习方法中，以找到具有尽可能少的可购买前体的合成途径。

However, the UCT method is prone to have a lower success rate in finding pathways than the Alpha Go-like PUCT (predictor + UCT) MCTS variant used by Segler et al. An efficient search scheme that maintains high search success rate and favors green chemistry routes is still missing.
然而，与Segler等人使用的Alpha Go-like PUCT (predictor + UCT) MCTS变体相比，UCT方法在寻找通路方面的成功率较低。目前仍缺乏一种高效的搜索方案，既能保持较高的搜索成功率，又能优选绿色化学路径。


A critical challenge for green chemistry CASP is the difficulty of evaluating chemical compounds and reactions in the search tree.
绿色化学CASP面临的一个关键挑战是很难在搜索树中评估化合物和反应。

Evaluation of compounds and reactions in the sense of green chemistry requires a fast-computing model to predict the
conditions, such as solvents, catalysis and reaction temperature, of a given reaction.
在绿色化学的意义上评价化合物和反应需要一个快速计算模型来预测条件，如一个给定的反应的溶剂，催化和反应温度。

Struebing et al. used quantum mechanical (QM) calculations to effectively find solvents that can accelerate certain reactions. But the high computational cost of QM calculations, makes such an approach infeasible for CASP tree search.
斯特鲁宾等人利用量子力学(QM)计算有效地找到了能够加速某些反应的溶剂。但由于QM计算的高计算成本，使得这种方法在CASP树搜索中不可行。

Recent data-driven condition prediction models could potential be sufficiently fast for CASP. As examples, Marcou et al. demonstrated an expert system to predict the catalysts and solvents for Michael additions and Gao et al. developed a data-driven neural network model to predict the reaction conditions with high accuracy.
最近的数据驱动的条件预测模型可能足够快的CASP。作为例子，Marcou等人演示了一个专家系统来预测Michael添加剂的催化剂和溶剂，Gao等人开发了一个数据驱动的神经网络模型来预测反应条件，具有较高的准确性。

So far, such models have not been incorporated into the tree search algorithms to guide and evaluate the searches.
到目前为止，这些模型还没有被整合到树搜索算法中来指导和评估搜索。


Additionally, the evaluation of the compounds in a CASP search tree could be facilitated by reinforcement learning (RL).
此外，强化学习(RL)可以促进CASP搜索树中化合物的评价。

RL has been frequently used in solving games and evaluating game positions. The similarity between the synthesis plan- ning problem and strategic games, suggests that RL would be a suitable method for evaluating chemical compounds as demonstrated by Schreck et al. in their application of RL to assess the cost of compounds in retrosynthetic analysis.
RL常用于求解博弈和评估博弈位置。合成计划问题和战略博弈之间的相似性表明，RL是一种适合于评价化合物的方法，正如Schreck等人在反合成分析中应用RL评价化合物成本时所证明的那样。

In this work, we tackle the aforementioned challenges by proposing a more efficient MCTS variant that incorporates the condition prediction model and RL.
在这项工作中，我们通过提出一种更有效的MCTS变体来解决上述挑战，该变体将条件预测模型和RL结合在一起。

This MCTS variant, modified UCT with “dynamic c”, enables tuning the balance between exploring new reaction templates and exploiting known templates dynamically during tree search. Specifically, we promote an active search by adjusting the “c” coefficient along with the tree expansion.
这个MCTS变体，用“动态c”修改了UCT，能够在树搜索过程中调整探索新的反应模板和动态利用已知模板之间的平衡。具体来说，我们通过随着树的展开调整“c”系数来促进主动搜索。

Because of the forced exploration by our method, the modified UCT with dynamic c algorithm is able to significantly boost the success rate compared to the original UCT algorithm proposed by Kocsis et al.
由于我们的方法的强制探索，与Kocsis等人提出的原始UCT算法相比，采用动态c算法的改进UCT算法能够显著提高成功率。


A value network trained by MCTS self-play RL is used explicitly as the look-ahead mechanism to evaluate the “synthesis easiness” of each compound in order to steer the tree expander towards short route length and high success rate.
采用MCTS自演RL训练的价值网络作为前瞻性机制，评估每个化合物的“合成易度”，以引导树扩展器向短路径长度和高成功率方向发展。

A policy network is applied implicitly to narrow down the beam of search. However the exact value of the policy function is not used in the UCT formulation of MCTS, making our algorithm more robust to an imperfect policy network. In addition, each reaction in the proposed routes is evaluated by the “greenness” of neural network predicted solvents.
隐式地应用策略网络来缩小搜索的范围。然而，在MCTS的UCT公式中没有使用策略函数的精确值，使得我们的算法对不完美的策略网络具有更强的鲁棒性。此外，还通过神经网络预测溶剂的“绿度”对所提出的反应路线进行了评价。

We use automatically extracted templates (patterns of reaction rules) as other retrosynthesis efforts to generate a full buyable path with multistep reactions.
我们使用自动提取的模板(反应规则的模式)作为其他retrosynthesis的efforts，以生成一个完整的可购买的路径与多步反应。

Although many recent efforts have focused on developing template free synthesis planning, they are primarily addressing single step conversion and template-free full synthesis planning is still emerging.
虽然最近的许多努力集中于开发无模板综合规划，但它们主要是解决单步转换，而且无模板的全面综合规划仍在出现。


We show that our new MCTS variant is able to suggest shorter pathways with greener solvents than the suggestions by the PUCT MCTS without sacrificing the success rate of route searching. At the same time, the new MCTS also demonstrates significantly higher success rate compared to the original UCT MCTS.
我们发现，在不牺牲路径搜索成功率的情况下，我们的新MCTS变体能够比PUCT MCTS提出的使用更绿色溶剂的路径更短。与此同时，新MCTS的成功率也明显高于原MCTS。

Although we only consider route length and solvent greenness in this work, the method is generally applicable for other green chemistry considerations, e.g. milder reaction temperature and more economical catalysts.
虽然本研究只考虑了路线长度和溶剂绿度，但该方法也适用于其他绿色化学考虑因素，如反应温度更温和、催化剂更经济等。

## Results and discussion

