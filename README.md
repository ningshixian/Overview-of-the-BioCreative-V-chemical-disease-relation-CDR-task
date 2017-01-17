# BioCreative-V 任务：chemical-disease-relation（CDR）-task 概述
[论文地址](http://www.biocreative.org/media/store/files/2015/BC5CDR_overview.final.pdf)

> BioCreative V：自动从文献中提取CDR

##Abstract
我们设计了两个挑战任务：疾病命名实体识别（`DNER`）和药物诱导疾病（`CID`）关系提取。为了协助系统的开发和评估，我们创建了一个大的注释文本语料库，包括来自1500 篇PubMed文章的药物、疾病及其关系的标注。DNER任务 - 最好的系统获得了86.46％的F分数，接近人类注释者协议（0.8875）的结果； CID任务-最高结果的F得分为57.03％。通过机器学习整合所有队伍提交的系统，能够进一步提高DNER和CID的F-score，达到88.89％和62.80％。此外，我们评估的另一个方面是测试每个参与系统返回实时结果的能力：每个队伍在DNER和CID Web服务系统上的平均响应时间分别为5.6和9.3秒。大多数团队使用基于机器学习的混合系统来提交结果。

[Database URL](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)


##Introduction and motivation

虽然药物发现的最终目标是开发用于治疗的化学品，但是化学品和疾病之间不良药物反应（**ADR**）的识别对于改善化学安全性和毒性研究以及促进药物化合物存活的新筛选测定是重要的。鉴定药物作为生物标记，也有助于了解药物和病理学之间的潜在关系。 因此，从非结构化自由文本到结构化知识的这种药物疾病关系（`CDR`）的手工标注，以促进潜在毒性的识别已经是某些生物信息学数据库的重要主题。
	手工标注生物文献的CDR是昂贵的，并且无法跟上文献的快速增长速度。因此已经有许多的尝试，通过自然语言处理（NLP）方法来自动化提取这种关系。近几年，提出了各种各样的关系提取方法，诸如`简单的共现`，`模式匹配`，`机器学习`和`知识驱动`的方法。 还开发了少量测试语料库，但是它们的大小和标注范围有限。 最近，一组类似的计算方法已被应用于许多不同的数据集，例如FDA的不良事件报告系统（FAERS），电子医疗记录，推特和社交媒体中的用户评论。自由文本的自动生物医学关系检测仍然具有挑战性，从识别相关概念[ 疾病 ]以提取关系。由于缺乏一个综合性基准数据集，限制了不同计算技术之间的比较，难以评估和改进现有技术的当前状态。 此外，几乎没有用于关系提取的软件工具可以免费获得。

##Materials and methods

通过BioCreative V，BioNLP研究的主要正式评估活动之一，我们组织了从生物医学文献自动提取CDR的挑战任务，目标是**支持生物化学，新药物发现和药物安全监测**。 更具体地说，我们设计了两个子任务：
1. **疾病命名实体识别（`DNER`）**。 用于CDR自动抽取的中间步骤是DNER和归一化，根据先前的BioCreative CTD任务和其他研究的经验，DNER是非常困难的。 对于这个子任务，参赛系统被给予PubMed标题和摘要，并要求返回规范化的疾病概念标识符（normalized disease concept identifiers）。
2. **药物诱导疾病（`CID`）关系提取**。为参赛系统提供来自PubMed文章的标题和摘要作为输入（与DNER输入相同），并要求返回具有归一化概念标识符的排序列表<chemical, disease>，其中药物诱导的疾病与摘要相关（**drug-induced diseases are associated in the abstract**）。
注意，药物和疾病使用医学主题词表（`MeSH`）控制词汇来描述。 系统需要返回实体对; 两个实体需要归一化为MeSH的标识符，（along with）以及实体对在文章中的文本跨度。

![Alt text](./QQ截图20170117153116.png)

####Task data

对于任务，我们总共准备了**1500篇PubMed文章**：500 each for the training, development and test set. 所有1500篇文章，大多（1400篇）选自现有的CTD-Pfizer协作相关数据集， 剩余的100篇文章代表完全新的方案，被纳入[测试集](http://www.biocreative.org/re%20sources/corpora/biocreative-v-cdr-corpus/)。
	在过去与辉瑞合作期间，CTD策划了超过15万种化学疾病相互作用。 CTD生物检查者遵循CTD的严格的策划过程，并尽可能从摘要中规定互动，除非需要引用全文以解决摘要中提到的相关问题。对于CDR任务，我们主要利用1400篇文章中的现有训练数据。 另外100篇文章的关系数据是在CTD工作人员的CDR挑战期间生成的，并且在挑战完成之前，该数据集不公开。
####Task evaluation
对于参与者系统的最终评估，使用标准`precision, recall 和 F-score` 进行度量。将文本挖掘出实体（疾病）和关系（`<化学，疾病>对`）与手动标注的数据进行比较。 更具体地，通过将文档内标注的一组疾病概念与参与者系统预测的一组疾病概念进行比较，来评价DNER结果。类似地，通过将文档内注释的化学疾病关系集合与系统预测的化学 - 疾病关系集合进行比较来评估CID结果。
对于结果提交，参赛队伍遵循以前的BioCreative-CTD任务执行的程序，通过Web服务提交结果（我们允许手动运行的离线提交）。 特别地，RESTful 被选择作为参与者web服务的体系结构风格。 为了帮助参与者，组织者提供可执行文件以及分步安装指南。 此外，还向团队提供了一个测试网站，以模拟以后用于测试参与者系统的确切的系统测试环境。由于使用REST，我们能够报告每个系统的响应时间以及它们的准确性。

####基准系统
为了比较，我们为DNER和CID任务开发了几个基准系统。
对于DNER，我们首先开发了一种直接的字典查找基线方法，需要依赖于CTD中的疾病名称的。我们还使用out-of-box
DNorm（NCBI以前的DNER和归一化工作）重新训练了模型。 DNorm将基于丰富特征的方法和用于命名实体识别的条件随机场与用于基于成对学习排序的归一化的新颖机器学习方法相结合。 DNorm是一个可比较的基准系统，在之前的疾病挑战中取得了最高的性能。
对于CID任务，我们使用具有两种变体的简单同现方法建立了基线：药物和疾病在抽象级别和在句子水平上的同现。使用NCBI的内部工具 DNorm 和 tmChem 自动识别药物和疾病实体。
####整合系统
为了从所有参与的系统中获益并进一步改进结果，我们使用基于机器学习的方法来合并所有队伍的结果。 我们使用一个二元特征来表示每个参与者的系统输出。 对于DNER任务，每个特征表示相应系统是否返回了疾病概念。 对于CID任务，每个特征表示单个系统是否返回<Chemical ID, Disease ID>对。 我们的实现使用支持向量机。 我们对测试集执行了5折交叉验证，以评估我们的整体方法。

##Results
共有**25个团队**在CDR任务中提交了34个系统进行测试：16个DNER测试，18个CID任务测试。 每个团队被允许为每个任务提交至多三次（即它们的工具的三个不同版本） ，总共提交了86次运行。 这25个团队代表了来自四大洲的12个不同国家：澳大利亚（1），亚洲（12），欧洲（9）和北美（3）。
####DNER结果
共有16个团队成功提交了40次DNER结果，多个团队实现了高于85％的F分数，最高为`86.46％`（团队314），这是接近人类标注的结果（0.8875）。 平均精确度，召回率和F分-分别为78.99％，74.81％和76.03％。 通过使用CRF模型和在维基百科和Medline（团队296）上训练的word2vec模型获得最佳精度结果; 通过使用具有低级拼写校正的规则和字典获得最佳的召回结果（团队304）; 并且通过使用具有后处理的CRF模型（团队314）获得最佳F分数结果。 详细结果见附录表A1和A2。

####CID结果
共有18个团队成功地提交了46次运行的CID结果。 F分数范围从32.01％到`57.03％`（团队288），平均为43.37％。 所有团队通过简单的摘要水平同现法（精确度为16.43％，召回率为76.45％，F分数为27.05％）优于基线结果。 通过组合两个支持向量机（SVM）分类器获得了最好的结果，它们分别在句子和文档级别上训练（团队288）。 详细结果见附录表A3和A4。
####响应时间结果
DNER团队的平均响应时间为5.57秒，标准偏差为6.1，每个请求的时间为0.053至19.4秒。 CID团队的平均响应时间为8.38秒，标准偏差为6.5，范围为0.119到27.8秒。 最快的响应时间是由团队304获得的，团队304对于DNER和CID任务使用基于规则的系统。
####整合方法结果
使用整合方法，我们能够分别在DNER和CID的最佳个体系统上实现F评分的2.8％和10.1％的改善。

##讨论
BioCreative V的DNER任务表明，通过**自动命名实体识别技术**自动检测来自PubMed摘要的疾病实体是一个可行的任务。为了确定DNER和CID任务的难度，我们研究了多少团队正确地识别了测试集中的每个 `gold standard` DNER概念和CID关系。如表4所示，大约有〜5％的测试集中的1988年DNER概念未被任何一个队伍发现（即说明95％的概念被至少一个团队检索到）。 对于CID任务，1066个CID对中的128个（12％）没有被任何团队检测到。这意味着只有88％的CID关系被至少一个团队检索到，表明CID任务的难度。
任务结束后，我们调查了解常用的参与技术，工具和资源。结果显示在附录表A7中。总体而言，大多数团队使用机器学习技术开发他们的混合系统：主要是用于DNER任务的`CRF`和用于CID的`SVM`。其他两种一般方法是`模式匹配`（CID）和`字典查找`（DNER）。只有少数团队探索使用其他机器学习技术，如`最大熵`和`逻辑回归`。虽然基于机器学习的方法在一般情况下获得了最高的分数，值得注意的是，一些`基于规则`的系统也具有很强的竞争力，其中有一个团队分别在DNER和CID任务中排名第二和第三。当开发自己的系统时，许多团队调整了实现机器学习算法（例如LibSVM或CRFsuite）或通用NLP软件（例如Stanford CoreNLP或OpenNLP）的现有软件包。
在资源方面，许多队伍使用了外部数据库，其中UMLS和CTD是最常用的资源。
##结论
鉴于参与程度和团队结果，我们得出结论，CDR挑战任务成功执行并且对文本挖掘和生物化学研究社区做出了重要贡献。据我们所知，**`构建的语料库是同类疾病注释和药物疾病关系中最大的`**。我们认为这个数据集对于推进关系提取任务的文本挖掘技术将是非常宝贵的。此外，我们的注释数据包括跨句子边界（即不在相同句子中）断言的±30％的CDR关系。
与 BioNLP 中的大多数挑战任务不同，我们的任务旨在通过两个不同的请求为协助基于文献的生物实验提供实际的好处：（1） 所有文本挖掘的实体和关系将被标准化为数据库标识符，易于用于数据整理
（2） 通过Web服务，参赛队伍可以实时远程请求文本挖掘结果，而无需对文本挖掘工具采用和技术基础设施进行额外投资。通过这样做，我们希望BioNLP系统的最高水准（the state-of-the-art）得到进一步的发展，以在未来的发展努力中实现更高的互操作性和可扩展性标准。
