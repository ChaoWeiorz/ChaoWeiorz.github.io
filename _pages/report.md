---
title: "Data Report"
permalink: /report/
author_profile: true
---

## 报告目录
- [研究背景](#研究背景)
- [数据与方法](#数据与方法)
- [主要结果](#主要结果)
- [讨论](#讨论)
- [核心代码](#核心代码)

---

## 研究背景

心力衰竭患者出院后的短期再入院不仅反映疾病严重程度，也与医疗资源利用及患者预后密切相关。
28 天内再入院常被视为评估住院管理质量与短期风险的重要结局指标。

本研究基于 PhysioNet 公共数据库中来自中国四川地区的回顾性住院心力衰竭队列，
以“28 天内是否再入院”为唯一结局变量，旨在通过统计分析与机器学习方法，
探索影响短期再入院风险的关键临床因素，并评估不同模型在该情境下的预测能力。

Zhang, Zhongheng, et al. "Hospitalized patients with heart failure: integrating electronic healthcare records and external outcome data" (version 1.3). PhysioNet (2022). RRID:SCR_007345. https://doi.org/10.13026/5m60-vs44

## 数据与方法

**数据来源与研究对象**

研究数据来源于 PhysioNet 公共数据库中
“Hospitalized patients with heart failure”回顾性队列，
该数据集基于中国四川地区真实世界电子病历构建，
纳入 2016–2019 年间共 2,008 例患者，覆盖 168 个临床变量。

本研究以 28 天内是否再入院（re.admission.within.28.days）作为唯一结局变量。
基于研究目标、临床相关性、数据可用性及方法学可控性，
从原始 168 个变量中筛选出 10 个研究变量（1 个因变量、9 个自变量），
构建统一分析框架。

**数据预处理与统计分析**

对分类及等级变量进行重编码，并依据变量分布特征完成缺失值填补。
随后按结局变量进行分层随机抽样，将数据划分为训练集（70%）与测试集（30%）。

统计分析阶段使用未填补缺失值的数据集，
采用描述性统计、成对删除法的组间比较及单因素 Logistic 回归，
探索各变量与 28 天再入院风险之间的关联。

**监督学习与无监督学习**

在监督学习阶段，基于填补后数据集分别构建
Logistic Regression、Random Forest 与 Support Vector Machine 模型，
统一在训练集上建模、在测试集上评估，并通过 ROC 曲线与 AUC 比较模型性能。
对 AUC 表现最优的模型进一步开展混淆矩阵分析及模型可解释性分析。

在无监督学习阶段，对连续变量进行标准化处理，
采用 K-means 聚类进行探索性分析，
并结合肘部法则与轮廓系数确定聚类数，
利用主成分分析（PCA）对聚类结果进行二维可视化展示。

## 主要结果

<h3 style="
  font-size:1.1em;
  font-weight:600;
  margin-top:1.6em;
  margin-bottom:0.6em;
  border-left:4px solid #3b82f6;
  padding-left:10px;
">
变量分布可视化
</h3>

<!-- Row 1 -->
<div style="display:flex; overflow-x:auto; gap:12px; margin-bottom:16px;">
  <img src="/images/s1.png"  style="height:220px;">
  <img src="/images/s2.png"  style="height:220px;">
  <img src="/images/s3.png"  style="height:220px;">
  <img src="/images/s4.png"  style="height:220px;">
  <img src="/images/s5.png"  style="height:220px;">
</div>

<!-- Row 2 -->
<div style="display:flex; overflow-x:auto; gap:12px; margin-bottom:16px;">
  <img src="/images/s6.png"  style="height:220px;">
  <img src="/images/s7.png"  style="height:220px;">
  <img src="/images/s8.png"  style="height:220px;">
  <img src="/images/s9.png"  style="height:220px;">
  <img src="/images/s10.png" style="height:220px;">
</div>

<!-- Row 3 -->
<div style="display:flex; overflow-x:auto; gap:12px;">
  <img src="/images/s11.png" style="height:220px;">
  <img src="/images/s12.png" style="height:220px;">
  <img src="/images/s13.png" style="height:220px;">
  <img src="/images/s14.png" style="height:220px;">
  <img src="/images/s15.png" style="height:220px;">
</div>


<h3 style="
  font-size:1.1em;
  font-weight:600;
  margin-top:1.6em;
  margin-bottom:0.6em;
  border-left:4px solid #3b82f6;
  padding-left:10px;
">
不同预测模型对 28 天再入院的 ROC 曲线比较
</h3>

<img src="/images/roc.png" alt="三种模型ROC比较" style="max-width:70%; height:auto;">

<div style="
  background:#f6f8fa;
  border-left:4px solid #3b82f6;
  padding:12px 16px;
  margin:16px 0;
  border-radius:8px;
">
<strong>要点：</strong><br>
三种模型在验证集上的判别能力整体有限，其中 Logistic Regression 表现相对最优（AUC = 0.58），
Random Forest 接近随机分类（AUC ≈ 0.50），提示在当前特征与样本规模下模型预测能力仍有明显提升空间。
</div>


<h3 style="
  font-size:1.1em;
  font-weight:600;
  margin-top:1.6em;
  margin-bottom:0.6em;
  border-left:4px solid #3b82f6;
  padding-left:10px;
">
Logistic Regression 模型在测试集上的混淆矩阵
</h3>

<img src="/images/confusion.png" alt="Logistic Regression 混淆矩阵" style="max-width:60%; height:auto;">

<div style="
  background:#f6f8fa;
  border-left:4px solid #3b82f6;
  padding:12px 16px;
  margin:16px 0;
  border-radius:8px;
">
<strong>要点：</strong><br>
在以 0.5 作为分类阈值的情况下，Logistic Regression 模型在测试集中对未发生 28 天再入院的识别能力较强，
但对再入院事件的识别能力明显不足，表现为特异度较高而灵敏度较低。
该结果与样本中再入院事件比例较低的类别不平衡特征一致，
提示在当前变量体系下模型更倾向于将患者预测为低风险人群，
对短期再入院高风险患者的识别能力仍然有限。
</div>


<h3 style="
  font-size:1.1em;
  font-weight:600;
  margin-top:1.6em;
  margin-bottom:0.6em;
  border-left:4px solid #3b82f6;
  padding-left:10px;
">
Logistic Regression 模型回归系数、比值比及其 95% 置信区间
</h3>

<table style="border-collapse:collapse; margin:12px 0; width:100%;">
  <thead>
    <tr style="border-bottom:2px solid #e5e7eb;">
      <th style="text-align:left; padding:6px;">Variable</th>
      <th style="text-align:center; padding:6px;">Coefficient (β)</th>
      <th style="text-align:center; padding:6px;">OR</th>
      <th style="text-align:center; padding:6px;">95% CI lower</th>
      <th style="text-align:center; padding:6px;">95% CI upper</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>ageCat</td><td align="center">0.17</td><td align="center">1.18</td><td align="center">0.96</td><td align="center">1.46</td></tr>
    <tr><td>gender</td><td align="center">-0.19</td><td align="center">0.83</td><td align="center">0.54</td><td align="center">1.28</td></tr>
    <tr><td>systolic.blood.pressure</td><td align="center">-0.28</td><td align="center">0.75</td><td align="center">0.60</td><td align="center">0.94</td></tr>
    <tr><td>pulse</td><td align="center">0.07</td><td align="center">1.07</td><td align="center">0.87</td><td align="center">1.32</td></tr>
    <tr><td>NYHA.cardiac.function.classification</td><td align="center">0.29</td><td align="center">1.34</td><td align="center">0.97</td><td align="center">1.84</td></tr>
    <tr><td>brain.natriuretic.peptide</td><td align="center">-0.02</td><td align="center">0.98</td><td align="center">0.78</td><td align="center">1.22</td></tr>
    <tr><td>creatinine.enzymatic.method</td><td align="center">0.18</td><td align="center">1.20</td><td align="center">0.97</td><td align="center">1.48</td></tr>
    <tr><td>albumin</td><td align="center">0.10</td><td align="center">1.10</td><td align="center">0.88</td><td align="center">1.38</td></tr>
    <tr><td>CCI.score</td><td align="center">0.15</td><td align="center">1.17</td><td align="center">0.94</td><td align="center">1.45</td></tr>
  </tbody>
</table>

<div style="
  background:#f6f8fa;
  border-left:4px solid #3b82f6;
  padding:12px 16px;
  margin:16px 0;
  border-radius:8px;
">
<strong>要点：</strong><br>
多变量 Logistic Regression 分析显示，
在纳入 9 个自变量后，仅收缩压与 28 天再入院风险保持统计学显著关联
（OR = 0.75，95% CI：0.60–0.94），
表现为收缩压越低，再入院风险越高。
其余变量的 95% 置信区间均跨越 1，未达到统计学显著性。
其中，年龄分组、脉搏、NYHA 心功能分级、肌酐、白蛋白及 CCI 评分呈正向趋势，
而性别及 B 型利钠肽呈负向趋势，
上述方向性仅作趋势性描述，不作统计学结论。
</div>


<h3 style="
  font-size:1.1em;
  font-weight:600;
  margin-top:1.6em;
  margin-bottom:0.6em;
  border-left:4px solid #3b82f6;
  padding-left:10px;
">
Logistic Regression 模型的 SHAP 汇总图（特征重要性与方向性）
</h3>

<img src="/images/shapsum.png" alt="Logistic Regression SHAP 汇总图" style="max-width:70%; height:auto;">

<div style="
  background:#f6f8fa;
  border-left:4px solid #3b82f6;
  padding:12px 16px;
  margin:16px 0;
  border-radius:8px;
">
<strong>要点：</strong><br>
SHAP 汇总图显示，模型预测主要由连续临床指标驱动，
其中 B 型利钠肽（BNP）、肌酐及收缩压对模型输出的影响最为显著。
较高的肌酐水平与较低的收缩压均推动模型更倾向预测为 28 天再入院，
而 BNP 在本模型中呈现出与传统长期预后认知不同的方向性：
较高 BNP 水平反而降低模型预测为短期再入院的概率。
这一现象可能与研究结局聚焦于短期再入院、
以及高 BNP 患者在临床管理中接受更充分干预有关，
提示 SHAP 结果应被理解为特定研究情境下的模型决策特征，
而非对 BNP 临床意义的重新定义。
</div>


<h3 style="
  font-size:1.1em;
  font-weight:600;
  margin-top:1.6em;
  margin-bottom:0.6em;
  border-left:4px solid #3b82f6;
  padding-left:10px;
">
主要连续变量的 SHAP 依赖图（Logistic Regression 模型）
</h3>

<div style="display:flex; overflow-x:auto; gap:16px; margin:12px 0;">
  <img src="/images/shapdep1.png" alt="BNP 的 SHAP 依赖图" style="height:260px;">
  <img src="/images/shapdep2.png" alt="肌酐的 SHAP 依赖图" style="height:260px;">
  <img src="/images/shapdep3.png" alt="收缩压的 SHAP 依赖图" style="height:260px;">
</div>

<div style="
  background:#f6f8fa;
  border-left:4px solid #3b82f6;
  padding:12px 16px;
  margin:16px 0;
  border-radius:8px;
">
<strong>要点：</strong><br>
SHAP 依赖图显示模型主要通过连续变量的单调关系进行决策。
BNP 与 SHAP 值呈近似线性负相关，较高 BNP 水平推动模型预测为未再入院；
肌酐与 SHAP 值呈稳定正相关，提示肾功能受损程度越高，再入院风险越大；
收缩压则呈清晰的负相关关系，较低收缩压推动模型更倾向预测为再入院。
上述关系在不同协变量分层下整体一致，
反映了连续临床指标在短期再入院预测中的稳定方向性作用模式。
</div>

<h3 style="
  font-size:1.1em;
  font-weight:600;
  margin-top:1.6em;
  margin-bottom:0.6em;
  border-left:4px solid #3b82f6;
  padding-left:10px;
">
K-means 聚类的肘部法则图
</h3>

<img src="/images/k1.png" alt="K-means 聚类的肘部法则图" style="max-width:60%; height:auto;">

<div style="
  background:#f6f8fa;
  border-left:4px solid #3b82f6;
  padding:12px 16px;
  margin:16px 0;
  border-radius:8px;
">
<strong>要点：</strong><br>
肘部法则图显示，随着聚类数 K 的增加，类内平方和（WCSS）持续下降但降幅逐渐减小。
当 K 从 2 增加至 4 时，WCSS 下降较为明显；而当 K 大于 4 后，曲线趋于平缓，
提示进一步增加聚类数对类内紧密度的改善有限。
结合轮廓系数结果及聚类结果的临床可解释性，
本研究最终选择 K=4 作为 K-means 聚类的聚类数，
用于后续聚类分析与可视化展示。
</div>


<h3 style="
  font-size:1.1em;
  font-weight:600;
  margin-top:1.6em;
  margin-bottom:0.6em;
  border-left:4px solid #3b82f6;
  padding-left:10px;
">
K-means 聚类结果的 PCA 二维可视化（K = 4）
</h3>

<img src="/images/k2.png" alt="K-means 聚类结果的 PCA 二维可视化" style="max-width:65%; height:auto;">

<div style="
  background:#f6f8fa;
  border-left:4px solid #3b82f6;
  padding:12px 16px;
  margin:16px 0;
  border-radius:8px;
">
<strong>要点：</strong><br>
PCA 二维可视化显示，基于 K-means 聚类（K=4）的结果在二维主成分空间中形成了相对清晰的分布结构，
尽管部分区域存在重叠，但整体仍呈现一定程度的分离趋势。
部分聚类主要沿第一主成分（PC1）方向分布，
而另一些聚类更多体现在第二主成分（PC2）方向上的区分，
反映了研究人群在连续临床指标上的潜在异质性。
需要指出的是，PCA 可视化仅用于辅助理解聚类结构，
其二维投影无法完全反映高维空间中的真实距离关系，
但仍为后续对不同聚类亚群进行描述性比较提供了直观依据。
</div>




## 讨论

本研究结果显示，在纳入的多项临床变量中，
收缩压在多变量分析中仍与 28 天再入院风险保持稳定关联，
提示血流动力学状态可能在短期再入院风险中具有相对核心作用。
相比之下，部分在单因素分析中具有统计学意义的变量
在多变量模型中未能保持显著性，
反映出短期再入院风险可能由多因素共同作用，
且变量之间存在一定相关性。

监督学习模型整体区分能力有限，
即便表现最优的 Logistic Regression，
其 AUC 仍接近随机水平，且灵敏度较低。
这一结果提示，
仅依赖常规住院期可获得的临床指标，
对短期再入院这一结局进行精准预测具有较大挑战，
同时也反映出研究样本中再入院事件比例较低所带来的类别不平衡问题。

模型可解释性分析显示，
模型决策主要依赖连续临床指标，
其中肌酐与收缩压呈现出稳定的风险方向。
值得注意的是，B 型利钠肽在模型中呈现出
不同于传统长期预后评估的方向性，
这一现象可能与本研究关注的是短期再入院而非死亡或远期结局有关，
亦可能反映出临床管理策略在短期内对高风险患者的干预作用。
因此，该结果更应被理解为特定研究情境下的模型决策特征，
而非对其临床意义的重新定义。

此外，无监督学习结果提示，
在不引入结局变量的前提下，
基于连续临床指标仍可识别具有不同特征模式的潜在亚群，
为进一步探索心力衰竭患者的临床异质性提供了探索性线索。

## 核心代码
（这里先空着）

---

## 完整 PDF
- [打开 / 下载 PDF](/files/report.pdf)

<iframe
  src="/files/report.pdf"
  width="100%"
  height="900"
  style="border:1px solid #ddd; border-radius:12px;">
</iframe>
