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
  font-size:1.35em;
  font-weight:700;
  margin-top:2em;
  margin-bottom:0.8em;
  border-left:5px solid #3b82f6;
  padding-left:12px;
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

### 三种模型ROC比较

<img src="/images/roc.png" alt="三种模型ROC比较" style="max-width:70%; height:auto;">

<div style="
  background:#f6f8fa;
  border-left:4px solid #3b82f6;
  padding:12px 16px;
  margin:16px 0;
  border-radius:8px;
">
<strong>结论要点：</strong><br>
三种模型在验证集上的判别能力整体有限，其中 Logistic Regression 表现相对最优（AUC = 0.58），
Random Forest 接近随机分类（AUC ≈ 0.50），提示在当前特征与样本规模下模型预测能力仍有明显提升空间。
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
