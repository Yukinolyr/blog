---
title: '计量经济学——工具变量(IV)'
date: '2025-12-16 18:45:59'
categories:
  - 笔记
tags:
  - Economics
thumbnail: 'images/IV/5.png'
---
## 通过这一章，你应该学会的东西：
- 内生性与外生性（应该之前就学会了吧？）
- 工具变量的条件：相关性条件（Relevance Condition）& 排他性约束（Exclusion Restriction）
- 两阶段最小二乘法（Two-Stage Least Squares, 2SLS）
- 主分层与依从类型（Principal Stratification and Compliance Types）
- 意向治疗效应（ITT）/局部平均处理效应（LATE）/处理者平均效应（TOT）

> HW说工具变量是一个非必要不使用的工具————是药三分毒，而IV无疑是一剂重药。那么问题来了，为什么考试还要学这些东西？？总之还是学吧。

## 工具变量（Instrumental Variables, IV）
### 为什么使用工具变量（IV）？
- 在实际研究中，通过简单回归（如普通最小二乘法OLS）获取因果关系可能不够可靠。当自变量$X$与误差项$u$相关（即存在内生性问题）时，OLS无法给出准确的因果效应估计。
- IV方法通常用于解决OLS中遇到的以下问题：
  1.遗漏变量偏误（Omitted variables bias）
  2.测量误差（Measurement error）
  3.反向因果（Reverse causality）

- 当模型中存在内生性自变量$X$时，可使用IV估计。
- 一个示例：教育对工资的影响？

  - 设定模型：$$Wage_{i}=\beta_{0}+\beta_{1} \cdot Edu_{i}+\beta_{2} \cdot Ability _{i}+v_{i}$$
  - 这是一个我们都熟悉的问题：研究者无法观测到$Ability _{i}$（能力），若直接使用OLS在未纳入$Ability _{i}$的情况下进行回归，会出现遗漏变量问题。
  - 考虑错误设定的回归方程：$$Wage _{i}=\beta_{0}+\beta_{1} \cdot Edu_{i}+u_{i}$$
    其中，$$u_{i}=\beta_{2} \cdot Ability _{i}+v_{i}$$
  - 在该错误设定下，OLS估计量为：$$\hat{\beta}_{1_{OLS}}=\frac{cov\left( Wage _{i}, Edu_{i}\right)}{Var\left(Edu_{i}\right)}$$
  - 我猜你已经不熟OLS是什么了。此乃自然之理，因此让我们来温习一下：
  OLS的核心思想是**最小化残差平方和**（$\sum_{i=1}^n (Wage_{i} - \hat{Wage}_{i})^2$，其中$\hat{Wage}_{i} = \hat{\beta}_{0} + \hat{\beta}_{1} \cdot Edu_{i}$为拟合值），通过求解该最小化问题，最终得到教育水平$Edu_{i}$的OLS估计量：  
  $$\hat{\beta}_{1_{OLS}} = \frac{cov(Wage_{i}, Edu_{i})}{Var(Edu_{i})}$$  
  - **分子$cov(Wage_{i}, Edu_{i})$**：工资与教育的协方差，衡量两者的线性相关程度，计算公式为：  
   $$cov(Wage_{i}, Edu_{i}) = E\left[(Wage_{i} - E[Wage_{i}]) \cdot (Edu_{i} - E[Edu_{i}])\right]$$  
   （$E[\cdot]$表示期望，即总体均值）。
  - **分母$Var(Edu_{i})$**：教育水平的方差，衡量教育水平的总体波动程度，计算公式为：  
   $$Var(Edu_{i}) = E\left[(Edu_{i} - E[Edu_{i}])^2\right]$$  
  - OLS估计量本质是“工资与教育的线性关联强度”除以“教育自身的波动强度”，试图捕捉$Edu_{i}$每变化1单位时，$Wage_{i}$的平均变化幅度。

  - 将实际数据生成过程
  
  $$Wage_{i}=\beta _{0}+\beta _{1}\cdot Edu_{i}+\beta _{2}\cdot Ability _{i}+v_{i}$$代入，可得：
    $$\begin{aligned} \hat{\beta}_{1_{OLS}} & =\frac{cov\left(\beta_{0}+\beta_{1} Edu_{i}+\beta_{2} Ability_{i}+v_{i}, Edu_{i}\right)}{Var\left(Edu_{i}\right)} \\ & =\beta_{1}+\frac{\beta_{2} \cdot cov\left( Ability _{i}, Edu_{i}\right)}{Var\left(Edu_{i}\right)}+\frac{cov\left(v_{i}, Edu_{i}\right)}{Var\left(Edu_{i}\right)} \end{aligned}$$
  - 在真实模型中，若$v_{i}$与$Edu$独立（即$v_{i}$不存在额外内生性问题），则：$$E\left[\hat{\beta}_{1_{OLS}}\right]=\beta_{1}+\frac{\beta_{2} \cdot E\left[Cov\left( Ability _{i}, Edu_{i}\right)\right]}{E\left[Var\left(Edu_{i}\right)\right]}$$
  - 显然，若$Ability _{i}$与$Edu$正相关（例如更有能力的人往往接受更多教育），则$E[\hat{\beta}_{1_{OLS}}] \neq\beta_{1}$，这意味着OLS估计存在偏差和不一致性。

### 工具变量（IV）的核心思想是什么？
- IV的核心思想是寻找一个外生工具变量$Z_{i}$，以提供$X_{i}$的“纯净变化”（外生变异，exogenous variation）。直观来看，IV像一个“杠杆”或“筛子”，利用$Z_{i}$提取$X_{i}$中与误差项$u$无关的那部分变化，从而帮助识别$X_{i}$对$Y_{i}$的真正因果影响。
- 有效的IV需满足两个关键条件：
  1. 相关性条件（Relevance Condition）：工具变量$Z_{i}$必须与$X_{i}$显著相关，即$$Cov(Z_{i}, X_{i}) \neq0$$。这确保$Z_{i}$能为$X_{i}$提供有意义的变异。若$Z_{i}$与$X_{i}$弱相关，则为无效工具变量（弱工具变量问题，weak instrument problem）。
  2. 排他性约束（Exclusion Restriction）：工具变量$Z_{i}$必须与误差项$u_{i}$不相关，即$$Cov(Z_{i}, u_{i})=0$$。这意味着$Z_{i}$只能通过$X_{i}$影响$Y_{i}$，而不能通过其他路径直接影响$Y_{i}$。
- 第一条（相关性）可通过第一阶段回归检验，而第二条（外生性）通常需要理论或情境支持，无法直接通过统计检验验证。
- 由于外生性难以严格验证，这也是IV方法有时遭受质疑的原因。

### 工具变量示例
- 在研究教育对工资的影响时，可使用出生地的教育政策变化（例如义务教育年限的变革）作为工具变量。只要该政策变化能影响教育年限，且不直接影响个体工资（除通过教育途径外），即可视为满足外生性和相关性条件。

### IV估计的基本设定
考虑基本模型：

$$Y_{i}=\beta _{0}+\beta _{1}X_{i}+u_{i}$$，
且

$$Cov(X_{i},u_{i}) \neq 0$$。

若找到满足
$$Cov(Z_{i}, X_{i}) \neq0$$
且

$$Cov(Z_{i}, u_{i})=0$$的工具变量$Z_{i}$，

则IV估计量为：

$$\hat{\beta}_{IV}=\frac{Cov\left(Z_{i}, Y_{i}\right)}{Cov\left(Z_{i}, X_{i}\right)}$$


该比率的直观含义是：用$Z_{i}$与$Y_{i}$的关系（整体影响）除以$Z_{i}$与$X_{i}$的关系（对$X_{i}$的纯影响），即可获得$X_{i}$对$Y_{i}$的纯因果效应。

### 两阶段最小二乘法（Two-Stage Least Squares, 2SLS）
当存在多个控制变量或多个内生变量时，比率形式的IV估计不再便捷，常用方法为两阶段最小二乘法（2SLS）：
1. 第一阶段（First Stage）：$$X_{i}=\pi _{0}+\pi _{1} Z_{i}+ 其他外生控制变量 +\eta_{i}$$
   通过该回归得到拟合值$$\hat{X}_{i}$$，$$\hat{X}_{i}$$仅包含$X_{i}$中由$Z_{i}$和其他外生因素解释的部分，已剔除内生成分。
2. 第二阶段（Second Stage）：$$Y_{i}=\beta_{0}+\beta_{1} \hat{X}_{i}+ 其他外生控制变量 +\tilde{u}_{i}$$
   对该阶段进行OLS估计，可得到$\beta_{1}$的一致估计量。

若将$Z_{i}$直接代入主方程：$$Y_{i}=\delta _{0}+\delta _{1}Z_{i}+其他外生控制变量 +u_{i}$$，则$\delta_{1}$为$Z$对$Y$的简化式效应（reduced form effect），而第一阶段的$\pi_{1}$表示$Z$对$X$的影响。两者的比率可识别$X$对$Y$的因果效应。

> 简单来讲，2SLS在第一阶段先对带有内生性的变量进行处理，通过工具变量来消除这个变量的内生性，之后再将消除内生性的变量带入原方程，此时变量与残差项完全正交，我们就可以通过方程识别其中的因果效应。


### IV估计结果的解释
在有异质性处理效应（不同个体对处理的反应不同，可能比较难以理解，往下看）时，2SLS估计通常被解释为“局部平均处理效应”（Local Average Treatment Effect, LATE）：
- LATE指的是那些会因工具变量变化而改变处理状态的个体的平均处理效应，这类人群被称为“依从者”（Compliers）。
- 简单来说，IV 无法估计 “所有群体” 的平均处理效应，只能精准识别 “受工具变量影响而行动” 的特定群体的效应 —— 这一 “局部” 群体的平均效应，就是 LATE。
- 因此，IV估计结果的解释需慎重，其外部有效性常常需要额外讨论（例如与OLS结果对比系数大小，或讨论群体中依从者的特征）。

### 主分层与依从类型（Principal Stratification and Compliance Types）
在存在不完全遵从（noncompliance）的情境下，可根据研究对象在不同激励（或指派）状态下对处理的响应方式进行分类，该分类方式称为主分层（Principal Stratification）。每个个体根据“假设分配”为处理组或对照组时是否会接受处理，被分为以下四类：
- 依从者（Compliers）：有激励时接受处理，无激励时不接受处理。定义为：$$D_{i}(1)=1$$且$$D_{i}(0)=0$$
- 总是接受者（Always-takers）：不论是否受到激励，都接受处理。定义为：$$D_{i}(1)=D_{i}(0)=1$$
- 从不接受者（Never-takers）：不论是否受到激励，都不接受处理。定义为：$$D_{i}(1)=D_{i}(0)=0$$
- 反向遵从者（Defiers）：有激励时不接受处理，无激励时接受处理。定义为：$$D_{i}(1)=0$$且$$D_{i}(0)=1$$

上述定义中的$D_{i}(z)$表示当工具变量（激励）$Z_{i}=z$时，个体$i$的处理状态$D_{i}$。

然而，根据观测数据（$Z_{i}, D_{i}$的组合），我们无法直接辨别个体属于哪一种类型，因为这些类型的定义基于反事实状态下的行为：

|        | $Z_{i}=1$               | $Z_{i}=0$               |
| ------ | ----------------------- | ----------------------- |
| $D_{i}=1$ | 依从者/总是接受者       | 反向遵从者/总是接受者   |
| $D_{i}=0$ | 反向遵从者/从不接受者   | 依从者/从不接受者       |

所以在没有更多假设的情况下，无法从上述表格中观测到的联合状态唯一识别出四类人群。

### 意向治疗效应（ITT）的分解
意向治疗效应（ITT effect）可分解为各子群体ITT效应的组合：

$$ITT=ITT_{c}× Pr(依从者)+ITT_{a}× Pr(总是接受者)+ITT_{n}× Pr(从不接受者)+ITT_{d}× Pr(反向遵从者)$$


其中：
$$ITT_{c}=\mathbb{E}\left[Y_{i}\left(1, D_{i}(1)\right)-Y_{i}\left(0, D_{i}(0)\right) | D_{i}(1)=1, D_{i}(0)=0\right]$$


$$ITT_{a}=\mathbb{E}\left[Y_{i}\left(1, D_{i}(1)\right)-Y_{i}\left(0, D_{i}(0)\right) | D_{i}(1)=D_{i}(0)=1\right]$$，其余类似。

在单调性假设和排他性约束下，该分解可简化为：
$$\begin{aligned} ITT= & ITT_{c} × Pr(依从者)+ITT_{a} × Pr(总是接受者) \\ & +ITT_{n} × Pr(从不接受者)+0 \quad[单调性假设] \\ = & ITT_{c} × Pr(依从者)+0 × Pr(总是接受者) \\ & +0 × Pr(从不接受者) \quad[排他性约束] \\ = & ITT_{c} × Pr(依从者) \end{aligned}$$

因此，$ITT _{c}$可通过非参数方法识别：


$$ITT_{c}=\frac{ITT}{Pr(依从者)}$$


$$ITT_{c}=\frac{\mathbb{E}\left[Y_{i} | Z_{i}=1\right]-\mathbb{E}\left[Y_{i} | Z_{i}=0\right]}{\mathbb{E}\left[D_{i} | Z_{i}=1\right]-\mathbb{E}\left[D_{i} | Z_{i}=0\right]}=\frac{Cov\left(Y_{i}, Z_{i}\right)}{Cov\left(D_{i}, Z_{i}\right)}$$

$ITT _{c}$在单调性假设和排他性约束下，可解释为依从者的局部平均处理效应（LATE）：

$$ITT_{c}={LATE}={\mathbb {E}}[Y_{i}(1)-Y_{i}(0) | D_{i}(1)=1,D_{i}(0)=0]$$

我们称之为**Wald估计量**。

如你所见，Wald 估计量是工具变量（IV）框架中简单场景下计算局部平均处理效应（LATE）的核心方法，特指当存在「一个工具变量（Z）、一个内生变量（D）、一个结果变量（Y）」时，通过「简化式效应与第一阶段效应的比值」得到的因果效应估计量，本质是两阶段最小二乘法（2SLS）在单工具变量场景下的特例。



### ITT、LATE与TOT的对比
- 意向治疗效应（ITT）：易于识别（衡量指派处理的平均效应），但无法反映仅针对实际接受处理者的效应。
- 局部平均处理效应（LATE）：在标准假设下，通过IV可识别的明确因果效应。它是一个局部参数，不一定等于ATE（整体平均处理效应）或TOT（处理者平均效应），但通常更易解释和识别（仅针对工具变量诱导的依从者群体）。
- 处理者平均效应（TOT）：试图捕捉实际接受处理者的效应，但识别TOT通常需要工具变量和额外假设。在计算TOT并声称其为特定个体的效应时，实际假设处理组内的参与者是随机的，且未处理者不受溢出效应影响。这些假设更为严格：若存在溢出效应或同伴效应，假设可能不成立。

简单来讲，ITT覆盖的群体最广，包括依从者+总是接受+总不接受。它是无偏的保守估计，因此会低估效应，因为会有总不接受的群体将效应冲淡。LATE通过IV进行无偏的识别，只考虑依从者，因此会丢失掉总是接受群体的效应。TOT是理想的精准目标，因为他包括依从者+总是接受者两个群体，涵盖了受原变量影响的所有群体，但我们无法完全无偏估计，因为IV拆不了具有自选择偏差的总是接受者。

### Which statement is NOT TRUE?  
(a) LATE is determined by the ratio of reduced form to first-stage estimates.  

(b) ITT captures the causal effect of being assigned to treatment.  

(c) TOT captures the average causal effect for individuals who actually receive the treatment.  

(d) LATE equals ITT multiplying the difference in compliance rates between treatment and control groups.  

#### 答案
(d)  

#### 解析
工具变量（IV）框架中，核心效应指标的关系及关键定义如下：  
- (a) 正确：局部平均处理效应（LATE）的核心计算逻辑是「简化式效应（Reduced Form）与第一阶段效应（First Stage）的比值」，即通过工具变量诱导的结果差异除以工具变量对内生变量的影响差异，剥离内生性偏差。  
- (b) 正确：意向性治疗效应（ITT）聚焦「分组分配本身」的因果效应，无论研究对象是否实际接受处理（如是否使用分配的住房券、是否就读中奖的特许学校），仅反映“被分配到处理组”与“被分配到对照组”的结果差异，是无偏的保守估计。  
- (c) 正确：处理组平均处理效应（TOT）针对「实际接受处理的群体」，衡量该群体从处理中获得的平均收益（需通过IV矫正自选择偏差，否则OLS估计会失真）。  
- (d) 错误：核心公式为：  
     $$LATE = \frac{Reduced Form}{First Stage} = \frac{ITT}{Pr(ET=1 | D=1) - Pr(ET=1 | D=0)}$$  

     其中，$Pr(ET=1 | D=1) - Pr(ET=1 | D=0)$ 表示「处理组依从率与对照组依从率的差异」（即第一阶段效应）。LATE是ITT矫正“不依从稀释效应”后的结果，需用ITT除以依从率差异，而非乘以。 



### Experienced Teacher and Students’ Academic Performance
Student test scores predict future earnings. Teacher experience is hypothesized to improve test scores.
This section examines whether experienced teachers causally improve students’ standardized test scores (0–100).
#### Q1
Interpret $\hat{\beta}_{0}=60$ and $\hat{\beta}_{1}=5.0$ in the observational regression:  
$$Y_{i}=60+5.0 E T_{i}+\varepsilon_{i}$$  
(Standard errors for intercept and slope are 20 and 2.5, respectively.)

##### 答案
- $\hat{\beta}_{0}=60$：未被经验教师授课（$ET_{i}=0$）的学生，其标准化考试成绩的预测值为60分（满分100分）。  
- $\hat{\beta}_{1}=5.0$：在该观测回归中，被经验教师授课（$ET_{i}=1$）的学生，平均而言考试成绩比未被经验教师授课的学生高5.0分。

##### 解析
观测回归的系数仅反映“变量关联”而非“因果关系”：  
- 截距项$\hat{\beta}_{0}$对应“解释变量取0时的被解释变量均值预测值”，此处即“无经验教师”组的基准预测成绩；  
- 斜率项$\hat{\beta}_{1}$反映“解释变量每变化1单位，被解释变量的平均变化量”，此处即“经验教师授课”与“非经验教师授课”的成绩均值差，但该差异可能包含自选择偏差（如更优秀的学生更易匹配经验教师），并非严格的因果效应。

---

#### Q2 
Discuss Alice’s critique about tracking/selection into experienced teachers: Do you agree that $\hat{\beta}_{1}$ may not be causal due to tracking/selection?

##### 答案
Yes, $\hat{\beta}_{1}$ is likely not a causal effect—tracking/selection leads to omitted-variable bias in OLS estimation.

##### 解析
核心矛盾在于“自选择偏差”（omitted-variable bias）：  
- 存在未观测变量（如学生的勤奋程度、家长教育动机、基线能力），这些变量既会提高学生的考试成绩（$Y_i$），又会增加学生被经验教师授课的概率（$ET_i$）；  
- 这导致OLS回归的误差项$\varepsilon_i$与解释变量$ET_i$不满足“均值独立”假设，即$$\mathbb{E}[\varepsilon_i | ET_i] \neq 0$$；  
- 因此，观测回归的斜率项$\hat{\beta}_{1}=5.0$混淆了“经验教师的真实效应”与“未观测变量的效应”，无法被解读为经验教师对成绩的因果影响。

---
![NHIS](/images/IV/3.png)
#### Q3
##### Q3(a) 
Compute the coefficient on $D_{i}$ from the first-stage regression of $ET_{i}$ on $D_{i}$.

###### 答案
$$\hat{\pi}_1 = 0.75$$

###### 解析
第一阶段回归的核心是估计“工具变量$D_i$对内生变量$ET_i$的影响”，系数计算公式为：  
$$\hat{\pi}_1 = \mathbb{E}[ET_i | D_i=1] - \mathbb{E}[ET_i | D_i=0]$$  
- 从Table 3提取数据：$\mathbb{E}[ET_i | D_i=1] = 1$（所有处理组学生均选择经验教师），$\mathbb{E}[ET_i | D_i=0] = 0.25$（对照组仅25%学生获得经验教师授课）；  
- 代入计算：$\hat{\pi}_1 = 1 - 0.25 = 0.75$，即处理组比对照组的经验教师授课概率高75个百分点。

##### Q3(b) 
Compute the coefficient on $D_{i}$ from the reduced-form regression of $Y_{i}$ on $D_{i}$.

###### 答案
$$\hat{\rho}_1 = 12.5$$

###### 解析
简化式回归的核心是估计“工具变量$D_i$对结果变量$Y_i$的直接影响”（即ITT效应），系数计算公式为：  
$$\hat{\rho}_1 = \mathbb{E}[Y_i | D_i=1] - \mathbb{E}[Y_i | D_i=0]$$  
- 从Table 3提取数据：$\mathbb{E}[Y_i | D_i=1] = 86.25$（处理组观测成绩均值），$\mathbb{E}[Y_i | D_i=0] = 73.75$（对照组观测成绩均值）；  
- 代入计算：$\hat{\rho}_1 = 86.25 - 73.75 = 12.5$，即处理组比对照组的观测成绩平均高12.5分。

##### Q3(c) 
Compute the Wald estimator for the impact of $ET_{i}$ on $Y_{i}$ using Q3(a) and Q3(b).

###### 答案
$$\hat{LATE}_{Wald} \approx 16.67$$

###### 解析
Wald估计量是IV框架中LATE的核心计算方法，逻辑为“简化式效应/第一阶段效应”，公式为：  
$$\hat{LATE}_{Wald} = \frac{\hat{\rho}_1}{\hat{\pi}_1}$$  
- 代入Q3(a)和Q3(b)的结果：$\hat{\rho}_1=12.5$，$\hat{\pi}_1=0.75$；  
- 计算得：$\hat{LATE}_{Wald} = \frac{12.5}{0.75} \approx 16.67$，即通过工具变量$D_i$识别的“依从者”群体中，经验教师授课对成绩的平均效应约为16.67分。

---

#### Q4 
Directly compute $LATE = \mathbb{E}[Y_{1i} - Y_{0i} | \text{Compliers}]$ using potential outcomes.

##### 答案
$$LATE = 17.5$$

##### 解析
依从者的核心特征：$ET_i(D=1)=1$（处理组时选择经验教师）且$ET_i(D=0)=0$（对照组时无法获得经验教师）。  
从Table 3筛选出依从者ID：$\{1,2,4,6,7,8\}$（共6人）。

| 依从者ID | $Y_{1i}$（潜在成绩，$ET=1$） | $Y_{0i}$（潜在成绩，$ET=0$） | 个体因果效应（$Y_{1i}-Y_{0i}$） |
|----------|------------------------------|------------------------------|--------------------------------|
| 1        | 80                           | 70                           | 10                             |
| 2        | 90                           | 70                           | 20                             |
| 4        | 85                           | 65                           | 20                             |
| 6        | 70                           | 50                           | 20                             |
| 7        | 95                           | 80                           | 15                             |
| 8        | 100                          | 80                           | 20                             |

$$LATE = \frac{1}{6} \sum_{i \in \{1,2,4,6,7,8\}} (Y_{1i} - Y_{0i}) = \frac{10 + 20 + 20 + 20 + 15 + 20}{6} = \frac{105}{6} = 17.5$$

---

#### Q5
Explain why the Wald estimator from Q3 and the LATE in Q4 are different.

##### 答案
The discrepancy arises from **finite-sample bias** (small sample imbalance in compliance types).

##### 解析
在LATE框架中，满足IV四大假设（独立性、排他性、单调性、非零第一阶段）时，Wald估计量在**大样本下会收敛到真实LATE**，但本案例中$N=8$（极小样本），导致实际估计偏差：  
1. 样本分配与依从类型失衡：对照组中存在“总是就读者”（Always-taker，如ID=3，$ET_i(D=0)=1$），而处理组无“从不就读者”（Never-taker），打破了样本层面的依从类型分布均衡；  
2. 简化式与第一阶段的样本差异未完全匹配依从者群体：$\mathbb{E}[Y|D=1] - \mathbb{E}[Y|D=0]$（12.5）和$\mathbb{E}[ET|D=1] - \mathbb{E}[ET|D=0]$（0.75）包含了非依从者的干扰（如对照组中总是就读者的成绩），而非纯粹的依从者效应；  
3. 最终导致Wald估计量（16.67）与基于潜在结果的真实LATE（17.5）存在微小差异，这一偏差在大样本中会自然消失。

### Industrial Air Pollution and Childhood Asthma
Childhood asthma is a growing concern. Estimating the causal impact of pollution is challenging because pollution may correlate with other local characteristics. An IV approach uses coal-fired power plant presence as an instrument for local SO2 concentration.

asthma：儿童哮喘住院率（每千人入院数）

so2：SO2浓度

plant：燃煤电厂存在虚拟变量

income：人均收入（USD）

unemp：失业率（%）

![NHIS](/images/IV/4.png)
---

#### Q1 
If IV is consistent for the true causal effect, why could OLS be much smaller in magnitude?

##### 答案
OLS系数存在**负向遗漏变量偏差**（negative omitted-variable bias），导致其绝对值小于真实因果效应。

##### 解析
SO2浓度升高会增加儿童哮喘住院率（真实效应为正）。我们会遗漏关键变量：污染缓解行为（mitigation behavior），包括使用空气净化器、佩戴口罩、减少户外活动、地方防控措施等。

- 污染越严重的地区，居民的缓解行为越强，so2与缓解行为正相关）；
- 缓解行为会降低哮喘住院率（缓解行为与asthma负相关）；
- 这导致OLS回归的误差项u包含缓解行为的影响，且满足$$Cov(so2_{ct}, u_{ct}) < 0$$；
- 负向遗漏变量偏差会“稀释”SO2对哮喘的正向效应，最终使OLS系数（0.062）小于IV估计的真实因果效应（0.185）。

---

#### Q2
State the two key IV assumptions for using \(plant_{ct}\) as an instrument for \(so2_{ct}\). Which can be partially assessed using Table 4?

##### 答案
- 两个关键IV假设：相关性假设（Relevance）、排他性假设（Exclusion Restriction）；
- 可通过Table 4部分验证的假设：相关性假设。

##### 解析
两个核心IV假设的具体内容：
   - 相关性假设：工具变量\(plant_{ct}\)能显著预测内生变量\(so2_{ct}\)（即第一阶段效应非零），数学表达为$$Cov(plant_{ct}, so2_{ct}) \neq 0$$；
   - 排他性假设：工具变量\(plant_{ct}\)仅通过内生变量\(so2_{ct}\)影响结果变量\(asthma_{ct}\)，与哮喘的未观测决定因素无关，数学表达为$$Cov(plant_{ct}, u_{ct}) = 0$$。

Table 4对假设的验证：
   - 相关性假设：Table 4第一阶段结果显示，\(plant_{ct}\)对\(so2_{ct}\)的回归系数为5.240，且在1%水平上显著（\(p<0.01\)），说明电厂存在与\(SO_2\)浓度强相关，直接验证了相关性假设；
   - 排他性假设：无法通过Table 4直接检验，需依赖理论论证（如“电厂存在仅通过污染影响哮喘，与其他因素无关”），数据无法直接证明无其他影响路径。

---


#### Q4 
Why might an “upwind plant” dummy be a better instrument for local \(SO_2\) than within-county plant presence?

##### 答案
“上风向电厂”（UpwindPlant\(_{ct}\)）在**相关性**和**排他性假设合理性**两方面更具优势，能提升IV估计的可信度。

##### 解析
其优势具体如下：
- 县内电厂的污染可能被本地防控措施（如废气处理设备）削弱，对本地SO2浓度的影响不稳定；
- 县内电厂的建设和运营可能与本地经济政策、产业规划高度相关（如为促进就业引入电厂），这些因素可能直接影响儿童哮喘住院率（如经济发达县医疗资源更好），导致plant与误差项相关，违反排他性；
- 上风向电厂（尤其位于其他县的上风向电厂）与目标县的经济政策、本地特征关联度更低，更难通过非污染路径影响哮喘住院率，显著降低了与未观测变量相关的风险，使排他性假设更具说服力。