# 根据提供的论文<Transcriptional regulation of nitrogen-associated metabolism and growt》的内容,我对该研究的可重现性以及每个图的重现难度和文本说明进行了分析。

## 1.组员
+ 王家傲2025303120130 https://github.com/okbk130
+ 窦佳艺2025303110105 https://github.com/doujiayi-2025
+ 周奕楠2025303120083 https://github.com/pidan431
+ 陈宇豪2025303110108 https://github.com/chenyuhaobio
+ 毕嘉榆2025303110101 https://github.com/Bijiayu-yyds
2. 可重现性总体评估
## 2. 可重现性总体评估

论文《Transcriptional regulation of nitrogen-associated metabolism and growth》发表在2018年的Nature杂志上，其原始数据处于文章末尾，分析代码在Github上发布https://github.com/agaudinier/Gaudinier2018。其可重现性总体上较好，但不同图件所对应的方法层级差异较大。原文公开了 RNA-seq 数据的 accession（GSE107988），同时提供了 Supplementary Tables、R code 和 Cytoscape files，这使得结果复核、网络再导出以及部分统计图重建成为可能。尤其对于基于补充表的 Figure 5a、Figure 5b/c，用户可以较直接地依赖表格数据完成再统计和再绘图。

但从方法学深度来看，Fig. 1a、1b、4a 的“完全复现”并不只是绘图问题，而涉及增强型 yeast one-hybrid（Y1H）筛选、公开氮响应转录组数据整合、TF–target 相关性分析、NeCORR 排名、Gini 相关显著性分析以及 Cytoscape 网络可视化等多个步骤。Supplementary Information 中明确给出了被筛选的 promoter、TF–promoter 互作、公开表达数据集、相关性分析结果以及 NeCORR 排名所依赖的数据结构，说明作者在数据共享方面较充分；但这些图若要从头严格复现，仍需要较强的实验与生信整合能力，因此其真实重现门槛并不低。


---

## 3. 各图重现难度及文本说明

### Figure 1a — 氮代谢相关启动子与转录因子的组合式互作网络

**文本说明：**  
该图展示的是与氮代谢、氮信号传导及其他氮相关生物过程有关的基因启动子，与大量转录因子之间的组合式调控关系网络。按照原文方法，这一图首先不是由表达矩阵直接绘制出来的，而是基于增强型 yeast one-hybrid（enhanced Y1H）筛选获得 TF–promoter 相互作用，再将结果组织成网络图。Supplementary Information 显示，作者对一组氮相关启动子进行了 Y1H 筛选，并整理了 TF–promoter interactions；图中矩形代表 promoter，椭圆代表 transcription factor，菱形代表同时兼具 promoter 与 TF 身份的基因，节点颜色对应不同氮相关生物过程，灰色连线表示 TF–promoter 结合关系。也就是说，Fig. 1a 的本质是实验筛选得到的调控互作网络可视化结果。

**重现难度：较高。**  
如果只是复核结果并重新导出图形，难度不算高；但若严格按照原文方法从头复现，则需要获得同样的 promoter 集合、TF 文库以及增强型 Y1H 筛选结果，再在 Cytoscape 中完成网络组织和版式调整。这意味着它并不是单纯依赖代码即可完整重建的图，而是带有明显实验基础的网络图。

---

### Figure 1b — 硝酸同化相关子网络的聚焦展示

**文本说明：**  
Fig. 1b 可以理解为在 Fig. 1a 全局 TF–promoter 互作网络基础上，对硝酸同化相关模块进行的进一步抽取与聚焦展示。Supplementary Information 中专门列出了 “TF-promoter interactions for nitrate assimilation subnetwork”，说明作者从全局 Y1H 网络中提取了与硝酸同化核心基因相关的子网络，用来更清楚地展示多个转录因子对关键氮代谢节点的组合调控关系。因此，这一图的方法学基础与 Fig. 1a 一致，核心仍然是增强型 Y1H 获得的 TF–promoter 互作，只是网络范围从全局图收缩为功能更聚焦的子图。

**重现难度：较高。**  
这张图虽然比 Fig. 1a 结构更紧凑，但并不代表复现更容易。因为它要求先有全局互作筛选结果，再准确识别并抽取“nitrate assimilation subnetwork”，最后按论文风格重新布局。换言之，Fig. 1b 的关键难点不在绘图函数，而在于上游互作识别与子网络提取逻辑。

---

### Figure 4a — 硝酸响应转录调控网络

**文本说明：**  
Fig. 4a 展示的是原文提出的 nitrate-responsive transcriptional regulatory network。这幅图相较于 Fig. 1a/1b 已不再只是单纯的 Y1H 互作展示，而是建立在多层信息整合之上：作者一方面利用 Y1H 得到 TF–promoter 互作框架，另一方面整合公开氮处理表达数据集，对 TF–target 关系进行 Pearson/Spearman 相关分析，并进一步进行 NeCORR 排名与 Gini correlation 显著性评估，最后在 Cytoscape 中构建并可视化硝酸响应调控网络。Supplementary Information 对公开表达数据集、相关性结果和 NeCORR 分析均有单独说明，正文也给出了 Cytoscape、RNA-seq 与相关统计分析的资源线索。因此，Fig. 4a 本质上是一个实验互作网络与转录组响应信息耦合后的整合型调控网络图。

**重现难度：高。**  
这是本批图中方法学要求最高的一类。若要真正按原文流程重建，不仅要有 Y1H 互作数据，还要整合公开氮响应表达谱、完成相关性与网络排序分析，并在 Cytoscape 中映射网络拓扑与表达变化。你当前脚本已经能通过 RCy3 打开 `.cys` 会话并导出网络图，这对于“结果级复现”很有效；但如果按原文方法定义“全流程复现”，其复杂度明显高于 Fig. 5a 和 Fig. 5b/c。

---

### Figure 5a — 不同扰动背景下差异表达基因数量统计图

**文本说明：**  
代码直接读取 SupplementalTable16 的两个工作表，分别统计全部显著差异基因和 YNM 中显著差异基因的数量，再用双层横向条形图进行展示。因此，这一图并不依赖重新做原始差异分析，而是基于作者已公开的统计结果进行再汇总和再绘图。

**重现难度：中等。**  
主要难点在于正确理解 SupplementalTable16 中各数据集的 logFC 与 adjusted P value 字段，并保持与原文一致的显著性判定阈值。相对于 Fig. 1a、1b、4a，这一图的复现更偏向数据整理与可视化实现。

---

### Figure 5b / 5c — 转录调控因子与氮代谢相关基因的反馈热图

**文本说明：**  
脚本分别预设了一组转录调控因子相关基因和一组氮代谢/转运相关基因，提取其在不同突变体数据集中的 logFC，并把不显著的项置零，最后生成热图。原文 Figure 5 的主题是“遗传扰动后对氮代谢或调控的转录反馈”。

**重现难度：中等偏上。**  
虽然不涉及重新跑原始实验，但热图比条形图更容易受到基因筛选、显著性阈值、重复标签处理、行列排序与配色方案等因素影响。
