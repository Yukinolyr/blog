---
title: '计量经济学——回归不连续设计（RDD）'
date: '2025-12-28 13:42:10'
categories:
  - 笔记
tags:
  - Economics
thumbnail: 'images/rdd/0.png'
---
## 回归不连续设计（Regression Discontinuity Design, RDD）
> RDD是一种准实验方法，用于估计因果效应。它利用明确的断点或阈值，将样本分为处理组和对照组，从而在断点附近的小范围内模拟随机实验的效果。

---

### 为什么使用RDD？
#### 传统实验方法（RCT）的挑战
- 随机实验难以实施，成本高（例如，难以随机分配学生是否学习经济学）。
- 实验数据难以获取，现实中很少有高质量的实验数据。

#### 观察数据的局限
- 每个人是否学经济学（$D=1/0$）和每个人的收入$Y$，观察数据只能看到结果，难以建立因果联系。
- 存在个体的自选择问题（内生性），无法进行因果推断。

#### 案例分析：Bleemer & Mehta (2022)
##### 研究背景
- 2003年，加州大学圣克鲁兹分校（UCSC）经济学系实施了GPA限制政策：平均GPA达2.8及以上的学生才能选择经济学主修，否则只能选心理学或社会学等其他专业。
- 核心假设：GPA为2.79与2.80的学生在能力上的差距不大。
- 关键发现：GPA略高于2.8的学生选择经济学主修的概率提升了36%。

##### 研究方法
- 采用模糊断点回归设计（Fuzzy RDD）。
- 分析GPA略高于和略低于2.8的学生在收入上的差异。
![NHIS](/images/rdd/1.png)

##### 主要结论
- GPA略高于2.8的学生在早期职业的年薪约增加$22,000。
- 结果表明，收入差异源于专业选择，而非学生能力差异。
- 经济学教育的回报率相对较高。

---

### RDD的核心概念
#### 带宽选择
- 带宽决定了断点附近使用的数据范围（例如，选择GPA=2.8附近多大范围的学生）。
- 带宽选择需在两点间权衡：
  - 带宽过大：样本可比性较差，误差增大（如GPA=1.0与4.0的学生差异过大）。
  - 带宽过小：样本数量不足，统计推断方差过大（置信区间宽，估计的断点效应可能不显著）。

#### 带权重的曲线拟合（Weighted Curve Fitting，了解即可）
- 采用加权局部回归（Weighted Local Regression）进行拟合：
  - 离断点越近的观测值，权重越大。
  - 离断点较远的观测值，权重较小或为0。
- 常用权重函数（核函数，Kernel Function）：
  - 高斯核（Gaussian Kernel）
  - 三角核（Triangular Kernel）
  - Epanechnikov核函数

#### 驱动变量与处理状态（Running Variable and Treatment Status）
- **驱动变量（$X_i$）**：连续变量，用于定义断点（如例子中的GPA）。
- **处理状态（$D_i$）**：二元变量（1=接受处理，0=未接受处理），如“是否选择经济学专业”。
- 两种RDD类型的核心区别：
  - **精确断点回归（Sharp RD）**：驱动变量完全决定处理状态：
    $$D_{i}= \begin{cases}1, & 如果 X_{i} \geq 断点 \\ 0, & 如果 X_{i}<断点\end{cases}$$
  - **模糊断点回归（Fuzzy RD）**：断点仅改变处理概率，不完全决定处理状态（如UCSC案例中，并非所有GPA≥2.8的学生都会选择经济学主修，但断点显著提升了选择概率）。

#### 曲线拟合形式（Curve Fitting Models）
- 可选择线性、多项式等低阶回归模型（$f$函数的阶数决定拟合形式）：
  $$Y_{i}=\alpha+\beta \cdot \mathbb{I}\left(X_{i} \geq 断点\right)+\gamma \cdot f\left(X_{i}-断点\right)+\epsilon_{i}$$
- 也可使用非参数回归（如核密度估计，了解即可）。

---

### RDD的基本假设与验证方法
应用RDD需满足以下基本假设，且需通过实证检验确保结果有效性：

#### 核心假设
##### 驱动变量不可操控
- 参与者不能完全操控自己的驱动变量$X_i$（例如GPA），以影响处理状态$D_i$（如是否选经济学专业）。
- 若能操控$X_i$（如学生刻意将GPA从2.79提高到2.80），则断点附近样本存在自选择偏差，RDD估计失效。

##### 断点附近的可比性
- 断点附近的处理组和对照组“可比”：除处理状态外，其他基线特征（baseline covariates）无系统性差异。
- 核心要求：所有影响结果$Y$的基线协变量在断点处连续，不会发生显著跳跃。

#### 假设验证方法（Validation Methods）
##### 验证驱动变量不可操控
- 方法1：绘制驱动变量$X_i$的直方图，重点观察断点附近分布。若断点附近存在不规则堆积，可能表明人为操控。![NHIS](/images/rdd/2.png)
- 方法2：McCrary密度检验。评估断点处$X_i$的密度是否有显著跳跃，若有跳跃则说明驱动变量可能被操控。![NHIS](/images/rdd/3.png)

##### 验证断点附近样本可比
在使用RDD时,我们通常会基于某个“断点”将个体分为两个组,例如,收入高于某个阈值的个体和低于阈值的个体. 为了确保这两个组的可比性,我们需要验证他们的可观察前定特征 (baseline covatiates)是否在断点附近没有显著差异｡
- 方法1：比较断点上下两侧可观察特征的均值，检验是否存在显著差异。
- 方法2：局部平滑性检验。将基线协变量作为结果变量进行RDD回归，观察断点处是否有跳跃；或通过散点图+拟合线，观察协变量分布是否平滑。
- 方法3：用基线协变量预测结果变量$Y$，观察预测值$\hat{Y}$在断点处是否平滑。若$\hat{Y}$跳跃，则说明协变量在断点附近存在不平滑变化，样本不可比。
- 注意事项：
  - RDD仅能检验可观测协变量，无法完全排除潜在内生性问题。
  - 检验对象仅为“基线协变量”，不可用事后变量（否则属于机制检验）。
![NHIS](/images/rdd/4.png)
#### 其他稳健性检验（Robustness Check）
- **安慰剂检验（Placebo Tests）**：假设非实际断点（如将GPA断点设为2.7或2.9），检验该断点处是否存在$Y$的跳跃。若不存在跳跃，则原RDD结果更稳健。
- **带宽灵敏度（Bandwidth Sensitivity）**：更换不同带宽范围，验证结果是否在合理带宽内保持稳定。
- **多项式阶数灵敏度（Polynomial Order Sensitivity）**：改变拟合多项式的阶数，确保不同阶数下结果一致。
- **基线协变量灵敏度（Baseline Covariates Sensitivity）**：比较“包含协变量”与“不包含协变量”的回归结果，确保结果稳健。

#### RDD稳健性核心口诀
“两跳两不跳”：
- 两跳：处理变量（$D_i$）在断点处跳、结果变量（$Y_i$）在断点处跳。
- 两不跳：基线协变量在断点处不跳、驱动变量的密度在断点处不跳。

---

### RDD的优势与不足
#### 优势
- 断点附近的局部范围内，样本近似随机分配，因果推断能力强，在较弱假设下即可识别因果效应。
- 核心假设易于检验（虽无法完全验证）。

#### 不足
- 外部有效性较差，结果难以外推到断点之外的人群。
- 估计结果为**局部平均处理效应（LATE）**，而非整体平均处理效应（ATE）。例如，断点处的因果跳跃仅在带宽范围内成立，不能随意外推到全部人群。
- 若需外推结论，需充分讨论断点附近人群与整体人群的差异及关联。

---

### RDD的具体方程形式
以“是否进入一本大学”的第一阶段回归为例：
$$\begin{array}{rl}
EliteCol _{i,p,y,tr} &= \alpha _{0}+\alpha _{1}\cdot \mathbb{I}( Score _{i}\geq Cut_{p,y,tr}) \\
&+\theta _{1} \cdot f( Score _{i}-Cut_{p,y,tr}) \\
&+\theta _{2} \cdot f( Score _{i}-Cut_{p,y,tr})\cdot \mathbb{I}(Score_{i}\geq Cut_{p,y,tr})+\epsilon _{i,p,y,tr}
\end{array}$$
- 变量说明：
  - $EliteCol _{i,p,y,tr}$：观测个体是否进入一本大学（处理状态）。
  - $\mathbb{I}( Score _{i} ≥ Cut _{p,y,tr})$：驱动变量（高考分数）是否超过断点的指示变量。
  - $f( Score _{i}-Cut_{p,y,tr})$：驱动变量与断点距离的函数（如线性、二次函数）。
  - $\alpha_1$：关键因果系数（断点对处理状态的影响）。
  - $\theta_2$：斜率变化系数，捕捉断点前后函数趋势的变化。
  - $\epsilon _{i,p,y,tr}$：误差项。
![NHIS](/images/rdd/5.png)
---

### Fuzzy RD与IV的核心区别
1. **局部随机化假设**：RDD假设断点附近处理组和对照组的观测特征随机分布，理论上无需控制变量；而IV估计方程中，除替换内生变量为工具变量外，需纳入所有外生控制变量。
2. **拟合函数差异**：RDD方程包含驱动变量与断点距离的函数$f(\cdot)$，用于捕捉断点前后的趋势变化；IV估计方程中无此函数项。

---

### RDD估计系数的计算与解释
RDD系数的解读需结合具体案例（表格/图形），核心逻辑与IV存在关联（Fuzzy RD可类比IV估计）：

#### 案例：高考分数与一本大学、工资的关系（读表）
![NHIS](/images/rdd/6.png)
| 维度                | 说明                                                                 |
|---------------------|----------------------------------------------------------------------|
| Panel A（ITT）      | 因变量：工资对数（In Wage）；系数含义：高考分数≥一本线（断点）对工资的总体影响
| Panel B（第一阶段） | 因变量：是否进入一本大学（Elite College）；系数含义：断点对处理状态的影响（如系数0.185***表示过线者上一本概率高18.5%）。 |
| Panel C（LATE）     | 因变量：工资对数（In Wage）；系数含义：上一本大学对工资的因果效应（=Panel A系数/Panel B系数，如0.328***表示上一本者工资高32.8%）。 |
| 关键假设            | 超过一本线对工资的影响，仅通过“是否上一本大学”实现（无其他直接路径）。 |

#### 案例：GPA与经济学专业、工资的关系（读图）
![NHIS](/images/rdd/7.png)
- 左图（处理状态）：并不是所有超过断点的学生都会选择经济学主修，但断点的存在改变了选择的概率。GPA≥2.8的学生选择经济学专业的概率提升36个百分点（百分点，percentage points），对应RDD对处理状态的影响。
- 右图（结果变量）：
  - β系数（7989）：RDD对工资的直接影响，即ITT效应（GPA≥2.8者年薪平均增加$7,989）。
  - IV系数（22123）：通过经济学专业对工资的因果效应，即LATE（=ITT效应/处理概率提升幅度=7989/36%≈22123，与文档结果一致）。
- 我们会发现Fuzzy RD的效果近似于IV
  $$IV=\frac{RD \to Wage }{RD \to Econ }$$
- 关键假设：超过经济学主修线（cutoff）对工资的影响，仅通过“是否选择经济学主修”实现。

---

### Q1
Which of the following statements is NOT TRUE regarding Regression Discontinuity Design (RDD)?  

- A. RDD estimates the causal effect of a treatment by comparing outcomes for individuals just below and just above a threshold.  
- B. In a sharp RDD design, treatment assignment strictly follows the cutoff, meaning individuals above the threshold always receive treatment.  
- C. RDD requires a random assignment of running variable to individuals.  
- D. The assumption of continuity in potential outcomes is critical for the validity of RDD estimates.


#### 选项A：正确
RDD通过比较阈值上下附近个体的结果来估计处理的因果效应。

这是RDD的核心识别逻辑。RDD的核心思想是利用“运行变量（running variable）是否超过某一阈值”来划分处理组（阈值以上）和对照组（阈值以下），通过比较阈值“紧邻区域”内的个体结果差异，剔除运行变量连续变化带来的混淆，从而分离出处理的因果效应。

#### 选项B：正确
在精确RDD（sharp RDD）设计中，处理分配严格遵循临界值，即阈值以上的个体总是接受处理。
 
RDD分为“精确RDD（sharp RDD）”和“模糊RDD（fuzzy RDD）”：  
- 精确RDD的核心特征是“处理状态完全由运行变量决定”——运行变量超过阈值→100%接受处理，未超过→100%不接受处理，无任何模糊性；  
- 模糊RDD则是阈值仅影响处理概率（而非完全决定）。  

#### 选项C：错误
RDD要求运行变量被随机分配给个体。
 
这是对RDD假设的关键误解，核心混淆了RDD与随机对照试验（RCT）的要求：  
1. **RCT的核心要求**：处理状态（或干预变量）需随机分配，以保证处理组和对照组的基线特征均衡；  
2. **RDD的核心假设**：RDD不要求“运行变量（如年龄、分数）随机分配”（运行变量通常是内生的，如年龄是自然增长的，分数是个体努力的结果），而是要求两个关键假设：  
     - 驱动变量不可操控：个体无法精确控制运行变量，使其刚好落在阈值上下（避免“自选择”导致的混淆）；  
     - 断点附近的可比性：除处理状态外，其他影响结果的因素（潜在结果）在阈值处是连续的（即阈值上下个体的基线特征近似均衡）。  


#### 选项D：正确
潜在结果的连续性假设对RDD估计的有效性至关重要。

这是RDD的核心识别假设之一。潜在结果（potential outcomes）包括“个体未接受处理时的结果（Y₀）”和“接受处理时的结果（Y₁）”。

RDD假设：在阈值处，Y₀和Y₁都是连续的——即如果没有处理（如退休政策），阈值上下个体的锻炼行为（Y₀）不会出现突然跳跃；同理，处理后的潜在结果（Y₁）也不会突然跳跃。 

若该假设不成立，观察到的结果（如60岁后锻炼概率上升）可能来自潜在结果本身的不连续（而非处理效应），导致RDD估计偏误。因此，连续性假设是RDD有效性的关键。

### Q2
Many people in society have spent a lot of time working but little on physical exercises. People want to know the causal effects of working on physical exercise in China.

中国城市男性普遍以60岁为退休年龄，David利用这一自然阈值，采用回归断点设计（RDD）识别“工作状态”对“频繁参与体育锻炼”的因果效应。题目配套图1A（工作概率随年龄变化）和图1B（频繁锻炼概率随年龄变化），核心特征为：横轴为年龄，纵轴为对应行为概率，圆圈面积代表各年龄季度组（age-in-quarter bin）的样本量，拟合线为线性形式。
![NHIS](/images/rdd/8.png)
#### 小问a
Note that the areas of the circles are similar in both figures, which suggests that the sample size of each age-in-quarter bin is similar. Please explain why this is especially important for the validity of RDD identification.

##### 考点
RDD识别的核心假设——**无精确操纵假设（No Manipulation of the Running Variable）** 及密度检验（McGary Test）的应用。


RDD的因果识别依赖“阈值附近的个体近似随机分配到处理组/对照组”：  
- 处理组：年龄＞60岁（退休，工作概率下降）；  
- 对照组：年龄＜60岁（未退休，工作概率较高）；  
- 运行变量（running variable）：年龄季度（age-in-quarter）。  

若个体能**精确操纵运行变量**（如刻意调整年龄记录以刚好落在60岁阈值上下），则阈值附近的个体不再具有随机性，会导致选择性偏差（selection bias），破坏RDD的有效性。


文档明确指出：“各年龄季度组样本量相似，说明RDD通过了McGary检验（密度检验）”，其本质意义为：  
- 样本量均匀意味着**没有个体通过精确操纵年龄季度，刻意规避或选择阈值位置**（若存在操纵，会出现某一紧邻阈值的组样本量异常偏大/偏小）；  
- 由此可推断，阈值（60岁）附近的个体可被视为“近似随机分配”到处理组或对照组，满足RDD的无精确操纵假设，为因果效应识别提供了基础。

#### 小问b
Suppose that, for individual i, the outcome variable is noted as \(y_{i}\) (which is doing physical exercise) and the age in years as \(age _{i}\). The dummy variable, \(D_{i}\), equals to 1 if age in years is above 60, and zero for otherwise. Write down the regression equation to identify the RDD effects in the Figure 1B above, and point out the parameter representing the potential RDD effects. (Note that the fitted lines are linear)

##### 考点
线性拟合下RDD的标准回归形式、变量定义及因果效应参数解读。

RDD的线性回归方程需包含以下核心组件：  
1. 结果变量（因变量）：频繁参与体育锻炼的状态（$$y_i$$）；  
2. 处理变量（核心解释变量）：是否跨越阈值（$$D_i$$，年龄＞60岁为1，否则为0）；  
3. 中心化运行变量：$$age_i - 60$$（将运行变量的均值中心化到阈值处，消除阈值位置对截距的影响）；  
4. 交互项：$$D_i \cdot (age_i - 60)$$（允许处理组和对照组的线性拟合斜率不同，符合图中“分离线性拟合线”的特征）；  
5. 误差项（$$\varepsilon_i$$）：未观测到的混淆因素。

#### 回归方程（文档标准形式）
$$y_{i}=\beta_{0}+\beta_{1} D_{i}+\beta_{2}(age _{i}-60)+\beta_{3} D_{i} \cdot(age _{i}-60)+\varepsilon_{i}$$

#### 各变量定义
| 变量 | 含义 |
|------|------|
| $$y_i$$ | 个体$$i$$的结果变量：是否频繁参与体育锻炼（0=否，1=是） |
| $$D_i$$ | 处理变量：个体$$i$$年龄是否＞60岁（1=是，0=否） |
| $$age_i - 60$$ | 中心化后的运行变量：个体$$i$$的年龄与阈值（60岁）的差值 |
| $$D_i \cdot (age_i - 60)$$ | 处理变量与中心化运行变量的交互项：控制处理组和对照组的斜率差异 |
| $$\beta_0$$ | 基准截距：对照组（$$D_i=0$$）在阈值处（$$age_i=60$$）的预期结果值 |
| $$\beta_2$$ | 对照组的斜率：对照组中年龄每变化1岁，频繁锻炼概率的变化幅度 |
| $$\beta_3$$ | 斜率差异项：处理组与对照组的斜率差值 |
| $$\varepsilon_i$$ | 随机误差项：未观测到的影响锻炼行为的因素 |

#### 因果效应参数
$$\beta_1$$ 是RDD的核心因果效应参数，代表**阈值处的平均处理效应（Average Treatment Effect at the Cutoff, ATE at Cutoff）**：  
- 几何意义：图1B中两条线性拟合线在阈值（60岁）处的垂直距离；  
- 经济意义：年龄刚好超过60岁（退休）的个体，相比年龄刚好低于60岁（未退休）的个体，频繁参与体育锻炼的概率差异（即退休对锻炼行为的直接因果效应）。

## 总结
1. RDD的关键假设（无精确操纵）及验证方法（密度检验/样本量均匀性）；  
2. 线性RDD的回归方程构建逻辑（中心化运行变量、交互项的作用）；  
3. 处理效应参数的识别与经济意义解读。  
本质是检验“如何将RDD理论应用于具体政策阈值（退休年龄）”，核心是通过阈值附近的“局部随机性”分离因果效应。