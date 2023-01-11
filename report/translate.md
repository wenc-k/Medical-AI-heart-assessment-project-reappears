# report

## 翻译

### Abstract

#### Original

Accurate assessment of cardiac function is crucial for the diagnosis of cardiovascular disease[1](https://www.nature.com/articles/s41586-020-2145-8#ref-CR1), screening for cardiotoxicity[2](https://www.nature.com/articles/s41586-020-2145-8#ref-CR2) and decisions regarding the clinical management of patients with a critical illness[3](https://www.nature.com/articles/s41586-020-2145-8#ref-CR3). However, human assessment of cardiac function focuses on a limited sampling of cardiac cycles and has considerable inter-observer variability despite years of training[4](https://www.nature.com/articles/s41586-020-2145-8#ref-CR4),[5](https://www.nature.com/articles/s41586-020-2145-8#ref-CR5).Here, to overcome this challenge, we present a video-based deep learning algorithm—EchoNet-Dynamic—that surpasses the performance of human experts in the critical tasks of segmenting the left ventricle, estimating ejection fraction and assessing cardiomyopathy. Trained on echocardiogram videos, our model accurately segments the left ventricle with a Dice similarity coefficient of 0.92, predicts ejection fraction with a mean absolute error of 4.1% and reliably classifies heart failure with reduced ejection fraction (area under the curve of 0.97). In an external dataset from another healthcare system, EchoNet-Dynamic predicts the ejection fraction with a mean absolute error of 6.0% and classifies heart failure with reduced ejection fraction with an area under the curve of 0.96. Prospective evaluation with repeated human measurements confirms that the model has variance that is comparable to or less than that of human experts. By leveraging information across multiple cardiac cycles, our model can rapidly identify subtle changes in ejection fraction, is more reproducible than human evaluation and lays the foundation for precise diagnosis of cardiovascular disease in real time. As a resource to promote further innovation, we also make publicly available a large dataset of 10,030 annotated echocardiogram videos.

#### Translate

准确的心功能评估对于**心血管疾病的诊断**、**心脏毒性的筛查**和**危重患者的临床管理决策**至关重要。

然而，人类对心脏功能的评估只关注有限的心脏周期采样，尽管经过多年的训练，但观察者之间的差异还是相当大的。在这里，为了克服这一挑战，我们提出了一种基于视频的深度学习算法EchoNet - Dynamic，它在**分割左心室**、**估计射血分数**和**评估心肌病**等关键任务中的表现超过了人类专家。

在超声心动图视频的训练下，我们的模型准确分割左心室，**Dice相似系数为0.92**，预测射血分数的**平均绝对误差为4.1%**，并使用减小的射血分数(曲线下面积为0.97)对心力衰竭进行可靠分类。

在另一个医疗系统的外部数据集中，EchoNet-Dynamic预测了射血分数，平均绝对误差为6.0%，并通过曲线下面积为0.96的降低的射血分数对心力衰竭进行了分类。通过重复人体测量进行的前瞻性评估证实，该模型的方差与人类专家的方差相当或更小。

通过利用多个心动周期的信息，我们的模型可以快速识别射血分数的细微变化，**比人类评估更具重复性**，为实时准确诊断心血管疾病奠定基础。作为促进进一步创新的资源，我们还公开了10,030个带注释的超声心动图视频的大型数据集。

### Introduction

#### Original

Cardiac function is essential for maintaining normal systemic tissue perfusion with cardiac dysfunction manifesting as dyspnea, fatigue, exercise intolerance, fluid retention and mortality[1](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R1),[2](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R2),[3](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R3),[5](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R5)-[8](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R8). Impairment of cardiac function is described as “cardiomyopathy” or “heart failure” and is a leading cause of hospitalization in the United States and a growing global health issue[1](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R1),[9](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R9),[10](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R10). A variety of methodologies have been used to quantify cardiac function and diagnose dysfunction. In particular, left ventricular ejection fraction (EF), the ratio of change in left ventricular end systolic and end diastolic volume, is one of the most important metrics of cardiac function, as it identifies patients who are eligible for life prolonging therapies[7](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R7),[11](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R11). However, echocardiography is associated with significant inter-observer variability as well as inter-modality discordance based on methodology and modality[2](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R2),[4](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R4),[5](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R5), [11](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R11)-[14](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R14).

Human assessment of EF has variance in part due to the common finding of irregularity in the heart rate and the laborious nature of a calculation that requires manual tracing of ventricle size to quantify every beat[4](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R4),[5](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R5). While the American Society of Echocardiography and the European Association of Cardiovascular Imaging guidelines recommend tracing and averaging up to 5 consecutive cardiac cycles if variation is identified, EF is often evaluated from tracings of only one representative beat or visually approximated if a tracing is deemed inaccurate[5](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R5),[15](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R15). This results in high variance and limited precision with inter-observer variation ranging from 7.6% to 13.9%[4](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R4),[12](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R12)-[15](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R15). More precise evaluation of cardiac function is necessary, as even patients with borderline reduction in EF have been shown to have significantly increased morbidity and mortality[16](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R16)-[18](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R18).

With rapid image acquisition, relatively low cost, and without ionizing radiation, echocardiography is the most widely used modality for cardiovascular imaging[19](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R19),[20](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R20). There is great interest in using deep learning techniques on echocardiography to determine EF[21](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R21)-[23](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R23). Prior attempts to algorithmically assess cardiac function with deep learning models relied on manually curated still images at systole and diastole instead of using actual echocardiogram videos and they have substantial error compared to human evaluation of cardiac function with R2 ranging between 0.33 and 0.50[21](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R21),[22](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R22). Limitations in human interpretation, including laborious manual segmentation and inability to perform beat-to-beat quantification may be overcome by sophisticated automated approaches[5](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R5),[22](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R22),[23](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R23). Recent advances in deep learning suggest that it can accurately and reproducibly identify human-identifiable phenotypes as well as characteristics unrecognized by human experts[25](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R25)-[28](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R28).

To overcome current limitations of human assessment of cardiac function, we propose EchoNet-Dynamic, an end-to-end deep learning approach for left ventricular labeling and ejection fraction estimation from input echocardiogram videos alone. We first perform frame-level semantic segmentation of the left ventricle with weakly supervised learning from clinical expert labeling. Then, a 3-dimensional (3D) convolutional neural network (CNN) with residual connections predicts clip level ejection fraction from the native echocardiogram videos. Finally, the segmentations results are combined with clip level predictions for beat-by-beat evaluation of EF. This approach provides interpretable tracings of the ventricle, which facilitate human assessment and downstream analysis, while leveraging the 3D CNN to fully capture spatiotemporal patterns in the video[5](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R5),[29](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R29),[30](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8979576/#R30).

#### Translate

心脏功能对于维持正常的全身组织灌注至关重要。

心脏功能障碍表现为呼吸困难、疲劳、运动不耐受、液体滞留以及死亡风险的增加。

心脏功能受损被描述为心肌病或心力衰竭，是美国住院的主要原因，也是日益严重的全球健康问题。已经有多种方法被用于量化心脏功能和诊断功能障碍。

特别是**测量左心室射血分数**，即左心室收缩末期和舒张末期容积的变化比率。这是衡量心脏功能的最重要指标之一，因为它可以确定哪些患者有资格接受延长生命的治疗。然而，射血分数的评估与可观的观察者间变异性以及基于方法学和模态的模态间不一致性相关。

人类对射血分数的评估存在差异，一部分原因是发生心率不规则的情况很普遍以及计算耗费大（计算需要手动追踪心室大小以量化每一次搏动）。

尽管美国超声心动图学会和欧洲心血管成像协会的指南共同建议，如果发现变异出现，最多可追踪和平均五个连续心动周期，但是射血分数通常仅根据一个代表性搏动的轨迹进行评估，如果追踪被认为不准确，则进行视觉近似。这种做法导致了评估结果的方差很大，精度也有限，其中观测者间方差在7.6%到13.9%之间。由此可见更精确的心功能评估是必要的，因为即使是射血分数临界降低的患者也可能导致发病率和死亡率的显著增加。

超声心动图图像采集速度快，成本相对较低，没有电离辐射，是最广泛使用的心血管成像方式。人们对**使用超声心动图的深度学习技术**来确定射血分数很感兴趣。用深度学习模型算法评估心脏功能的*过往尝试依赖于心脏收缩期和心脏舒张期手动精选的静态图像*，而不是使用实际的超声心动图视频，这些模型与人类心脏功能评估相比有很大误差，R^2^在0.33和0.50之间。

而人工评估的局限性（包括费力的手工分割和和无法进行逐拍量化）可以通过复杂的自动化方法来克服。

深度学习的最新进展表明，它可以准确、可重复地识别人类可识别的表型以及人类专家无法识别的特征。

为了克服目前人类心脏功能评估的局限性，我们提出了EchoNet-Dynamic，一种端到端的深度学习方法，用于**标记左心室**和**仅从输入超声心动图视频中估计射血分数**。

我们首先使用来自临床专家标记的**弱监督学习**来执行左心室的帧级(frame-level)语义分割。

然后，一个具有残差连接的三维卷积神经网络(CNN)从原始的超声心动图视频中预测片段级（clip-level）的射血分数。

最后，将分割结果与片段级（clip-level）预测相结合，以产生射血分数的逐拍评估。这种方法提供了可解释的心室轨迹，便于人类评估和下游分析，同时利用CNN完全捕捉视频中的时空模式。
