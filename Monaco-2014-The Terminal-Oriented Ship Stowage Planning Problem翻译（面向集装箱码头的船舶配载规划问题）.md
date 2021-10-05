# 面向集装箱码头的船舶配载规划问题

## 作者

Maria Flavia Monaco

Marcello Samarra

Gregorio Sorrentino

## 摘要

船舶配载规划问题是确定集装箱在集装箱船中的最佳位置的问 题。 在本文中，我们主要通过考虑与堆场和运输作业有关的码头管理目标来解决这个问题。我们提出了一个0-1整数规划模型和两步启发式算法。广泛的计算实验表明了我们的方法的效率和有效性。本文还提供了配载规划问题的分类方案。

## 关键字

- 物流

- 优化模型

- 元启发式算法

- 集装箱码头

##  1. 引言

除了2009年观察到的萧条外，1990年至2010年间，集装箱贸易额平均增长了8.2%，达到1.4亿标准箱（TEU），或超过13亿吨（见贸发会议秘书处，2011年，第21页）。为了满足海运的巨大需求，世界集装箱船船队的数量和总容量都有所增加。与2011年有关的最后一个数据（贸发会议秘书处，2011年）显示，集装箱船世界船队共有9688艘船只，总容量约为1260万标准箱，其中包括一些名义容量为14770标准箱的船只。容量为18,000标准箱的新船将交付给一家领先的航运公司。

**对于此类大型船舶，配载计划/规划，即决定集装箱在航行期间必须放置在集装箱船内的哪个位置，成为确定集装箱船运营成本的关键因素。** 实际上，正如 Delgado、Jensen 和 Guilbert（2012）所指出的，**配载计划旨在减少船舶在港口的停留时间，从而减少港口费用。 此外，较短的周转时间允许船舶在预定到达时间内以较低的航行速度到达下游港口，从而也减少了燃料消耗。 港口费用和船用燃料成本是航运公司运营成本的主要决定因素**（Rodrigue、Comtois 和 Slack，2011 年）。 船舶减速也有很大的环境效益，因为降低航行速度已被认为是减少温室气体排放的最直接的单一因素（贸发会议秘书处，2011 年）。

配载规划不仅是船公司的相关问题，也是码头运营商的相关问题，**码头运营商实际上涉及数千个集装箱的装卸和储存。码头运营商的目标是通过有效利用码头资源，如泊位、岸桥、堆场空间和堆场机械，为集装箱船提供高水平的服务**。由于配载规划问题固有的困难，管理实践通常采用分层的方法，从集装箱船的泊位和岸桥分配开始，如果必要的话，并最终在集装箱船到达时间接近时更新决策。然后确定码头岸桥的调度。最后，在装卸过程开始之前，必须对码头岸桥的工作顺序进行设计。由于**码头运营商必须实现船公司提供的装（卸）货指令，良好的配载计划对码头活动有很大的影响**。此外，码头管理人员也有机会参与配载规划过程，从而降低码头运营成本，提高效率。

在本文中，我们讨论了面向码头的船舶配载规划问题（SSPP），即确定配载计划，使与堆场和运输作业相关的成本最小化的问题。本文的提纲如下。在第2节中详细描述了这个问题。分类和文献综述提供在第3节。SSPP的0-1整数模型将在第4节中给出并讨论。第5节给出了启发式求解算法；数值结果将在下面的第6节中进行分析。最后一节给出了结论和未来的研究趋势。

## 2. 问题描述

确定集装箱在船舶内的分配是一项非常复杂的工作，它需要了解集装箱和船舶特性的全部信息。由于这些原因，配载计划的设计通常包括两个连续的规划阶段（Alvarez, 2006, Steenken et al.， 2004），涉及两个不同的决策者。**第一阶段由航线规划者执行，他们对集装箱船在其航程中所访问的每个港口的所有装卸集装箱以及船舶的结构都有一个完整的了解**。**第二阶段与码头规划有关：它在船舶港口轮转的每个港口执行，只考虑在该特定港口装载的集装箱。码头船舶规划的最终目标是设计装卸设备（码头岸桥和水平运输工具）的装载作业顺序。最后一个问题被称为船舶装载或装载顺序问题**。下面，我们将简述集装箱船的结构。更多细节可以在附录A中找到。然后我们分析配载计划过程的两个阶段。

我们可以把停泊在码头的集装箱船想象成一个最长边与码头平行的箱子。它由几个更小的盒子组成，称为槽（slots），能够容纳一个20英尺的集装箱(TEU)。一个槽（slot），以及集装箱在集装箱船上的位置，可以通过三个坐标来确定：贝（bay），row（行），tier（层）。贝（bay）是槽的纵向坐标；行和层分别表示横线和垂直线。两个相邻的槽，共享同一行和层坐标，且用于存储两个TEU集装箱。船舶配备有货物固定手册，其中说明了每个槽的最大允许负载、捆扎方式、以及每个船组的最大重量和高度。

### 2.1 第一阶段规划

在第一阶段，**集装箱根据其属性进行分类，如尺寸（标准、45英尺、高立方、超大)，重量级别（轻、中、重），类型（冷藏、开顶），装载（危险，易腐）和目的港（Port of destination，POD）。为了保证船舶的静、动平衡，考虑了许多约束条件，将同一类的集装箱安排在相邻的槽位内**。**航运公司的目标是最大限度地利用船舶，并最大限度地减少在港口轮转期间on-board shifts（船舶翻箱）的数量**，也称为**重新积载（re-stows）或过度积载（over-stows）**。**在给定的A码头，当装载目的港为B的集装箱必须临时卸下，然后在同一集装箱船上重新装上（可能在不同的位置），以便卸下部分目标港为A的集装箱时，就会发生重新积载（re-stow）**。

我们将把**预配计划（pre-stow）称为第一阶段的输出**。它**要么是一个粗略的配载图，要么是一个详细的配载图**。**在第一种情况下，航线规划者为每个舱位（ship-slot）指明了一个集装箱类，这是一组可以装在舱位（slot）中的集装箱的属性**。因此，这种预配计划也被称为基于类的配载。图1显示了一个基于类的配载图示例。这里集装箱类是由目的港、尺寸和类型定义的。**任何带有目的港为A的高立方的冷藏集装箱都可以装在82层和84层的任何允许的槽（slot）中**。

![](F:\100-Major-专业\105-港口运作优化\006-海侧作业 (Quayside Operiations)\配载计划 (Stowage Planning)\Fig. 1 Class based stowage plan for deck slots of a vessel bay.png)

<center>图1 基于类的船舶贝甲板槽配载计划</center>

相反，**详细的预配计划指示要装载在每个槽（slot）中的确切集装箱，如图 2 所示。该装载计划可以检索的详细信息是：目的港（A）、装载港（B）、重量、冷藏集装箱的工作温度、类型、槽位坐标，还有一个集装箱代码，用于明确标识每个槽位中要装载的集装箱。 请注意，详细的预配计划隐含地定义了一个且仅一个基于类的配载计划。 我们将后者称为详细的基于类的配载计划（class-based stowage plan induced by the detailed one）**。 相反，**基于类的预配计划可以产生许多不同但兼容的详细计划。 由于后者的特性，码头管理在规划集装箱船的配载和优化运营成本方面发挥着积极的作用**。 这是下一小节的主题。

![](F:\100-Major-专业\105-港口运作优化\006-海侧作业 (Quayside Operiations)\配载计划 (Stowage Planning)\Fig. 2. A detailed stowage plan for some slots of Fig. 1.png)

<center>图2 在图1中一些槽的详细存储计划</center>

### 2.2 第二阶段规划

**预配计划通过电子数据交换（EDI）文件发送到集装箱船将要访问的所有港口。它作为码头规划者执行第二阶段的输入基准**。在给定的港口，**输入包括：在以前的港口装载并将卸到别处的集装箱（剩余在船上的集装箱）、将要被卸船的集装箱、预配计划限制的要被装船的集装箱**。第二阶段的规划是将预配计划转化为运营计划，并为集装箱船上的每个岸桥定义一个详细的工作清单。

**码头规划者的目标是使船舶周转时间和码头运营成本最小化。值得注意的是，周转时间主要由战术和操作层面的几个层次决策决定，即泊位分配、岸桥分配分配和岸桥调度**，感兴趣的读者可以参考（Meisel, 2009, Sammarra et al.， 2007, Monaco et al.， 2009）关于这些主题。**当码头规划者面对SSPP时，上述决定已经做出。因此，在设计可操作的配载计划时，码头规划者主要关注的是减少由于集装箱从堆场到岸边运输而产生的操作成本，并考虑可能的reshuffles、yard-shifts（堆场翻箱），是非常耗时和无生产力的行动，每当一个堆栈的顶部的一些集装箱必须被移走（然后重新堆放），以便拿起位于同一堆栈的底部的一个集装箱时，就会发生这种情况。**

**当预配计划是基于类时，可通过选择与预配计划兼容的详细配载计划中成本最小的计划，获得可操作性的配载计划。码头规划者必须遵守的基本约束是与集装箱船舶稳性有关。**当完成可操作性预配计划时，该预配计划必须被集装箱船船长进行有效性验证以及检查，一旦被接受，它将成为码头的操作计划，并且装载列表可以被设计。

**如果有一个详细的预载计划，码头规划者似乎没有办法减少操作成本。事实上，他可以接受预配计划作为可行计划，最终诉诸于预翻箱策略（pre-marshalling）**（Lee & Chao, 2009）。**预翻箱（pre-marshalling）是将出口集装箱重新分配到靠近停泊船舶的堆场区域，以减少装载作业的运输时间（和成本），避免重新洗牌（reshuffles，堆场翻箱）**。预翻箱有时可能既不可行，也无利可图。可行性取决于装船前重新分配集装箱的时间和资源（设备和工作团队）。此外，靠近岸边的堆场区域代表着一种战略性的码头资源，管理实践旨在减少重新分配（reallocation），因为它们在任何情况下都是昂贵的（Moccia, Cordeau, Monaco， & Sammarra, 2009）。因此，必须在成本/效益分析的基础上决定采用预翻箱策略。规划人员还可以从预配计划中检索基于类的预配计划，像以前一样寻找不同的兼容解决方案，由船长批准。与第二规划阶段相关的整个决策过程如图3所示。

![](F:\100-Major-专业\105-港口运作优化\006-海侧作业 (Quayside Operiations)\配载计划 (Stowage Planning)\Fig. 3. The flowchart of the decisional process.png)

<center> 图3 决策过程流程图</center>

**在本文中，我们讨论了SSPP的第二阶段，假设一个基于类别的预配计划作为输入。参照图3，我们考虑“码头配载计划（terminal stowage plan）”的过程。我们将专注于将出口集装箱分配到船槽（ship slots），符合预配计划和稳定性约束，最大限度地减少总运输时间和堆场翻箱（yard-shifts）**。我们特别提到了意大利南部焦亚陶罗港的中转集装箱码头，正是这个港口激发了我们的研究。焦亚陶罗有一个广泛的堆场，由一个跨运车（直接转运系统- DTS）经营。在这样的背景下，堆场几乎是三/四个集装箱的高度(Monaco et al.， 2009)。

## 3. 问题分类和文献综述

SSPP受到了研究者们的广泛关注。然而，在以前的研究中所考虑的各种各样的设置、假设和目标突出了对这个问题缺乏普遍接受的观点。例如，只有在一些文献中明确考虑到两个规划阶段之间的区别。在本节中，我们根据所处理问题的最重要特征提出一个分类方案，并相应地回顾了一些相关的科学文献。分类问题所需的信息已经从上下文推断出来，如果不是作者明确提供的。

### 3.1. 分类方案

我们采用四字段表示法。 第一个字段是指决策者， 第二个字段中描述决策问题的输出； 第三个字段代表设置以及问题的输入； 最后一个字段描述了要最小化的目标函数。 第二个和第三个字段可以采用一个或多个值，在这种情况下，它们用逗号分隔。 目标函数可以表示为单个性能度量的加权总和，其中权重由$\lambda$表示。 每个字段可以取的值列在表1中。

<center>表1 分类方案的四个字段以及相应的数值</center>

| Field 字段            | Value 值  | Description 描述                                             |
| :-------------------- | --------- | ------------------------------------------------------------ |
| Decision maker 决策者 | *line*    | Shipping line planner 航线规划者                             |
|                       | *port*    | Terminal planner 码头规划者                                  |
| Ouput 输出            | *class*   | Class-based stowage plan 基于类的配载计划                    |
|                       | *detl*    | Detailed stowage plan 详细的配载计划                         |
|                       | *lseq*    | Load sequencing 装载顺序                                     |
|                       | *prot*    | Stowage plan for the whole port rotation 整个港口轮询的配载计划 |
| Setting 设置          | *1-qc*    | Only one quay cranes performing the loading process 只有一个岸桥完成装载过程 |
|                       | *m-qcs*   | Many quay cranes performing the loading process 有许多个岸桥完成装载过程 |
|                       | *ycranes* | The yard is operated by yard cranes 堆场由场桥操作           |
|                       | *reachs*  | The yard is operated by reach-stackers                       |
|                       | *scarr*   | The yard is operated by straddle carriers 堆场由跨运车操作   |
|                       | 1-bay     | A single bay is involved in the stowage process 在配载过程中只涉及单个贝 |
|                       | *class*   | The class-based stowage plan is an input datum 基于类配载计划作为一个输入范式 |
|                       | *equil*   | Equilibrium constraints are explicitly imposed 等式约束被明显的施加 |
| Objective 目标        | *oshift*  | Number of on-board shifts 船上翻箱的数量                     |
|                       | *mixst*   | Number of ship stacks with different port of destination 不同目的港的船舶堆叠数量 |
|                       | *nstack*  | Number of ship stacks used to stow containers 用于装载集装箱的船舶堆数 |
|                       | *yshift*  | Number of yard shifts 堆场翻箱的数量                         |
|                       | *tload*   | Total loading time 总共装载时间                              |
|                       | *eqviol*  | Violation of equilibrium constraints 违反等式约束            |
|                       | *stobj*   | Objective related to the vessel stability 与船舶稳定有关的目标 |
|                       | *qcobj*   | Quay cranes related objective 与岸桥有关的目标               |
|                       | *transp*  | Yard-to-quay transport time 堆场到岸边的运输时间             |
|                       | *late*    | Total lateness in the yard-to-quay transport 堆场到岸边运输总共的延误 |
|                       | *misc*    | Other performance measures 其他性能衡量                      |

表1中的一些条目需要澄清。首先，我们注意到value class对于第二个和第三个字段都是可行的。实际上，基于类的积载计划（class-based stowage plan）可以是规划过程第一阶段的输出，也可以是第二阶段的输入。目标函数tload是岸桥装载所有出口集装箱所需的时间。等式或稳定约束有时被视为软约束，在这种情况下，他们的违法将在目标函数中受到惩罚，用*eqviol*表示。其他需要最小化的稳定性函数，如trim和list angles，用*stobj*表示。至于*qcobj*，它是衡量岸桥效率（makespan、跨bay移动等）的一个指标。

### 3.2 文献分析

表2总结了有关SSPP的论文、分类和数学模型的类型（如果有的话）。如果同一作者的许多参考文献关注由不同算法解决的同一个问题，我们只报告问题陈述和可能表述的论文。

<center>表2 涉及SSPP的论文列表【整数规划(IP)；0-1规划(BIP)；混合整数规划(MIP)；非线性整数规划；约束规划(CP)】</center>

|              | **paper**                                                    | **Problem Classification**                                   | **Model** |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | --------- |
| First Phase  | [Avriel et al. (1998)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0035) | *Line\|detl,prot\|1-bay\|oshift*                             | BIP       |
|              | [Wilson and Roach (1999)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0145) | *Line\|class,detl,prot\|equil\|oshift+mixst*                 |           |
|              | [Dubrovsky et al. (2002)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0050) | *Line* \|*detl, prot* \|*1-bay* \|*oshift* *+* λ · *eqviol*  |           |
|              | [Kang and Kim (2002)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0065) | *Line* \|*class, detl, prot* \|*equil* \|*oshift* + *qcobj*  | IP        |
|              | [Pacino et al. (2011)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0110) | *Line* \|*class, prot* \|*equil* \|*oshift* + *qcobj*        | IP        |
|              | [Delgado, Jensen, Janstrup et al. (2012)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0045) | *Line* \|*detl, prot* \|*class, equil* \|λ1·*oshift* +λ2·*mixst* +λ3·*nstack* | BIP+CP    |
| Second Phase | [Steenken et al. (2001)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0130) | *Port* \|*detl, lseq* \|*1-qc, scar, class* \|*late*         | MIP       |
|              | [Ambrosino et al.(2004)](https://www.sciencedirect.com/science/article/pii/S0965856403000892) | *Port* \|*detl* \|*class, equil* \|*tload*                   | BIP       |
|              | [Kim et al. (2004)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0070) | *Port* \|*detl, lseq* \|*mqcs, ycranes,class, equil* \|λ1·*yshift* +λ2·*transp* *+* λ3·*misc* | NIP       |
|              | [Alvarez (2006)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0005) | *Port* \|*detl* \|*1-qc, reachs, class* \|λ1·*yshift* +λ2·*transp* +λ3·*eqviol* | MIP       |
|              | [Imai et al. (2006)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0060) | *Port* \|*detl, lseq* \|λ1·*stobj* +λ2·*yshifts*             | BIP       |
|              | [Monaco and Sammarra (2008)](https://www.sciencedirect.com/science/article/pii/S0377221714004536#b0090) | *Port* \|*detl* \|*mqcs, scar, class, equil* \|*yshift* + *transp* | BIP       |

第一组论文从航线规划的角度来研究这一问题。他们的共同目标是尽量减少所有港口的船舶轮班的on-board shifts（船上翻箱）。Avriel, Penn, Shpirer和Witteboon(1998)的论文依赖于一个简化的假设(船由一个大的bay组成)，忽略了平衡约束，作者提出了一种启发式算法。Dubrovsky、Levitin和Penn(2002)针对Avriel等人(1998)所描述的问题提出了一种遗传算法，但考虑了软平衡约束。

在所有剩下的四篇论文中，SSPP被分解为两个子问题：第一个子问题用于将集装箱组分配到bay sections，第二个子问题用于将集装箱分配到槽（slots）。明确地施加了平衡约束，目标函数还包括码头岸边运作的一些性能度量。Wilson and Roach(1999)的论文，以及（Wilson and Roach,2000, Wilson et al 2001)，提出了一个分支定界算法的用于求解第一个子问题，和禁忌搜索启发式用于求解第二个子问题。Kang和Kim(2002)对集装箱组到bay的子问题提出了贪婪启发式方法，对集装箱到槽（slot）的问题提出了树搜索方法。此外，Pacino et al.， 2011, Delgado, Jensen, Janstrup et al.， 2012利用了分解方法，将两个子问题表示为multi-port master planning 和slot planning。Pacino等人(2011)讨论了master planning，获得了一个基于类的配载计划，该计划成为Delgado、Jensen、Janstrup等人(2012)提出的slot planning阶段的输入。在这两篇论文中，实际情况的数值结果都是由商业求解器给出的。

第二组中的三篇论文涉及配载和装载顺序问题（load sequencing problems）的整合。Steenken等人，2001年，Kim等人，2004年假设基于类的配载计划（class-based stowage plan）作为输入，并考虑集装箱装卸设备。Steenken et al.（2001）中描述的问题依赖于只有一台岸桥操作集装箱船的假设，并且忽略了平衡约束，其目的是最大限度地减少堆场至岸侧运输的总延误。给出了由商业求解器获得的实际算例的数值结果。Kim等人（2004年）在更现实的背景下描述了集成问题，该模型将等式约束和若干性能指标结合在目标函数中，采用beam搜索算法求解。相反，Imai、Saski、Nimimura和PaaDimiTiRouu（2006）的论文没有考虑搬运设备，提出了一种遗传算法。

Ambrosino, Sciomachen和Tanfani(2004)解决了港口轮转的第一个港口的SSPP问题，即mater bay plan problem，没有参考所采用的设备，但考虑了硬平衡约束。作者提出了一个三步启发式。Ambrosino et al.， 2006, Sciomachen and Tanfani, 2007, Ambrosino et al.， 2009给出了相同问题的不同算法，而Ambrosino, Anghinolfi, Paolucci, and Sciomachen(2010)给出了求解方法的实验比较。在Alvarez(2006)研究的SSPP指的是堆场由reach-stacker运作，并且只服务一台岸桥，作者提供了一个禁忌搜索算法。

按照上述分类方案，本文讨论的SSPP表示为*port | detl | mqcs, scar, class, equil | yshift + transport*。Monaco and Sammarra (2008)提出了一个初步的模型来解决同样的问题。本文所建立的模型在移码评价和稳定性约束方面都是对以往模型的丰富和改进。从表2的分析可以看出，我们的研究方向弥补了SSPP文献中的一个空白，因为之前的研究没有涉及现实假设下DTS码头背景下的配载规划。实际上Steenken et al.(2001)研究了采用跨运车（straddle carriers）的码头的问题，但只考虑了一个岸桥和一个不同的目标函数。我们考虑的目标函数与Alvarez, 2006, Kim等人2004有一些相似之处，但在那篇论文中，配载规划与集约堆场的（intensive yards）码头有关，在集约堆场中，集装箱被安排在密集的进出口箱区。这些箱区由可达堆垛机（reach-stackers）或专用堆场场桥操作，而堆场到海侧的运输则由车队的卡车执行(Indirect Transfer System- ITS)。相反，在DTS码头，straddle carriers可在每个堆场自由作业。由于堆场布局和设备的不同，DTS和ITS码头的规划策略存在显著差异。因此，在给定的上下文中为SSPP设计的数学模型和算法不能应用于另一个上下文中。

## 4. SSPP的一个数学模型

在这一节，我们描述了SSPP的一个0-1整数规划模型。如2.2节所示的，我们将给出：

1. 船舶的配置，使用bays(*b*),row(*r*),和tier(*t*)(vessel bay-plan)表示；
2. 基于类的预配计划（the class-based pre-stow plan)；
3. 分配给船舶服务的岸桥的集合*K* (*|K|=m*)；起重机拆分和调度（crane split and scheduling）（crane split = QCAP），即每台岸桥处理bay的序列，以及每台岸桥在bay的操作方式(sea-to-shore、shore-to-sea、row-wise、stack-wise)；
4. 需要被装载的集装箱集合*N*(*|N|=n*)，对每一个集装箱$i\in N$，其类别$c_i$，weight（权重？重量？）$w_i$，代码以及在堆场的位置；

请注意，起重机计划（crane schedules）的构建考虑了非干扰约束non-interference（参见 Meisel，2009，Monaco 和 Sammarra，2011）。 多亏了可用信息，对于每个起重机 $k\in K$，$k$ 处理的槽(slot)序列是唯一定义的。 令 $S^k$ 是可用于配载并与岸桥 $k$ 相关的槽(slot)的有序子集。 假设岸桥的工作中没有由于堆场到岸边的运输而发生空闲（这可以考虑适当数量的跨运车straddle carriers来实现），我们还可以将每个槽slot$(b,r,t)\in S^k$ 一个开始装卸时间，即岸桥$k$ 开始在该槽(slot)中装载集装箱的时间。因此，集合$S^k$ 可以合并为集合$S$，根据开始处理时间完全排序（ties任意打破关系）。 每个槽位slot $(b,r,t)\in S$ 可以映射成一个索引 $p=\{1,…,\sum_{k=1}^mn^k\}$，因此决定集装箱的装载位置简化为为每个集装箱分配一个槽位(slot)位置 $p\in P$。 我们采用以下符号：

- $c_p$ $\forall p\in P$	表示槽位（slot-position）的类，如预配计划所描述的；
- $\theta_p$ $\forall p\in P$    表示槽位（slot-position）的开始装卸时间；
- $\tau_{ip}$ $\forall i\in N,p\in P$   表示将集装箱$i$从其堆场位置运输到船舶bay $b$（对应槽位置slot-position $p\equiv(b,r,t)$所需要的时间；
- $L=\{1,...,|L|\}$  表示涉及到配载过程的ship-stacks（船舶堆栈？船的一个船舱？）的集合；
- $\Psi(l)\subseteq P$  $\forall l\in L$   表示属于ship-stacks（船舶堆栈？）的槽位（slot-position）的集合；
- $W_l$  $\forall l\in L$   表示ship-stacks（船舶堆栈）$l$允许的最大的能装载集装箱的重量；

- $N(p)$  $p\in P$  表示集装箱$i\in N$的一个子集，在这个子集中，满足$c_i=c_p$ （此参数表示能够配载到槽位slot-position $p$的集装箱的集合）；
- $P(i)$   $\forall i\in N$  表示槽位（slot-position）$p\in P$的一个子集，在这子集中，满足$c_i=c_p$ （此参数表示集装箱$i$可以配载的槽位slot-position）的集合； 
- $\Delta(i)\subset N$  $\forall i\in N$ 表示和集装箱$i$在同一个堆场堆栈（yard-stack）的集装箱的集合，且集合当中的集装箱均在集装箱$i$的上方；
- $\pi(p)$  表示和槽位（slot-position）$p$在同一个船堆栈（ship-stack），并且就在$p$的紧邻的上方的槽位，也就是，$p\equiv(b,r,t)$，那么，$\pi (p)\equiv (b,r,t+1)$；
- $\sigma$  表示跨运车（straddle carrier）完成一次翻箱（yard-shifit）所需要的的时间；

我们定义了如下决策变量：

- $x_{ip}\in \{0,1\}$  $\forall i\in N, p\in P$  当且仅当集装箱$i$被分配到槽位（slot-position）$p$时，$x_{ip}=1$；
- $z_{ij}\in \{0,1\} $  $\forall i\in N, j\in \Delta(i) $  当且仅当集装箱$i$离开其堆场堆栈（yard-stack）比集装箱$j$早时，$z_{ij}=1$；

SSPP模型如下所示：
$$
\begin{equation}
min \sum_{i\in N}\sum_{p\in P(i)}\tau_{ip}x_{ip}+\sigma\sum_{i\in N}\sum_{j\in \Delta(i)}z_{ij}\label{1}
\end{equation}
$$

$$
\sum_{p\in P(i)}x_{ip}=1,\forall i\in N\label{2}
$$

$$
\sum_{i\in N(p)}x_{ip}=1,\forall p\in P\label{3}
$$

$$
\sum_{i\in N(p)}w_ix_{ip}-\sum_{j\in N(\pi (p))}w_jx_{j\pi (p)}\ge0,\forall p\in P\label{4}
$$

$$
\sum_{p\in \Psi(l)}\sum_{i\in N(p)}w_ix_{ip}\le W_l,\forall l\in L\label{5}
$$

$$
\sum_{q\in P(j)}(\theta_q-\tau_{jq})x_{jq}-\sum_{p\in P(i)}(\theta_p-\tau_{ip})x_{ip}\le Mz_{ij},\forall i\in N,j\in \Delta(i)\label{6}
$$

$$
x_{ip}\in \{0,1\},\forall i\in N,p\in P(i)\label{7}
$$

$$
z_{ij}\in \{0,1\},\forall i\in N,j\in \Delta (i)\label{8}
$$

在这个模型中，等式$\eqref{2}$和$\eqref{3}$为在属于同一类（class）的集装箱和槽位（slot-position）之间的匹配约束，约束$\eqref{4}$确保配载在同一个船堆栈（ship-stack）的集装箱的重量不会从下到上为依次递增的情况，船堆栈（ship-stack）的最大允许重量由约束$\eqref{5}$施加，约束$\eqref{6}$设置变量$z$的正确值，事实上，项$(\theta_p-\tau_{ip})$代表集装箱$i$离开其堆存的堆场堆栈（yard-stacks）的理想时间，如果集装箱$i$被分配到槽（slot）$p$，当集装箱$i$和集装箱$j$位于同一堆场堆栈（ship-stack），并且$j\in \Delta(i)$时，那么约束$\eqref{6}$左侧为正的，如果$i$在$j$之间被取走，在这种情况下，变量$z_{ij}$必须等于1，那么一个堆场翻箱（yard-shift）就被计算到了，相反，如果约束$\eqref{6}$的左边不为正，目标函数将迫使相应的变量在每个最优解处为零。目标函数是运输时间和翻箱（reshuffling）时间之和。

一般来说，该模型对翻箱（reshuffling）次数的估计较低。主要原因是在装卸作业期间堆场堆栈（yard-stacks）的配置可能会发生变化，要么是因为一些集装箱必须由其他集装箱船装载，要么是因为一些进口集装箱必须堆存在考虑中的船舶装卸作业所涉及的yard-stack中。在这两种情况下，我们的模型都没有考虑这样的集装箱，并且忽略了它们可能导致的堆场翻箱（yard-shifts）。另一方面，与给定配载图相关的reshuffles（翻箱）数不能精确计算，如2.2节所述。配载计划的设计本质上是一个离线优化问题，而yard-shifts（翻箱）是在装载过程中发生的，可能只能是预先估计的。当然，当堆场被组织成专门的装卸区域时，采用的堆场翻箱计算变得更加准确(Murty, Liu, Wan， & Linn, 2005)，也就是说，存放集装箱的堆场堆栈（yard-stack）不参与其他集装箱船的装卸作业。

## 5.求解算法

船舶配载规划问题通常是NP-hard (Avriel, 2000)。因此，文献中大多数的求解方法都是基于启发式过程的。在本节中，我们提出了求解SSPP的两步启发式算法(TSA)。第一步(CH)构造一个初始可行解，第二步(IH)寻找更好的可行解。这两个步骤都是基于Tabu Search范式的，Tabu Search范式是一种迭代的局部搜索启发式方法，采用了记忆机制(Glover, Laguna， & Martì， 2007)。在单纯形方法中，我们首先描述改进阶段的启发式IH，假设给出了初始可行解；然后，我们将说明如何在CH中成功地使用相同的算法来构造初始解。

### 5.1 改进阶段

TODO

### 5.2 构建阶段

现在，我们描述如何构造一个可行解，作为5.1算法的初始点。让可用的船槽（ship-slots）根据它们的类别（class）和坐标（coordinates）进行排序。同一类（class）的槽（slot）依次按POD（目的港，port of destination）、维度（dimension）、状态（status，满full、空empty和冷藏reefer）排序，而空间坐标（spatial coordinates）按以下顺序考虑：bay、tier和row。对出口集装箱执行类似的排序，首先按类（class）分类，比如船槽（ship-slots），然后按重量（weight）分类。

通过将第i个集装箱分配给第i个船槽（ship-slot），得到一个基于类的完美匹配$x^r$。向量$x^r$满足模型的约束条件$\eqref{2}$，$\eqref{3}$，使得最重的集装箱与最低层坐标的船槽（ship-slots）相匹配。让我们考虑下列数量：

TODO

基本上向量$\lambda$和$\mu$的分量分别测量$x_r$对约束$\eqref{4}$和$\eqref{5}$的违反，而(11）表示总违反。如果$v(\lambda(x^r),u(x^r))=0$，则满足约束$\eqref{4}$和$\eqref{5}$，因此$x_r$是模型的可行解，可以作为算法1的初始解。

当$x^r$不可行时，我们使用禁忌搜索算法来寻找可行解。特别地，我们采用了和算法1的同样的方案，在适当地重新定义了邻域结构后，采用了最小化的目标函数和起点。为此，$F(x)$是满足约束（2)和(3）的解集，将同质集装箱交换成的兼容的槽，目标函数$c(x)$是总共违反的$v(\lambda(x^r),u(x^r))$，如（11）所示，$x^0$是基于类的完美匹配$x^r$，如上所述。算法2总结了构造启发式算法。

## 6. 计算结果

