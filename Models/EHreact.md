# EHreact: 用于酶促反应模板提取和评分的扩展 Hasse 图

## ABSTRACT

数据驱动的计算机辅助合成规划利用大型数据库中的有机或生物催化反应在过去十年中获得了越来越多的兴趣，激发了大量工具的开发，以提取、应用和评分一般反应模板。酶反应的反应规则的生成尤其具有挑战性，因为酶之间的底物混杂性不同，导致规则特异性的最佳水平和包含的原子的最佳数量在酶之间有所不同。这使得从数据库中自动提取数据变得复杂，并促进了手工管理反应规则集的创建。在这里，作者介绍了EHreact，一个纯粹的数据驱动的开源软件工具，从已知的反应中提取和评分规则，这些反应由一种酶在不需要专家知识的情况下以适当的特异性水平催化。EHreact基于假想过渡结构中常见的子结构，将反应规则提取并分组为树状结构、Hasse图。每个图可以用来输出一个或一组反应规则，以及计算新衬底的概率由给定的酶处理通过推断信息已知的活性的酶反应和分组的模板树。EHreact启发式预测给定酶在新底物上的活性，在准确性和功能上优于现有方法。

##  INTRODUCTION

如今，生物催化转化包括一个不断扩大的化学、立体和区域选择反应工具箱。利用酶催化反应具有反应条件温和、介质为水溶剂、多步合成中不同反应步骤的相容性以及减少对保护基团的需求等优点。大多数酶至少在某种程度上是混杂的，或者可以被改造来接受一种新的底物，因此生物催化转化的可能范围足够大，足以引起合成化学家的兴趣。在过去的几十年里，大量的新型酶级联反应证明了这一点。因此，在合成医药中间体或精细化学品等方面，酶转化为有机反应提供了一个有前途的、生态友好的替代品。

在实践中，当设计一个途径时，适度混杂的酶通常是首选的，在那里可以通过定向进化增加少量的活性。酶既能表现底物混杂性，也能表现反应混杂性，但在生物逆合成中，通常只有底物混杂性被利用。

底物混杂性是指在非原生底物上催化原生反应的能力，而反应混杂性则是指催化非原生反应的能力。在下面，我们将只提及底物的混杂性。

为应对酶合成路径规划的挑战，近年来开发了多种计算工具，用于通用生物逆合成规划、酶的筛选、代谢通路探索和反应规律提取。这些工具通常通过识别反应中心，将原子和键的变化编码成反应规则，并根据一组标准对新基质进行相同转化的可行性进行评分，从而从已知的反应中提取催化转化。

在这里，一个关键的挑战是提高所使用的评分函数的准确性，从而正确地对那些预期可行的反应进行排序，而不是那些最有可能没有被所需酶催化的反应。然而，一些工具认为，如果一个反应在期望的特异性水平上满足反应规则，则该反应是可行的，而另一些工具则根据与已知反应或底物的化学相似性，通过指纹向量对转换的可行性进行评分。然而，依赖于相似性或反应规则特异性的方法缺乏通用性的酶和专一性的酶之间的区别，即它们缺少对酶混杂性的描述，正如Jeffryes等人最近指出的那样。通过将数据库中的每个反应作为独立的数据点处理，相同酶的已知底物之间的相关性将丢失，并据此估计酶的混杂性和底物范围。另一方面，正如Nath和Atkins提出的，在已知底物的基础上描述酶的混杂性，可能会受到数据缺乏的影响。事实上，一种研究不足的酶，只有一组有限的已知底物，可能被错误地认为是高度特异性的。然而，即使对滥交的一个不完美的预测也增加了预测反应的可行性的准确性。此外，随着BRENDA、RHEA、或KEGG32等酶反应数据库的增长，这种限制变得不那么严重。在BRENDA的情况下，跨底物集合信息的能力尤其正确，在这里，酶被报道在自然和非自然底物上有各种各样的活动。

缺少对酶混杂性的描述进一步影响了提取反应模板的质量。即，从生物催化反应数据库中提取的反应规则的特异性，即模板中包含的原子数，通常是由一个单一的用户定义值来设置，对特定的和混杂的酶一视同仁。一些手工编辑的酶反应规则集提供了酶特异性的规则通用性，但目前还没有方法自动检测酶的混杂性并提取相应的反应规则。

因此，我们认为需要一种数据驱动的方法来提取不同特异性水平的酶反应模板，以及在指纹相似度以外的标准上对新的查询进行评分。考虑到一种酶的估计混杂性和从已知底物推断反应中心周围化学结构的多样性。

在这篇文章中，我们提出了一种新的方法来计算酶反应模板和预测其在非天然底物上的适用性。我们根据已知底物组施加的特异性水平提取反应模板，并将其排列成树状结构(分子片段的Hasse图)，以便估计酶的混杂性和底物范围。通过考虑不同的整体相似性和多样性度量，并将查询基板的结构与已知基板中保守的子结构进行比较，在每个模板树的基础上对新的基板进行评分。我们的开源软件允许各种不同的查询，包括对特定反应的评分，对底物上可能的反应的建议和排名，包括区域选择性和底物的选择，或者在产物未知的情况下对底物进行评分，而不是对全部反应进行评分。因此，我们提供了一个有价值的工具来描述和预测酶的反应，这个工具可以在Github上免费获得。

本文的其余部分组织如下。提取算法，以及所使用的评分函数和文献数据集的准备的细节，在方法部分中进行了解释。然后，我们分析了不同数据库中每种酶的已知反应数量，在一个小示例上展示了模板提取程序，并比较了评分程序在活性预测、区域选择性和共底物建议方面的性能；在筛选数据的实验中，对比了基于指纹的方法。结果和讨论展示了BRENDA酶数据库中的反应。结束语在结论部分给出。

## METHODS

EHreact是用Python实现的，可以作为单独的命令行应用程序使用，也可以作为Python包导入。EHreact使用RDKit处理分子,Graphviz描述模板树。

### 输入的格式和转换，生成虚拟过渡结构（ITS）

EHreact可以运行在两种不同的模板树生成模式:以反应作为输入(默认，推荐)或仅反应物(单一底物)。

在标准设置下，即在反应模式下，EHreact采用一个平衡的、原子映射的反应smile作为输入，它必须包括显式的氢原子。如果原子映射未知，则通过反应解码器(RDT)自动计算，这是一种最先进的酶反应原子映射工具在这种情况下，非原子映射的smile反应既可以有氢也可以没有氢。在本研究中，通过RDT对不同酶类的原子映射的准确性在支持信息中给出。由于正确的原子映射对EHreact的性能是不可或缺的，我们建议通过RDT或任何其他工具预先计算映射，并在必要时对它们进行修正。

每个反应转化为对应的ITS，closely related to the condensed graph of reaction,通过识别反应过程中改变的原子和键。按照Coley等人概述的程序，我们考虑了电荷的变化，杂交，自由基数量，芳香性或成键数量和顺序。对反应无原子贡献的分子，如试剂，则略去。反应的ITS是反应物和生成物的topological superposition，只有反应物和生成物有键，反应物和生成物可以同时出现。因此，它描述的是反应物和生成物之间的人工过渡态(而不是真正的过渡态或机理)的图形。图1显示了通过乳酸氧化酶(EC 1.1.3.2)氧化乳酸的典型ITS。这种虚拟过渡结构虽然已经被发现了几十年，但最近引起了人们对解析反应数据库、预测结构-活动关系和开发反应描述符的兴趣。

我们注意到EHreact中提取的模板没有考虑手性，而是在评分算法中处理。在评分级别而不是模板级别处理立体化学有许多优点。首先，只在部分输入反应中指定立体中心的反应不会导致不同的模板，因此在省略立体信息的情况下在模板树中分支。这是很重要的，因为酶数据库包含的条目缺少选择反应的立体中心，但在评分时，通常可以从同一酶催化的其他反应中推断出正确的立体化学。在模板级别，立体中心的信息缺失会导致对一组反应提取不同的模板，使评分阶段的比较变得困难。其次，并不是每一种酶都具有完全的立体选择性，这使得在模板水平上考虑所有可能的立体异构体是有利的，然后在评分时过滤这些立体异构体，以考虑酶的选择性。

在单底物模式下，EHreact以SMILES字符串作为输入(例如，“CC(O)C(=O)[O-]”为乳酸盐)，可以输入氢原子或不输入氢原子。由于在此模式中没有指定产物，因此可以另外输入一个seed，以SMILES格式进行最大公共子结构搜索，以帮助算法聚焦于分子的相关部分。对于乳酸的氧化(图1)，一个有意义的种子是“C([H])O[H]”，即乳酸氧化酶转化为酮的仲醇。如果没有指定种子，则算法使用所有输入底物中的最大公共子结构作为种子。

在反应和单底物模式中，可以指定多个种子或反应中心来描述催化略有不同转化的酶，只要它们是互斥的。

### 模板树的生成

在确定反应中心(或底物模式下的种子的原子构成)后，根据已知反应或底物的结构，逐步展开模板。在反应模式下，其结构为ITS pseudo-molecule，初始原子构成反应中心。在单底物模式下，结构是真实的分子，即输入底物，种子可以手动给出，也可以从最大共同子结构中自动推断出来。由于EHreact是缺省的反应模式，我们将在下面使用ITS nomenclature。图2给出了模板树生成的概述。

该算法迭代地向模板添加更多信息，创建一个新的、更具体的模板。为了达到这个目的，所有没有指定邻居的原子都被列入当前模板的候选名单。例如，在图1中，反应中心(第一次迭代时的当前模板)只能在C3原子上展开，这是唯一一个没有指定邻居的原子。如果只有一个原子入围，则新模板由当前模板和入围原子的所有相邻键和原子组成。如果有多个原子入围，该算法会搜索尽可能多的原子组合，以得到完全相同的新的、放大的模板。图3显示了这个过程的一个例子，一组两个已知的反应，其中，从候选的原子1、7和8，只有原子1和8在所有的伪分子中有相同的邻居。因此，新的模板是由当前的模板和邻近的键和只有原子1和8的原子形成的。模板与伪分子可能会有多个匹配，在这种情况下，将探索所有选项，并保持导致所有伪分子中最大可相互扩展原子的匹配。如果可能有多种组合，则氢原子较少的组合更受青睐。如果只知道一个伪分子，所有入围的原子都将被扩展，这类似于基于直径的规则提取。如果没有展开导致所有伪分子的一个适用模板，那么所有入围的原子都会展开，从而导致生成的模板树出现分支，从当前模板中出现多个新模板。如果一个展开式在一个环中增加了一个原子，那么整个环就在一个展开式步骤中增加了。

生成的模板保存在一个模板树中，其中每个新模板都附加到它的父模板，即它产生的模板。每个模板只能有一个父模板，但可以有一个或多个子模板。树中没有子节点的节点只是一个输入的伪分子，其中所有的原子都包含在模板中，在短列表中不留下任何原子，因此没有更具体的模板可以作为子节点附加。在数学上，这样的图称为Hasse图，它是一种使用部分顺序对一组对象进行排序和描述的方法。在文献中已经提出了适用于分子中亚结构排序的Hasse图。由于我们不仅将父节点和子节点的信息保存到图中，而且还将大量的附加特性保存到图中，所以我们将生成的模板树称为“扩展的Hasse图”。图表以自定义Python类的形式保存到磁盘，其中所有模板都保存为RDKit分子。另外，EHreact生成Hasse图的文本表示，以及可选的PNG图片。支持信息中给出了一个输出示例。

综上所述，给定一个反应，则从反应中心提取不同直径的模板，创建一个没有分支的线性Hasse图。但如果已知多个反应，则算法利用它们之间的相互结构信息。在这两种情况下，模板树及其叶子节点的许多属性都被预先计算，以加快查询反应或底物的后续评分。这种方法的创新点是在反应中心加入原子和化学键，利用所有已知反应中保守的子结构，而不是预先设定的反应中心半径。因此，我们含蓄地假设，保守的亚结构表明了相应结构对反应机制或与酶活性袋中的氨基酸的特定相互作用的重要性。酶通常只与某些类型的底物发生反应，而化学试剂通常只针对一个官能团，因此推断已知底物中重要的亚结构的信息对生物催化转化特别相关。

### 在模板树上的查寻

图4示意地描述了如何在扩展的Hasse图上查询和评分新的底物或反应。有三种模式可以在给定的模板树上为反应或底物打分。首先，如果Hasse图是在反应模式下生成的，那么可以输入一个反应SMILES(最好是原子映射的，否则通过RDT自动映射)，即一个特定的反应，以获得给定的酶是否会催化查询反应的评分。如果查询的反应中心从未出现在模板树中，则得分为0;否则，将按照本文后面指定的方式计算。其次，可以使用SMILES格式的单个底物作为在反应模式下生成的Hasse图的查询。在这种情况下，基板与树中最小模板的反应物片段相匹配。如果找到匹配，EHreact就会确定是否有共底物缺失(对于非单分子反应)，以及转化是否会发生在分子的不同部位，并计算转化的所有可能产物。对于只有一个可能产物的单分子反应，将底物转化为相应的反应ITS并打分。对于可能的区域选择性反应，即不同的可能产物，每个可能性都被翻译成ITS并单独评分。在非单分子反应中，对于缺失的辅底物，算法能否检测到辅底物的类型是必要的(例如，在胺转移反应中，如果给定的底物是一种胺受体，那么要推断出辅底物是胺给体)并创建一个反应发生在树的每个辅底物,创建一个或多个可能的同期,每一项都是单独评分的。第三，可以用SMILES格式指定一个或多个底物，并省略辅底物搜索，因此如果缺少一个或多个辅底物，则得分为零。由于区域选择性，多个可能的产物被检测到，每个反应被单独评分。如果已知所有反应物，这种模式有利于非单分子反应，因此不需要寻找辅底物。

如果Hasse图是在单底物模式下产生的，则只能通过单底物查询。在这种情况下，查询基板转换后的产物仍然未知。如果图中的第一个子结构(种子)出现在查询中的多个位置，则会计算多个分数，因此该模式仍然提供一些区域选择性度量，但它不能提出辅模板或识别转换的产物。如果产物未知，建议使用此功能快速评分相关底物；对于所有其他情况，我们建议以反应模式进行训练，以便在查询和评分一个新的底物或反应时可以充分利用EHreact的能力。

### 打分函数

略

### 数据准备

为了验证评分函数的有效性，我们从手工文献中提取了一系列关于各种酶的底物范围的实验研究，以及有机偶联反应的研究，以测试EHreact对有机、非酶促反应的性能。每项研究都报告了在整个研究中一致的反应条件下，特定底物上酶/催化剂的产率或活性。通过手工指定一个阈值，每个数据集产生大约10 - 40%的活性反应，将每个反应分为活性反应和非活性反应(阈值列于表1);数据可以在Github存储库中找到。由于产率和活性的分布在不同数据集之间差异很大，阈值大致选择在每个数据集分布的平均值加上1个标准差附近，但不同的阈值可以获得可比的结果。对于有机偶联反应，由于数据集的大小，选择较大的阈值来限制活性反应的数量。省略了含有未知产物的底物，以及没有底物被标记为活性的酶。剩余底物和酶的数量也列在表1中。所有的反应都通过RDT进行原子映射，并通过对每个类的所有反应运行EHreact进行校正，用一个偏差的反应中心标记反应，并手动校正原子映射。初始正确和不正确原子映射的数量可在支持信息中找到。所有使用的酶/反应类的完整列表，包括各自参考文献中使用的标识符，可在支持信息中找到。

为了统计报道的各种EC类和酶的反应数量，我们进一步分析了BRENDA，RetrorRules，和RHEA，如下所述。

为了recover各种EC类的酶反应，使用2019年12月导出的数据库文本下载对BRENDA数据库进行了解析。为了从底物和产物名称中解析SMILES字符串，we search portal to 询BRENDA配体，以导出数据库中包含的各种配体的Inchi值。这并不包括所有的配体。为了补充从搜索工具中找到的配体，所有剩余的化合物都通过PubChem和Opsin名称解析器进行查询。对于下游分析，去除所有含有未解化合物的反应条目，并过滤每个EC类中的重复项。代码，包括下载必要文件的说明，可以在Github上找到。EC的反应1.1。X (X = 1.145, 1.149, 1.209, 1.213, 1.239, 1.265, 1.283, 1.50, 1.6, 1.64, 1.72, 3.2, 3.6, 3.9), EC 2.6.1。X (X = 1, 12, 14, 15, 18, 2, 27, 28, 36, 39, 40, 42, 44, 5, 51, 57, 64, 73)和EC 4.1.3.42通过RDT进行原子图绘制，并如上文所述手动修正，作为辅底物建议、区域选择预测和图表构建的测试用例。

来自RetroRules(版本rr02基于MNXref(版本rr02基于MNXref版本3.0,57兼容RetroPath2)的反应，在去除重复的反应条目(RetroRules将多底物反应拆分为多个反应)后，以最小规则直径在正向确定每个4位酶EC号单底物规则。

对于RHEA，将反应id与UniProt和SwissProt中各自的氨基酸序列交联，以确定每种酶(非EC类)可获得的唯一反应注释的数量。因为RHEA遵循分层注释技术，如果适当，特定反应与更广泛的主反应类相关联。如果一种酶同时标注了它的特定反应类和主反应类，则从分析中删除主反应类，以避免重复计数。

## RESULTS AND DISCUSSION

### 示例模板树构建

为了阐明输入的反应如何转变为ITS，以及如何围绕反应中心的共同子结构进行迭代搜索，讨论了4-羟基-2-氧戊二酸裂解酶(EC 4.1.3.42)。为此BRENDA列出了3种已知底物：4-羟基-2-氧戊二酸、4-羟基-2-牛丁醛和草酰乙酸。底物和4-羟基-2-氧戊二酸裂解酶催化的反应如图5a所示。这种酶使靠近羟基的碳-碳键分裂。这三种情况下的产品分别是丙酮酸、乙醛酸、甲醛和二氧化碳。例如，用文献方法提取反应模板，包括离反应中心一个键以下的所有原子(一种常见的选择)，将创建三种不同的模板(图5b)，它们都缺少从已知反应传递而来的互信息。反应的ITS显示大的公共子结构(图5c)。底物一侧是高度保守的，即形成丙酮酸侧(连接在C:10上)，而另一边(连接在C:4上)在结构和大小上是不同的。这表明丙酮酸侧是必需的，以很好地适应酶的活性袋，总而言之参与了反应机理。事实上，对4-羟基-2-氧戊二酸裂解酶的机理研究表明，活性袋中的氨基酸与底物的丙酮酸侧存在特定的相互作用，以及同侧的体积限制。EHreact利用已知反应之间的互信息，以迭代的方式将保守子结构中的原子添加到最小反应模板中(图5d中的第一个模板)。在每一步中，算法只添加原子及其对应的键，这些原子在所有的反应中都是守恒的，是模板中当前原子直接相邻的原子，最终得到图5d中的第四个模板，它适用于所有输入反应，是最具体的。它标识4-羟基-2-氧化戊二酸裂解酶作用于底物表现出重要的丙酮酸C−C键旁边的一部分的分割，没有指定另一边的分子。因此，完美地对应专家知识模板制作的活性口袋和机制系统。在将原子进一步添加到模板之后，图分为三个分支，其中两个分支直接指向叶节点(完全反应的ITS)，一个分支在结束于叶节点之前生成一个额外的模板。如果用户对单模板感兴趣，那么提取最具体的相互模板(图5d中的第四个模板)就足够了，并且与传统的模板提取方法相比具有优势，在传统的模板提取方法中，用户需要指定最适级别的特异性。

一般来说，本文提出的模板树提取过程在许多场景中都是有用的。例如，EHreact反应模式可用于从一组反应中提取单一、最特异但相互适用的模板。此外，在单底物模式下计算Hasse图有助于快速获得一组分子及其共同子结构和相似性的概览。EHreact的进一步应用可能是减少从酶和有机反应数据库中提取的模板的数量，而不失通用性，如图5所示。其中，EHreact为所有反应生成一个模板，而不是其他方法提取的三个不同模板。众所周知，提取模板的数量随着数据库中反应的数量而变化，而且即使在大型数据集中，大部分模板也只出现一次。因此，我们可以通过使用EHreact来提取基于公共子结构的模板，而不是基于反应中心附近固定数量的键，甚至可能利用模板树结构来加速将模板应用到分子上，从而减少模板的数量。在Hasse图中，如果缺少与最通用模板的匹配，则立即取消反应类型的资格，从而使计算机辅助合成计划更容易、更快。因此，EHreact模板应该有利于逆向合成应用，因为它们包含了一个小的、一致的和互斥的模板集。与基于半径的模板提取相比，每个酶类的通用性自动选择允许减少对较大分子的偏爱。EHreact模板进一步平衡，包括辅因子和辅底物，这使其适用于酶级联设计，包括辅因子回收，目前主要依赖于人工提取模板的研究领域。

### 酶反应数据集的构建

EHreact模板的质量和评分直接取决于反应的数量。反应的数量决定了每个模板树的大小和多样性，从而决定了它创建有意义的模板和分数的能力。

图6描述了不同数据库中每个EC类或酶的已知反应数量，即RHEA(与SwissProt和UniProt交联)、BRENDA和RetroRules。对于RHEA，反应id与UniProt和SwissProt中各自的氨基酸序列相关，并对每个酶计算独特的反应类别。与SwissProt或UniProt交联的结果几乎没有差异，下面我们只参考SwissProt的结果。对于BRENDA，具有有效的反应物和产品条目并且可以被解析为SMILES字符串的独特反应被计算为每个4位数的酶EC号。如果一个反应在BRENDA中同时发生在正向和反向方向，那么它只被计算一次。 对于RetroRules，每4位酶EC号的反应数是在去除重复反应条目后，以最小规则直径确定正向转化的反应数，RetroRules将多底物反应拆分为多个单底物规则。总的来说，76%和51%的EC和17%的酶分别与BRENDA、RetroRules和RHEA的不止一种反应相关，而EHreact可以潜在地利用这些反应之间的相互信息。尽管存在反应非常不同的情况，限制了EHreact的适用性，但我们相信基于反应集的模板提取对于酶促反应是一个可能且合理的选择，特别是对于BRENDA、与从独立的反应先例中提取反应模板相比，具有一定的优势。

### 实验数据的验证

作者比较了EHreact评分的能力，以识别在实验筛选研究中观察到的有希望的底物/酶组合，根据不同的相似性指标。为了达到这个目的，从文献中选择了9个最近的数据集，因为反应物和产物都是已知的(当前文献普遍的做法是只报告反应物，我们则不然)。8项研究包括酶转化，报告了不同的腈化酶、45种胺脱氢酶、46种醇脱氢酶、47种羧甲基转移酶、48种转氨酶、49种色氨酸合成酶、50种酰胺转移酶、51种脱卤酶52在不同底物上的活性。对7个有机C(sp2)−C(sp3)偶联的额外研究被用来展示EHreact在非酶转化上的性能。

---

Leave-one-out experiments were conducted on each data set, where the feasibility of each reaction (substrate/product/ enzyme combination) was evaluated by omitting it during the calculation of the template tree (one tree per enzyme) and subsequently calculating a score according to the above- discussed scoring scheme.
作者对每个数据集进行留一实验（Leave-one-out experiments），在计算模板树(每个酶一棵树)的过程中，对每个反应(底物/产物/酶组合)的可行性进行评估，然后根据之前评分方案计算得分。

We calculated scores using both EHreact and a traditional similarity metric (Tanimoto similarity on Morgan fingerprints of length 2048, radius 2, no features; see Supporting Information for other metrics and parameters).
使用EHreact和传统的相似度度量(Tanimoto, radius:2, len:2048, features:None)

Data points labeled as active according to the thresholds in Table 1 were treated as known reactions (inputs) for both EHreact and similarity-based approaches.
根据表1中的阈值标记为有效的数据点被视为EHreact和基于相似性的方法的已知反应(输入)。

The area under the curve (AUC) of the receiver operating characteristics was then evaluated per assay (for all leave-one-out experiments), as well as the binary classification accuracy at a threshold of 0.5 (which is close to the mean optimal threshold averaged over all enzymatic systems for both EHreact, 0.43, and similarity scores, 0.58; optimal thresholds for each system are given in the Supporting Information).
曲线下的面积(AUC)后评估每个接受者操作特征分析(分析实验),以及二元分类精度的阈值0.5。

Other thresholds and F1-scores are available in the Supporting Information, as well as AUC and Acc. for running EHreact in single-substrate mode (instead of reaction mode).
略

---

Table 2 lists the AUC and accuracy for the classification into active/inactive substrates in reaction mode.
表2列出了反应模式下活性/非活性底物分类的AUC和准确性。

In general, EHreact leads to a similar AUC but higher accuracies, with the differences being especially prominent for carboxyl-methyltransferases, transaminases, tryptophansynthases, and amidinotransferases. In these assays, substrates have high similarity scores between each other, but the enzymes only act on a very narrow range on substrates, i.e., are rather selective. In this case, a high similarity score does not necessarily ensure an enzyme being active toward a new substrate.
一般来说，EHreact可以得到类似的AUC，但准确率更高，在羧基-甲基转移酶、转氨酶、色氨酸合成酶和酰胺转移酶中差异尤其明显。在这些分析中，底物之间有很高的相似性分数，但酶只对底物起作用的范围很窄，也就是说，酶是相当有选择性的。在这种情况下，较高的相似性分数并不一定保证酶对新底物具有活性。

Figure7 depicts the similarity scores of a new substrate, as well as the similarities between known substrates, over the observed classification accuracies for all eight assays.
图7描述了在所有8种测定方法中观察到的分类准确度上，新底物的相似度得分，以及已知底物之间的相似度。

The classification accuracy of simple similarity metrics significantly decreases with increasing similarity scores (left panel) since the similarity between known substrates also increases, indicating very specific enzymes.
简单相似度指标的分类精度随着相似度分数的增加而显著降低(左图)，因为已知底物之间的相似度也增加了，这表明酶非常特定。

Since the specificity/promiscuity of an enzyme is not taken into account, the high similarity scores cause a large number of false positives in the classification.
由于没有考虑到酶的特异性/混杂性，较高的相似性分数导致了分类中大量的假阳性。

In contrast, the accuracy of EHreact scores (center panel) does not show a dependence on the individual similarities and specificities because they can both contribute to the score (higher specificities necessitate higher similarities to still observe a good overall score).
相比之下，EHreact得分的准确性(中间面板)并不显示出对个体相似性和特异性的依赖，因为它们都可以对得分做出贡献(更高的特异性需要更高的相似性，才能仍然观察到良好的总体得分)。

The right panel shows the difference of accuracies via EHreact and similarity scores, which is largest for cases with high individual similarities and specificities.
图7的右图显示了通过EHreact的准确率和相似性评分的差异，在个体相似性和特异性较高的例子中差异最大。

The shortcoming of similarity metrics to discern between specific and promiscuous enzymes was already identified in the literature, but, to the best of our knowledge, EHreact offers the first systematic scoring scheme to correct for it.
略

This observation is not tied to the threshold used (see the Supporting Information for other thresholds) but a fundamental shortcoming in similarity metrics not discerning between generalist and specialist enzymes and thus necessitating different thresholds for each enzyme.
这一观察结果与所使用的阈值无关，但在相似性度量方面存在根本缺陷，无法区分通才酶（generalist）和专才酶（specialist），因此需要为每种酶设置不同的阈值。

We thus find that the additional information in the shape, size, and diversity of the template tree of known reactions of an enzyme is beneficial for the scoring of new substrates and helps to find a universal scoring threshold across different data sets.
因此，已知酶反应的模板树的形状、大小和多样性的附加信息有利于对新底物的评分，并有助于在不同的数据集中找到通用的评分阈值。

---

Table 2 furthermore lists classification metrics for a nonenzymatic assay, namely, a set of organic C(sp2)−C(sp3) coupling reactions, where each name reaction (BF3K-Ni-photoredox, BF3K-Pd-Suzuki, CEC-Ni-Weix, CEC-Ni-photo-redox, COOH-Ni-photoredox, MIDA-Pd-Suzuki, and Negishi-Pd) was used to group known reactions, similar to each individual enzyme in the enzymatic assays.
表2进一步列出了非酶学分析的分类指标，即一组有机C(sp2)−C(sp3)偶联反应，其中每个命名反应(BF3K-Ni-photoredox, BF3K-Pd-Suzuki, CEC-Ni-Weix, CEC-Ni-photo-redox, COOH-Ni-photoredox, MIDA-Pd-Suzuki和Negishi-Pd)用于对已知反应进行分组，与酶分析中的每一种酶相似。

Leave-one-out experiments were conducted to score each reaction within each name reaction group. EHreact scores provide an improvement regarding both AUC and accuracy compared to similarity scores, although EHreact scores were developed and tested solely on enzymatic reactions.
对每个名称反应组内的每个反应进行留一法实验评分。EHreact评分与相似评分相比，提供了AUC和准确性方面的改进，尽管EHreact评分仅针对酶反应开发和测试。

We expect this improvement to hold for some other organic reactions, too, namely, whenever the structure around the reaction center contributes to the reaction outcome or yield significantly. Although this is certainly not the case for organic reactions in general, it makes EHreact a useful tool for at least some reaction classes.
作者预计这种改进也适用于其他一些有机反应，即当反应中心周围的结构对反应结果或产量有显著贡献时。虽然这肯定不是一般有机反应的情况，但它使EHreact至少对某些反应类是一个有用的工具。

---

Next, we investigated whether EHreact still provided an improvement over similarity-based approaches if only a single substrate per enzyme was known.
接下来，作者研究了在只知道一种酶的底物的情况下，EHreact是否仍然比基于相似性的方法更具优势。

Thus, template trees and similarity comparisons were solely calculated for the most active substrate for each enzyme in each data set, producing linear template trees without any branches. This analysis thus reflects the case of n = 1 in Figure 6.
因此，只计算每个数据集中每个酶的最活跃底物的模板树和相似性比较，得到没有任何分支的线性模板树。因此，该分析反映了图6中n = 1的情况。

In a linear template tree, the promiscuity scores do naturally not come into play, but the location score may still provide a means to penalize modifications close to the reactive center over modifications in other parts of the molecule compared to the reference structure.
在一个线性的模板树中，混杂分数（promiscuity scores）自然不会起作用，但是位置分数（location score）仍然可以提供一种方法来惩罚靠近反应中心的修饰，而不是相对于参考结构的分子其他部分的修饰。

However, we found no significant trends in the AUC between scores based on similarity and EHreact. For some systems, a penalty based on the location score was beneficial but not for others, indicating that diameter-based template scoring is not necessarily superior to the overall similarity scoring.
然而，作者发现基于相似度和EHreact的得分之间的AUC没有明显的差距。对于某些系统，基于位置分数的惩罚是有益的，但对于其他系统则不是，这表明基于直径的模板得分并不一定优于整体相似性得分。

### 区域选择性和辅底物建议

To evaluate the EHreact’s ability to propose meaningful cosubstrates for multisubstrate reactions, we selected EC classes from BRENDA, which report on reactions with two substrates each, have more than 10 known reactions, less than 70% occurrence of the most frequent substrate over all reactions, and molecular weights less than 200 g/mol per substrate.
为了评估EHreact在多底物反应中提出有意义的共底物的能力，作者从BRENDA中选择了EC类（两个底物的反应，每个反应都有超过10个已知的反应）在所有反应中最常见的底物的发生率低于70%，单个底物的分子质量小于200g/mol。

All reactions were then checked for balance, where unbalanced reactions were discarded, and then atom-mapped via RDT.
然后检查所有反应的平衡，其中不平衡的反应被丢弃，然后通过RDT绘制原子图。

Due to the difficulties of RDT to map some of the reactions, mappings were checked manually and corrected if necessary.
由于RDT在绘制某些反应时存在困难，因此需要手工检查，并在必要时进行校正。

This yielded 555 reactions in 18 EC classes, namely, 2.6.1.X with X = 1, 12, 14, 15, 18, 2, 27, 28, 36, 39, 40, 42, 44, 5, 51, 57, 64, and 73 (transaminase reactions).
这产生了18个EC类的555个反应，即2.6.1。X = 1、12、14、15、18、2、27、28、36、39、40、42、44、5、51、57、64和73(转氨酶反应)。

For the reactions in each EC class, the ability of EHreact and similarity scores to discern between combinations of amine-donors and acceptors as observed in BRENDA (positive data) and all other combinations (obtained by the exhaustive combination of all donors and acceptors within a class corresponding to negative data) was analyzed.
对于每个EC类中的反应，作者对EHreact和相似性得分的辨别能力进行了分析。

We calculated the area under the curve of the receiver- operator-characteristic to obtain a measure of how well the obtained scores can discern between true and artificial combinations of substrates (Figure 8, left panel).
作者计算了受试者-操作者特征的AUC，来分析在多大程度上能够区分真实和人工底物组合(图8，左面板)。

EHreact outperforms similarity scores with an average AUC of 0.69 versus 0.59. We furthermore calculated the rank of the correct reaction partner for each substrate, which occurred only once in the reported reactions but its partner occurred in multiple reactions by enumerating all possible reaction partners and calculating scores via EHreact and similarity on the basis of the other known reactions within an EC class.
EHreact的平均AUC为0.69，相似度得分则为0.59。

The average ranks are shown in Figure 8, right panel, where EHreact ranks the correct partner on average at rank 2.5 and thus higher than a comparison via similarity (average rank 3.6).
平均排名如图8右面板所示，其中EHreact对正确的反应组平均排名为2.5，因此高于通过相似性进行的比较(平均排名3.6)。

The fraction of reactions where the correct partner was identified at rank 1 (top-1-accuracy) is 64% for EHreact and 41% for similarity.
在反应中，在 rank 1 (top-1-accuracy) 识别出正确 partner 的比例为64%(EHreact)和41%(相似性)。

Taking into account the first three suggestions (top-3-accuracy), EHreact correctly identifies the cosubstrate in 81% of cases and similarity for 62% of cases.
考虑前三个提议(top-3-accuracy)，EHreact在81%的例子中中正确识别了共底物，在62%的例子中相似。

EHreact thus outperforms similarity scores for both classifying whether a given combination of substrates is likely to undergo an enzymatic reaction, as well as ranking suggestions for reaction partners.
因此，EHreact在对给定的底物组合是否可能进行酶促反应的分类以及对反应伙伴（reaction partners）的建议进行排序方面都优于相似性分数（similarity scores）

---

Regarding regioselectivity, we selected 13 EC classes from BRENDA where some reactions had multiple possible sites of transformation, here alcohols for oxidoreductase enzymes catalyzing the oxidation of alcohols to ketones/aldehydes (EC 1.1.X with X = 1.145, 1.149, 1.209, 1.213, 1.239, 1.265, 1.283, 1.50, 1.6, 1.64, 1.72, 3.6, 3.9).
在区域选择性方面，作者从BRENDA选择了13个EC类，其中一些反应有多个可能的转化位点，这里是氧化还原酶的醇类酶催化醇氧化成酮/醛(EC 1.1。X X = 1.145, 1.149, 1.209, 1.213, 1.239, 1.265, 1.283, 1.50, 1.6, 1.64, 1.72, 3.6, 3.9)。

We calculated scores for each reaction site using EHreact or similarity scores using the nonregioselective reactions within the same EC class as training reactions. Both EHreact and similarity scores showed 100% top-1-accuracy, thus identifying the correct site of transformation in all cases.
作者使用EHreact计算每个反应位点的分数，或使用相同EC类内的非区域选择性反应作为训练反应的相似分数。EHreact和相似度得分均为100%（top-1-accuracy）。

---

The capability of EHreact to propose and rank cosubstrates, as well as output byproducts, makes it especially attractive for use in the enzymatic cascade design.
略

Designing an efficient cascade includes selecting transformations from a set of possible reactions that recycle cofactors, reduce waste in the form of unwanted byproducts, and find combinations of reactions that drive the equilibrium to the product, which are all tasks that rely on an accurate prediction of cosubstrates, cofactors, and byproducts.
略

### Limitations

In the following, we briefly summarize the current limitations of our tool since we believe that a critical discussion helps us to prevent unintentional misuse, as well as spark developments and solutions that overcome current shortcomings. Since the software is open source, we furthermore invite interested users to contribute toward this effort.
作者简要总结了EHreact的局限性，因为，作者相信批判性的讨论可以防止无意的滥用，以及让更多的人参与“局限性”的解决。由于该软件是开源的，作者希望进一步邀请有识之士为这项工作做出贡献。

---

An apparent limitation of the proposed method is its need for atom-mapped, balanced reactions, which can add additional burden to the preprocessing of databases, where reactions are often unbalanced, and not always atom-mapped, sometimes even incorrectly atom-mapped.
一个明显的缺点是它需要原子映射的平衡反应，这可能会给数据库的预处理增加额外的负担，其中的反应通常是不平衡的，而且并不总是原子映射，有时甚至是不正确的原子映射。

In fact, erroneous atom- mappings are a major limitation to all template-based reaction predictions in both organic and biocatalytic syntheses. Incorrect atom-mappings usually cause unique, nonmeaningful ITSs, which branch off at the beginning of the Hasse diagram of templates.
事实上，在有机和生物催化合成中，错误的原子映射是所有基于模板的反应预测的一个主要限制。不正确的原子映射通常会导致唯一的、没有意义的ITS，这些ITS在模板的Hasse图的开头分支。

EHreact thus provides a framework to easily detect incorrect mappings, but a correction can be tedious and often requires manual interaction. On a similar note, the input of inconsistent configurations, such as open- and closed-loop sugars, leads to an undesired branching in the template diagram.
因此EHreact提供了一个框架(framework)，可以很容易地检测不正确的映射，但是纠正可能会很繁琐，而且常常需要手工交互。类似地，输入不一致的配置，例如开环和闭环糖，会导致模板图中不希望出现的分支。

Furthermore, the full functionality of EHreact requires the knowledge of reactants and products for the training set, but substrate screening studies often only report on the reactants but not the products, measuring reaction success by the consumption of the substrate or a cofactor.
此外，EHreact的完整功能需要训练集的有关于反应物和产物的知识，但底物筛选研究通常只报告反应物而不是产物（通过底物或辅助因子的消耗来衡量反应成功）。

---

We have shown in previous sections that EHreact functions best if more than one reaction per enzyme is known.
当每个酶有多个反应时，EHreact的效果比较好最佳。

If only a single reaction is known, the scoring scheme still profits from the multiple templates extracted at different specificities forming a linear template tree in some cases, but if the user wishes to only output a single reaction template, then there is no advantage over other template extraction routines in the literature.
如果只有一个反应，评分方案仍然从多个模板中提取不同特异性水平的信息，形成一个线性模板树。但如果用户希望只输出一个反应模板，EHreact和其他文献中的反应相比毫无优势。

For a linear template tree, EHreact cannot determine which specificity or level of generality is best, and the specificity has to be determined by user input (for example, include all atoms up to one bond away from the reaction center, which is the second template in a linear template tree).
对于线性模板树，EHreact不能确定哪种特异性或一般性水平最好，而特异性必须由用户输入确定(例如，包括远离反应中心一个键的所有原子，这是线性模板树中的第二个模板)。

This only comes into play where the primary use of EHreact is template extraction instead of scoring.
略

---

Finally, there are some limitations to the scoring algorithm, too. Although EHreact uses a scoring scheme beyond simple chemical similarity metrics, it is still based on common structures and their similarities.
最后，评分算法也有一些局限性。虽然EHreact使用的评分方案超越了简单的化学相似性度量，但它仍然基于常见结构及其相似性。

Thus, for enzymatic systems where the activity does not correlate well with conventional molecular descriptors, we also expect that EHreact will not perform well. Other similarity-based approaches will also fail for such cases.
因此，对于活性与传统分子描述符不太相关的酶系统，我们也预计EHreact不会有很好的表现。在这种情况下，其他基于相似度的方法也会失败。

Also, an inherent limitation of all similarity-based and structure-based approaches is their inability to extrapolate to new substrates, which are very different from known ones.
此外，所有基于相似性和基于结构的方法的固有限制是，它们不能推断出新的底物。

Although the diversity of the EHreact scoring routine might help us to perform better than a fingerprint similarity comparison for extrapolating to new substrates, we expect its extrapolation ability to be at best mediocre.
虽然EHreact评分程序的多样性可能帮助我们在外推到新底物时比指纹相似度比较表现更好，但作者预计其外推能力最多只能达到一般水平。

## CONCLUSIONS

We have introduced a novel method of extracting multiple reaction templates from a set of known reactions and utilizing the mutual information between them to obtain better predictions of the activity of non-natural substrates. The developed open-source software, EHreact, extracts, groups, and saves templates as imaginary transition structures and constructs a Hasse diagram of molecular fragments of the transition states.
作者介绍了一种新的方法：从一组已知的反应中提取多个反应模板，并利用互信息来更好地预测非天然底物的活性。因此，作者开发了开源软件EHreact，它能从模板提取、归类、保存为ITS，并构建过渡态分子片段的Hasse图。

---

EHreact allows for the extraction of single, unique, and mutually exclusive templates at a level of specificity imposed by the set of input reaction, whereas conventional extraction routines lead to multiple, sometimes not mutually exclusive templates and require user-defined criteria of how many atoms to include.
EHreact能够在一组输入的反应的特异性水平上提取唯一的互斥模板；传统的提取程序只能提取多个模板（有时不是互斥的），并需要用户定义标准（参数）来确定包含的原子。

Using the most specific mutual template in a Hasse diagram automatically includes all atoms close to the reactive center, which are conserved within the full set of known reactions, without any knowledge about the system. This significantly lowers the number of extracted templates in a database and discerns between specialist and generalist enzymes.
在Hasse图中使用的是最具体的相互模板，它会自动包含所有靠近反应中心的原子，这些原子在所有已知反应中是守恒的，而不需要对系统有任何了解。这不仅降低了从数据库中提取模板的数量，也区分了专一型酶和通用型酶（specialist and generalist enzymes）。

It furthermore reduces the bias toward larger molecules that is present in radius-based template sets, thus allowing for predictions of reactions that are missed by current approaches.
以半径为基础的模板集中存在“偏向大分子”（the bias toward larger molecules）的问题。EHreact能缓解该偏向，从而允许对当前方法所忽略的反应进行预测。

EHreact can also be used to visualize substrate scopes and specificities of enzymes (or groups of reactions in general) in a straightforward, transparent, and interpretable fashion. It thus offers a white box alternative to black box approaches such as neural network models to predict template specificity for chemical synthesis planning.
EHreact还可以直接、透明并以可解释的方式可视化底物范围和酶(或一般的反应组)的特异性。因此，它提供了一个白盒替代黑盒方法，就像预测化学合成计划的模板特异性的神经网络模型。

---

The template trees, together with a scoring function, can furthermore be utilized to propose possible transformations on a substrate by a given enzyme, as well as score and rank the proposed reactions according to their anticipated feasibility.
我们可以进一步利用模板树和评分功能，来预测酶在底物上建议的可能的转变以及根据预期的可行性对建议的反应进行评分和排名。

The scores allow for a better classification into active and nonactive substrate/enzyme combinations compared to similarity-based scores for experimental screening studies of substrate ranges of diverse enzymes.
与基于相似度的评分相比，该评分可以更好地对活性和非活性底物/酶组合进行分类，用于不同酶的底物范围的实验筛选研究。

The scoring scheme was furthermore shown to accurately rank the correct product highest for substrates that can undergo transformations at different positions, as well as correctly propose cosubstrates for multireactant transformations such as amine transfers, which is an important prerequisite for the application in the computer-aided enzymatic cascade design.
评分方案进一步表明，对于可以在不同位置进行转换的底物，EHreact可以准确地将正确的产物排到第一名，并正确地提出多反应物转换(如胺转移)的共底物，这是应用于计算机辅助酶级联设计的重要前提。

---

We have thus established the extraction and scoring of reaction templates based on Hasse diagrams of common substructures in the imaginary transition structures to be an easy and promising alternative to conventional template extraction and scoring routines, especially where only a few reactions per enzyme are known.
因此，基于假想过渡结构中共同子结构的Hasse图的反应模板的提取和评分程序：EHreact，尽管设计简单但有希望的替代传统的模板提取和评分程序。特别是在每个酶只有几个反应已知的情况下。

We acknowledge that different approaches, such as machine learning of structure activity relationships of enzymes and substrates, are a very promising alternative for large data sets, with a number of studies published recently.
作者也很看好其他方法。比如研究对酶和底物的构活关系的机器学习是一个非常有前景的替代大数据集的方法（最近也发表了许多研究）。

However, for regimes of little data, as presented in this study, we believe that simple heuristic scoring schemes are a more robust and interpretable route toward success and estimate the performance of EHreact to be satisfactory for use in computer-aided pathway design. We plan to utilize EHreact to design multistep synthesis pathways and enzymatic cascades.
然而，对于数据量小的系统，正如本研究中提出的，作者认为简单的启发式评分方案是一个更健壮和更易解释的方法，况且EHreact在计算机辅助路径设计中的性能还令人满意的。因此，作者打算利用EHreact设计多步合成途径和酶级联反应。
