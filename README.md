# Automatic White Balance (AWB) Learning Roadmap

> 自动白平衡学习路线：从统计先验、物理模型与传统机器学习，到深度学习、跨相机泛化、多光源建模和感知偏好。

自动白平衡（Automatic White Balance，AWB）是相机图像信号处理（ISP）中的关键环节。它需要从图像中估计场景光源，并校正光照造成的颜色偏移，使物体在不同光照下仍呈现稳定、自然的颜色。

这份 Roadmap 不追求覆盖所有 AWB 工作，而是围绕两条主线组织代表性内容：

- **算法如何演进**：从简单统计规律，发展到物理/概率建模、学习式方法和系统级 AWB。
- **问题如何扩展**：从单相机、单光源 RAW 图像，扩展到跨相机、多光源、sRGB 后处理、视频和真实设备部署。

## Roadmap

![AWB 算法发展历程与数据集发展](assets/development.png)

| 阶段 | 年代 | 代表方法 / 工作 | 重点 | 优先级 |
| --- | --- | --- | --- | :---: |
| 启发式先验 | 1970s–1990s | Retinex、White Patch / Max-RGB、Gray World | 从像素强度或通道统计中直接估计光源，建立 AWB 的基本问题形式 | ⭐⭐⭐ |
| 物理与概率模型 | 1990s–2005 | Gamut Mapping、Bayesian Color Constancy、Color by Correlation | 从规则方法走向成像约束、颜色分布与概率推断 | ⭐⭐ |
| 统一统计框架 | 2004–2014 | [Shades of Gray](https://doi.org/10.1117/1.1924555)、Gray Edge、Natural Image Statistics | 用 Minkowski 范数、图像导数和自然图像统计统一多类传统方法 | ⭐⭐⭐ |
| 传统学习式 AWB | 2007–2015 | 手工特征、[Regression Trees](https://openaccess.thecvf.com/content_cvpr_2015/html/Cheng_Effective_Learning-Based_Illuminant_2015_CVPR_paper.html)、Algorithm Selection | 从固定先验转向由数据选择特征、回归器或算法组合 | ⭐⭐ |
| 深度学习与结构化学习 | 2015–2020 | [CNN Color Constancy](https://openaccess.thecvf.com/content_cvpr_workshops_2015/W03/html/Bianco_Color_Constancy_Using_2015_CVPR_paper.html)、[CCC](https://openaccess.thecvf.com/content_iccv_2015/html/Barron_Convolutional_Color_Constancy_ICCV_2015_paper.html)、[FC4](https://openaccess.thecvf.com/content_cvpr_2017/html/Hu_FC4_Fully_Convolutional_CVPR_2017_paper.html)、[FFCC](https://openaccess.thecvf.com/content_cvpr_2017/html/Barron_Fast_Fourier_Color_CVPR_2017_paper.html) | 从手工特征转向端到端特征学习、置信度聚合与高效推理 | ⭐⭐⭐ |
| 真实场景扩展 | 2019–2023 | [sRGB White-Balance Editing](https://openaccess.thecvf.com/content_CVPR_2019/html/Afifi_When_Color_Constancy_Goes_Wrong_Correcting_Improperly_White-Balanced_Images_CVPR_2019_paper.html)、[Cross-Camera C5](https://openaccess.thecvf.com/content/ICCV2021/html/Afifi_Cross-Camera_Convolutional_Color_Constancy_ICCV_2021_paper.html)、Multi-Illuminant / Dense WB | 从单光源 RAW 估计扩展到相机渲染图像、未知传感器和空间变化光照 | ⭐⭐⭐ |
| 系统化与感知偏好 | 2023–至今 | Robustness、Camera-agnostic、Preference-aware WB、Hybrid AWB | 将物理中性、跨域稳定性、实时部署和人眼审美纳入统一系统 | ⭐⭐⭐ |

`⭐⭐⭐` 表示建议重点理解的主线内容，`⭐⭐` 表示重要扩展。

## Recommended Learning Order

如果第一次系统学习 AWB，建议先理解问题假设，再逐步放宽真实场景约束：

```text
成像模型与颜色恒常性
        ↓
Gray World / White Patch
        ↓
Gamut Mapping / Bayesian Methods
        ↓
Shades of Gray / Gray Edge
        ↓
Handcrafted Features / Regression Trees
        ↓
CNN / CCC / FC4 / FFCC
        ↓
sRGB WB Editing
        ↓
Cross-Camera / Camera-Agnostic AWB
        ↓
Multi-Illuminant / Temporal AWB
        ↓
Robust、Efficient、Preference-aware Hybrid AWB
```

学习时可以持续追问三个问题：

1. **估计什么？** 全局光源颜色、局部光照图，还是最终白平衡结果？
2. **依赖什么？** 统计先验、成像物理、相机光谱响应、监督数据，还是场景语义？
3. **在哪里运行？** RAW 域、线性 RGB、相机渲染后的 sRGB，还是实时 ISP / 视频管线？

## How the Main Ideas Evolved

| 研究问题 | 主要演进 |
| --- | --- |
| 如何估计光源？ | 简单通道统计 → 色域与概率模型 → 数据驱动回归 → 端到端特征学习 |
| 使用什么输入？ | 单张 RAW → 线性 RGB → 已渲染 sRGB → 视频与上下文信息 |
| 如何处理空间变化？ | 单一全局光源 → 局部块估计 → 稠密光照图 → 多光源分解与融合 |
| 如何适配不同设备？ | 单相机标定 → 多相机训练 → 跨相机适配 → Camera-agnostic AWB |
| 如何兼顾稳定性？ | 单帧精度 → 置信度建模 → 时序平滑 → 鲁棒系统级决策 |
| 什么是“正确”的白平衡？ | 物理中性 → 感知自然 → 场景语义 → 用户与品牌风格偏好 |
| 如何进入真实 ISP？ | 离线算法 → 高效网络 → 轻量化 / 低功耗 → 物理先验与 AI 混合方案 |

## Chronological Paper List

按发表年份组织的论文阅读清单。阅读优先级：**S+** 核心精读，**S** 主线必读，**A** 推荐阅读，**B** 扩展阅读；组合等级沿用原清单。

### 理论基础与物理建模：1971–1994

| 年份 | Paper | 标签 | 优先级 | 为什么读 |
|---|---|---|---|---|
| 1971 | [Land & McCann — **Lightness and Retinex Theory**](https://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Land71.pdf) | `[VISION] [PHYSICS]` | **S** | Retinex 经典理论；理解 illumination 与 reflectance 分离的视觉思想。 |
| 1980 | [Buchsbaum — **A Spatial Processor Model for Object Colour Perception**](https://doi.org/10.1016/0016-0032(80)90058-7) | `[STAT] [VISION]` | **S** | Gray-World 思想的重要经典来源。 |
| 1985 | [Shafer — **Using Color to Separate Reflection Components**](https://doi.org/10.1002/col.5080100409) | `[PHYSICS]` | **A** | Dichromatic Reflection Model 的经典基础。 |
| 1986 | [Lee — **Method for Computing the Scene-Illuminant Chromaticity from Specular Highlights**](https://doi.org/10.1364/JOSAA.3.001694) | `[PHYSICS]` | **A** | 利用镜面高光推断 illuminant。 |
| 1986 | [Maloney & Wandell — **Color Constancy: A Method for Recovering Surface Spectral Reflectance**](https://doi.org/10.1364/JOSAA.3.000029) | `[PHYSICS] [SPECTRAL]` | **S** | 从 spectral image formation 理解 reflectance / illuminant 可恢复性。 |
| 1989 | [Tominaga & Wandell — **Standard Surface-Reflectance Model and Illuminant Estimation**](https://doi.org/10.1364/JOSAA.6.000576) | `[PHYSICS] [SPECTRAL]` | **A** | 表面反射模型与 illuminant spectral estimation。 |
| 1990 | [Forsyth — **A Novel Algorithm for Color Constancy**](https://doi.org/10.1007/BF00056770) | `[GAMUT]` | **S+** | Gamut Mapping 路线奠基论文。 |
| 1994 | [Finlayson, Drew & Funt — **Color Constancy: Generalized Diagonal Transforms Suffice**](https://doi.org/10.1364/JOSAA.11.003011) | `[PHYSICS] [SPECTRAL]` | **S+** | 在特定低维光谱假设下，通过传感器空间换基实现对角校正；理解独立通道增益模型的成立条件。 |
| 1994 | [Finlayson, Drew & Funt — **Spectral Sharpening: Sensor Transformations for Improved Color Constancy**](https://opg.optica.org/josaa/abstract.cfm?uri=josaa-11-5-1553) | `[PHYSICS] [SPECTRAL]` | **S** | 介绍传感器响应的光谱锐化变换，使独立通道缩放更好地描述光照变化；与广义对角模型的理论结果互补。 |

### Gamut、概率模型与学习思想萌芽：1996–2002

| 年份 | Paper | 标签 | 优先级 | 为什么读 |
|---|---|---|---|---|
| 1996 | [Finlayson — **Color in Perspective**](https://doi.org/10.1109/34.541413) | `[GAMUT]` | **S** | 对 Forsyth gamut mapping 的重要发展与简化。 |
| 1996 | [Funt, Cardei & Barnard — **Learning Color Constancy**](https://library.imaging.org/cic/articles/4/1/art00016) | `[ML] [NN]` | **S+** | 非常早期的 neural-network CC；learning-based CC 并非 2015 才开始。 |
| 1997 | [Brainard & Freeman — **Bayesian Color Constancy**](https://doi.org/10.1364/JOSAA.14.001393) | `[BAYES] [PHYSICS]` | **S+** | 用 illuminant / reflectance prior 与 Bayesian decision theory 建模 CC。 |
| 1999 | [Finlayson et al. — **Selection for Gamut Mapping Colour Constancy**](https://doi.org/10.1016/S0262-8856(98)00179-6) | `[GAMUT]` | **A** | 研究 gamut feasible solutions 的选择问题。 |
| 2001 | [Finlayson, Hordley & Hubel — **Color by Correlation: A Simple, Unifying Framework for Color Constancy**](https://doi.org/10.1109/34.969113) | `[BAYES] [NIS] [ML]` | **S+** | 候选 illuminant likelihood；现代 histogram / classification 思想的重要祖先。 |
| 2002 | [Barnard, Cardei & Funt — **A Comparison of Computational Color Constancy Algorithms—Part I**](https://doi.org/10.1109/TIP.2002.802531) | `[SURVEY] [BENCHMARK]` | **S** | 在 synthetic 条件下系统比较经典算法。 |
| 2002 | [Barnard et al. — **A Comparison of Computational Color Constancy Algorithms—Part II**](https://doi.org/10.1109/TIP.2002.802529) | `[SURVEY] [BENCHMARK]` | **S** | 真实图像实验；适合理解 theory-to-real gap。 |

### 经典统计 CC 的成熟期：2004–2007

| 年份 | Paper | 标签 | 优先级 | 为什么读 |
|---|---|---|---|---|
| 2004 | [Finlayson & Trezzi — **Shades of Gray and Colour Constancy**](https://library.imaging.org/cic/articles/12/1/art00008) | `[STAT]` | **S+** | 用 Minkowski norm 统一 Gray-World 与 Max-RGB。 |
| 2004 | [Tan, Nishino & Ikeuchi — **Color Constancy through Inverse-Intensity Chromaticity Space**](https://doi.org/10.1364/JOSAA.21.000321) | `[PHYSICS]` | **A** | 利用 dichromatic / highlight physics 建立 inverse-intensity chromaticity space。 |
| 2005 | [Schaefer, Hordley & Finlayson — **A Combined Physical and Statistical Approach to Colour Constancy**](https://doi.org/10.1109/CVPR.2005.20) | `[PHYSICS] [STAT]` | **A** | physics + statistics 融合的早期代表。 |
| 2007 | [van de Weijer, Gevers & Gijsenij — **Edge-Based Color Constancy**](https://doi.org/10.1109/TIP.2007.901808) | `[STAT] [GRAY-EDGE]` | **S+** | Gray-Edge；统一 GW / Max-RGB / Minkowski / derivative。 |
| 2007 | [van de Weijer, Schmid & Verbeek — **Using High-Level Visual Information for Color Constancy**](https://doi.org/10.1109/ICCV.2007.4409109) | `[SEMANTIC]` | **S/A** | 很早的 top-down / semantic illuminant estimation。 |
| 2007 | [Gijsenij & Gevers — **Color Constancy Using Natural Image Statistics**](https://doi.org/10.1109/CVPR.2007.383206) | `[NIS] [ML]` | **A** | 动态选择 / 组合 CC 方法，是后续 expert-selection 思想的重要前置。 |

### Benchmark、自然图像统计与场景先验：2008–2014

| 年份 | Paper | 标签 | 优先级 | 为什么读 |
|---|---|---|---|---|
| 2008 | [Gehler et al. — **Bayesian Color Constancy Revisited**](https://www.microsoft.com/en-us/research/publication/bayesian-color-constancy-revisited/) | `[BAYES] [DATASET]` | **S+** | 引出后来极具影响力的 Gehler / ColorChecker 数据集。 |
| 2008 | [Chakrabarti, Hirakawa & Zickler — **Color Constancy Beyond Bags of Pixels**](https://doi.org/10.1109/CVPR.2008.4587664) | `[NIS] [SPATIAL]` | **A** | 显式建模像素空间依赖。 |
| 2009 | [Lu et al. — **Color Constancy Using 3D Scene Geometry**](https://doi.org/10.1109/ICCV.2009.5459391) | `[PHYSICS] [GEOMETRY] [SEMANTIC]` | **B/A** | 利用场景几何选择不同 CC strategy。 |
| 2010 | [Gijsenij, Gevers & van de Weijer — **Generalized Gamut Mapping Using Image Derivative Structures for Color Constancy**](https://doi.org/10.1007/s11263-008-0171-3) | `[GAMUT] [STAT]` | **S/A** | 将 derivative / n-jet structure 引入 gamut mapping。 |
| 2011 | [Gijsenij & Gevers — **Color Constancy Using Natural Image Statistics and Scene Semantics**](https://doi.org/10.1109/TPAMI.2010.93) | `[NIS] [SEMANTIC] [ML]` | **S** | 动态选择 / 组合 CC 算法；现代 conditional-expert 思路的重要历史前身。 |
| 2011 | [Gijsenij, Gevers & van de Weijer — **Computational Color Constancy: Survey and Experiments**](https://doi.org/10.1109/TIP.2011.2118224) | `[SURVEY]` | **S+** | 传统 CC 时代最重要综述之一。 |
| 2012 | [Chakrabarti, Hirakawa & Zickler — **Color Constancy with Spatio-Spectral Statistics**](https://doi.org/10.1109/TPAMI.2011.252) | `[NIS] [BAYES]` | **S** | spatial + spectral probability model 与 maximum-likelihood illuminant estimation。 |
| 2012 | [Vazquez-Corral et al. — **Color Constancy by Category Correlation**](https://doi.org/10.1109/TIP.2011.2171353) | `[NIS] [SEMANTIC]` | **A** | 利用 color-category prior。 |
| 2012 | [Gijsenij, Lu & Gevers — **Color Constancy for Multiple Light Sources**](https://staff.fnwi.uva.nl/th.gevers/pub/GeversTIP12-2.pdf) | `[MULTI-ILLUM] [STAT]` | **S** | 通过局部光源估计、估计结果融合与空间变化校正，将传统颜色恒常方法扩展到多光源场景。 |
| 2013 | [Finlayson — **Corrected-Moment Illuminant Estimation**](https://openaccess.thecvf.com/content_iccv_2013/html/Finlayson_Corrected-Moment_Illuminant_Estimation_2013_ICCV_paper.html) | `[STAT] [ML]` | **S** | 对颜色或导数的统计矩学习线性校正，连接 Gray World 等统计先验与学习式光源估计。 |
| 2014 | [Cheng, Prasad & Brown — **Illuminant Estimation for Color Constancy: Why Spatial-Domain Methods Work and the Role of the Color Distribution**](https://doi.org/10.1364/JOSAA.31.001049) | `[STAT] [NIS] [DATASET]` | **S+** | 重新解释 spatial CC，并引出 NUS 8-camera benchmark。 |
| 2014 | [Joze & Drew — **Exemplar-Based Colour Constancy and Multiple Illumination**](https://www.cs.sfu.ca/~mark/ftp/Pami2014/pami2014.pdf) | `[ML] [MULTI-ILLUM]` | **A** | exemplar learning 与 multiple illumination 的重要早期工作。 |
| 2014 | [**Reproduction Angular Error: An Improved Performance Metric for Illuminant Estimation**](https://www.bmva-archive.org.uk/bmvc/2014/papers/paper047/index.html) | `[BENCHMARK]` | **S+** | 区分 recovery 与 reproduction angular error；从光源估计方向误差进一步理解校正后白色的残余偏色。 |

### Learning-Based → Deep Learning 与跨相机评测：2015–2018

| 年份 | Paper | 标签 | 优先级 | 为什么读 |
|---|---|---|---|---|
| 2015 | [Cheng et al. — **Effective Learning-Based Illuminant Estimation Using Simple Features**](https://openaccess.thecvf.com/content_cvpr_2015/html/Cheng_Effective_Learning-Based_Illuminant_2015_CVPR_paper.html) | `[ML]` | **S** | 简单颜色特征 + regression trees；说明“深”并不是 learning CC 的唯一方向。 |
| 2015 | [Bianco, Cusano & Schettini — **Color Constancy Using CNNs**](https://openaccess.thecvf.com/content_cvpr_workshops_2015/W03/html/Bianco_Color_Constancy_Using_2015_CVPR_paper.html) | `[CNN]` | **S** | 经典 patch-based CNN illuminant regression。 |
| 2015 | [Barron — **Convolutional Color Constancy**](https://openaccess.thecvf.com/content_iccv_2015/html/Barron_Convolutional_Color_Constancy_ICCV_2015_paper.html) | `[ML] [LOG-CHROMA]` | **S+** | 把 CC 写成 log-chrominance 空间的 2D localization；FFCC 的直接基础。 |
| 2015 | [Chakrabarti — **Color Constancy by Learning to Predict Chromaticity from Luminance**](https://arxiv.org/abs/1506.02167) | `[ML] [NIS]` | **S/A** | 证明 pixel-level color statistics 本身也可形成很强的 learned likelihood。 |
| 2015 | [Yang, Gao & Li — **Efficient Illuminant Estimation for Color Constancy Using Grey Pixels**](https://openaccess.thecvf.com/content_cvpr_2015/html/Yang_Efficient_Illuminant_Estimation_2015_CVPR_paper.html) | `[STAT] [PHYSICS] [EFFICIENCY]` | **S** | 从偏色图像中检测近似灰像素并估计光源；连接 Gray World / Gray Edge 与后续 Grayness Index 方法。 |
| 2015 | [Cheng et al. — **Beyond White: Ground Truth Colors for Color Constancy Correction**](https://openaccess.thecvf.com/content_iccv_2015/html/Cheng_Beyond_White_Ground_ICCV_2015_paper.html) | `[PHYSICS] [DATASET] [BENCHMARK]` | **S** | 将评价对象从中性色扩展到彩色色块，并研究完整颜色校正；理解白平衡准确与整体颜色准确之间的关系。 |
| 2016 | [Shi, Loy & Tang — **Deep Specialized Network for Illuminant Estimation**](https://mmlab.ie.cuhk.edu.hk/projects/illuminant_estimation.html) | `[CNN] [AMBIGUITY]` | **S** | HypNet + SelNet；显式处理 local patch illuminant ambiguity。 |
| 2017 | [Oh & Kim — **Approaching the Computational Color Constancy as a Classification Problem through Deep Learning**](https://doi.org/10.1016/j.patcog.2016.08.013) | `[CNN] [CLASSIFICATION]` | **A** | 从直接 regression 转向 illumination classification。 |
| 2017 | [Hu, Wang & Lin — **FC4: Fully Convolutional Color Constancy with Confidence-Weighted Pooling**](https://openaccess.thecvf.com/content_cvpr_2017/html/Hu_FC4_Fully_Convolutional_CVPR_2017_paper.html) | `[CNN] [CONFIDENCE]` | **S+** | local illuminant + confidence weighted pooling；学习“哪些区域值得相信”。 |
| 2017 | [Barron & Tsai — **Fast Fourier Color Constancy**](https://openaccess.thecvf.com/content_cvpr_2017/html/Barron_Fast_Fourier_Color_CVPR_2017_paper.html) | `[LOG-CHROMA] [BAYES] [EFFICIENCY]` | **S+** | CCC → Fourier domain；完整 posterior、速度、temporal smoothing、移动端 AWB。 |
| 2017 | [Qian et al. — **Recurrent Color Constancy**](https://openaccess.thecvf.com/content_iccv_2017/html/Qian_Recurrent_Color_Constancy_ICCV_2017_paper.html) | `[CNN] [TEMPORAL]` | **S/A** | ConvLSTM 将多帧时序信息引入 CC。 |
| 2018 | [Aytekin, Nikkanen & Gabbouj — **A Dataset for Camera Independent Color Constancy**](https://researchportal.tuni.fi/en/publications/a-dataset-for-camera-independent-color-constancy/) | `[DATASET] [CROSS-CAMERA] [SPECTRAL]` | **S** | 提供不同相机拍摄的对应场景、相机光谱响应与实验室光源光谱，支持跨相机评测及颜色渐晕影响分析。 |

### 从 Benchmark Accuracy 转向真实问题：2019–2020

| 年份 | Paper | 标签 | 优先级 | 为什么读 |
|---|---|---|---|---|
| 2019 | [Bianco & Cusano — **Quasi-Unsupervised Color Constancy**](https://openaccess.thecvf.com/content_CVPR_2019/html/Bianco_Quasi-Unsupervised_Color_Constancy_CVPR_2019_paper.html) | `[CNN] [SELF-SUP]` | **S/A** | 不使用 illuminant GT 的弱监督 / quasi-unsupervised 思路。 |
| 2019 | [Qian et al. — **On Finding Gray Pixels**](https://openaccess.thecvf.com/content_CVPR_2019/html/Qian_On_Finding_Gray_Pixels_CVPR_2019_paper.html) | `[PHYSICS] [STAT] [MULTI-ILLUM]` | **S** | Grayness Index；证明深度学习时代 physics-based 方法仍极具竞争力。 |
| 2019 | [Afifi et al. — **When Color Constancy Goes Wrong: Correcting Improperly White-Balanced Images**](https://openaccess.thecvf.com/content_CVPR_2019/html/Afifi_When_Color_Constancy_Goes_Wrong_Correcting_Improperly_White-Balanced_Images_CVPR_2019_paper.html) | `[sRGB-WB]` | **S+** | 将研究从 RAW illuminant estimation 扩展到 ISP 后错误 sRGB WB correction。 |
| 2019 | [Banić et al. — **The Past and the Present of the Color Checker Dataset Misuse**](https://arxiv.org/abs/1903.04473) | `[DATASET] [BENCHMARK]` | **S** | 梳理 ColorChecker 数据集的黑电平处理与数据误用，理解实验结果的可比性；链接为 arXiv 版本。 |
| 2019 | [McDonagh et al. — **Formulating Camera-Adaptive Color Constancy as a Few-shot Meta-Learning Problem**](https://smcdonagh.github.io/publication/meta_awb/) | `[CROSS-CAMERA] [FEW-SHOT] [ML]` | **A** | ICCV Workshop 2019；用元学习实现少样本新相机适配，可在 MDLCC / C5 前阅读；需要目标相机的少量训练样本。 |
| 2019 | [Yoo & Kim — **Dichromatic Model Based Temporal Color Constancy for AC Light Sources**](https://openaccess.thecvf.com/content_CVPR_2019/html/Yoo_Dichromatic_Model_Based_Temporal_Color_Constancy_for_AC_Light_Sources_CVPR_2019_paper.html) | `[TEMPORAL] [PHYSICS]` | **A** | 利用高速摄影捕捉交流光源的时间变化，通过二色反射模型估计光源；补充具有特定光源与采集条件的时序物理方法。 |
| 2020 | [Yu et al. — **Cascading Convolutional Color Constancy**](https://doi.org/10.1609/aaai.v34i07.6966) | `[CNN] [AMBIGUITY] [CROSS-CAMERA]` | **A** | Cascaded hypotheses / coarse-to-fine learning，并关注跨 dataset / camera 稳定性。 |
| 2020 | [Hernandez-Juarez et al. — **A Multi-Hypothesis Approach to Color Constancy**](https://openaccess.thecvf.com/content_CVPR_2020/html/Hernandez-Juarez_A_Multi-Hypothesis_Approach_to_Color_Constancy_CVPR_2020_paper.html) | `[BAYES] [CNN] [AMBIGUITY] [CROSS-CAMERA]` | **S** | 多候选 illuminant + posterior；适合理解 CC 的多解性。 |
| 2020 | [Xiao, Gu & Zhang — **Multi-Domain Learning for Accurate and Few-Shot Color Constancy**](https://openaccess.thecvf.com/content_CVPR_2020/html/Xiao_Multi-Domain_Learning_for_Accurate_and_Few-Shot_Color_Constancy_CVPR_2020_paper.html) | `[CROSS-CAMERA] [FEW-SHOT] [CNN]` | **S+** | 把不同 camera 建模为不同 domain；现代 cross-camera CC 的关键工作。 |
| 2020 | [Afifi & Brown — **Deep White-Balance Editing**](https://openaccess.thecvf.com/content_CVPR_2020/html/Afifi_Deep_White-Balance_Editing_CVPR_2020_paper.html) | `[sRGB-WB] [CNN]` | **S** | End-to-end sRGB white-balance editing。 |
| 2020 | [Koskinen, Yang & Kämäräinen — **Cross-dataset Color Constancy Revisited Using Sensor-to-Sensor Transfer**](https://www.bmvc2020-conference.com/assets/papers/0082.pdf) | `[CROSS-CAMERA] [PHYSICS] [BENCHMARK]` | **S** | 利用已知相机的光谱特性进行传感器间数据迁移，构建跨数据集评测；适合与 C5 / DMCC / CCMNet 对照适配条件。 |
| 2020 | [Laakom et al. — **Bag of Color Features for Color Constancy**](https://trepo.tuni.fi/handle/10024/217447) | `[CNN] [STAT] [ATTENTION] [EFFICIENCY]` | **A** | BoCF 用特征袋池化构建紧凑模型，连接传统颜色统计、学习式表示与注意力，补充轻量光源估计路线。 |

### 泛化、自监督、多传感器：2021–2023

| 年份 | Paper | 标签 | 优先级 | 为什么读 |
|---|---|---|---|---|
| 2021 | [Lo et al. — **CLCC: Contrastive Learning for Color Constancy**](https://openaccess.thecvf.com/content/CVPR2021/html/Lo_CLCC_Contrastive_Learning_for_Color_Constancy_CVPR_2021_paper.html) | `[SELF-SUP] [CONTRASTIVE] [CNN]` | **S** | CC 中很重要的 contrastive-learning 思考：illumination 不能被当作应消除的 augmentation。 |
| 2021 | [Abdelhamed, Punnappurath & Brown — **Leveraging the Availability of Two Cameras for Illuminant Estimation**](https://openaccess.thecvf.com/content/CVPR2021/html/Abdelhamed_Leveraging_the_Availability_of_Two_Cameras_for_Illuminant_Estimation_CVPR_2021_paper.html) | `[MULTI-MODAL] [PHYSICS] [EFFICIENCY]` | **S/A** | 利用两个不同 spectral sensitivity 的 sensor 提供额外物理约束。 |
| 2021 | [Afifi et al. — **Cross-Camera Convolutional Color Constancy**](https://openaccess.thecvf.com/content/ICCV2021/html/Afifi_Cross-Camera_Convolutional_Color_Constancy_ICCV_2021_paper.html) | `[CROSS-CAMERA] [LOG-CHROMA]` | **S+** | C5；正式把目标推到“训练中从未见过的新 camera”。 |
| 2021 | [Kim et al. — **Large Scale Multi-Illuminant (LSMI) Dataset for Developing White Balance Algorithm Under Mixed Illumination**](https://www.dykim.me/projects/lsmi) | `[DATASET] [MULTI-ILLUM]` | **S** | 提供逐像素光照、各光源颜色与混合比例真值，并研究逐像素白平衡；理解真实多光源数据的采集与监督方式。 |
| 2021 | [Qian et al. — **A Benchmark for Burst Color Constancy**](https://trepo.tuni.fi/handle/10024/218396) | `[TEMPORAL] [DATASET] [BENCHMARK]` | **S** | 提供 600 段真实拍摄序列、固定评测划分与 TCCNet 基线；按正式出版版本收录，2020 年预印本题名为 A Benchmark for Temporal Color Constancy。 |
| 2021 | [Laakom et al. — **INTEL-TAU: A Color Constancy Dataset**](https://trepo.tuni.fi/handle/10024/215902) | `[DATASET] [CROSS-CAMERA] [BENCHMARK]` | **S** | 研究相机与场景不变性，并提供颜色渐晕校正前后的数据；此处按 IEEE Access 2021 正式版本收录，预印本发表于 2019 年。 |
| 2022 | [Ono et al. — **Degree-of-Linear-Polarization-Based Color Constancy**](https://openaccess.thecvf.com/content/CVPR2022/html/Ono_Degree-of-Linear-Polarization-Based_Color_Constancy_CVPR_2022_paper.html) | `[PHYSICS] [MULTI-MODAL] [MULTI-ILLUM]` | **S/A** | 利用 polarization physics 减少假设，并支持 multi-illumination。 |
| 2022 | [Afifi, Brubaker & Brown — **Auto White-Balance Correction for Mixed-Illuminant Scenes**](https://openaccess.thecvf.com/content/WACV2022/html/Afifi_Auto_White-Balance_Correction_for_Mixed-Illuminant_Scenes_WACV_2022_paper.html) | `[sRGB-WB] [MULTI-ILLUM] [CNN]` | **S** | 学习空间权重以融合不同白平衡预设渲染的图像，连接 sRGB 白平衡编辑与多光源校正。 |
| 2023 | [Buzzelli, Schettini & Bianco — **Learning Color Constancy: 30 Years Later**](https://library.imaging.org/cic/articles/31/1/17) | `[ML] [HISTORY]` | **S/A** | 以 1996 Learning Color Constancy 为起点回看 learning-based CC。 |
| 2023 | [Li et al. — **Ranking-Based Color Constancy With Limited Training Samples**](https://doi.org/10.1109/TPAMI.2023.3278832) | `[ML] [FEW-SHOT]` | **A** | 从有限训练样本角度研究数据效率。 |
| 2023 | [Lin et al. — **Color Constancy: How to Deal with Camera Bias?**](https://proceedings.bmvc2023.org/643/) | `[CROSS-CAMERA]` | **S/A** | 直接分析 camera bias 与 cross-camera evaluation。 |
| 2023 | [Xie et al. — **Camera-Independent Color Constancy by Scene Semantics**](https://www.sciencedirect.com/science/article/pii/S0167865523001010) | `[CROSS-CAMERA] [SEMANTIC]` | **A** | 用 gray assumptions 的 camera-independent 性质结合 scene semantics。 |
| 2023 | [Rizzo et al. — **Cascading Convolutional Temporal Color Constancy**](https://github.com/matteo-rizzo/cctcc) | `[TEMPORAL] [CNN] [EFFICIENCY]` | **A** | 将级联校正与 TCCNet 结合，分析使用帧数、精度与推理开销的关系；按 Journal of Electronic Imaging 2023 正式版本收录。 |

### Multi-Illuminant、Attention、Spectral：2024–2025

| 年份 | Paper | 标签 | 优先级 | 为什么读 |
|---|---|---|---|---|
| 2024 | [Han, Ha & Kim — **Spatio-Spectral Deep Color Constancy With Multi-Band NIR**](https://doi.org/10.1109/ACCESS.2024.3434574) | `[MULTI-MODAL] [ATTENTION] [SPECTRAL]` | **A** | RGB + multi-band NIR；把 CC 推向更丰富的 spectral observation。 |
| 2024 | [Kim et al. — **Attentive Illumination Decomposition Model for Multi-Illuminant White Balancing**](https://openaccess.thecvf.com/content/CVPR2024/html/Kim_Attentive_Illumination_Decomposition_Model_for_Multi-Illuminant_White_Balancing_CVPR_2024_paper.html) | `[MULTI-ILLUM] [ATTENTION]` | **S+** | Slot Attention 显式分解多个 light source 的 chromaticity + spatial weight maps。 |
| 2024 | [Li et al. — **NightCC: Nighttime Color Constancy via Adaptive Channel Masking**](https://openaccess.thecvf.com/content/CVPR2024/papers/Li_NightCC_Nighttime_Color_Constancy_via_Adaptive_Channel_Masking_CVPR_2024_paper.pdf) | `[ROBUST] [MULTI-ILLUM] [CNN]` | **A** | 补充夜间颜色恒常场景，关注日间训练到夜间应用的域差异与夜间多光源评测。 |
| 2024 | [Yue & Wei — **Effective Cross-Sensor Color Constancy Using a Dual-Mapping Strategy**](https://shuweiyue.com/pdf/josa-cross2024.pdf) | `[CROSS-CAMERA] [ML] [EFFICIENCY]` | **A** | DMCC 利用训练与测试传感器的 D65 白点重建图像和光源数据，并训练轻量 MLP；依赖目标传感器的 D65 白点标定。 |
| 2024 | [Afifi, Hu & Liang — **Optimizing Illuminant Estimation in Dual-Exposure HDR Imaging**](https://www.ecva.net/papers/eccv_2024/papers_ECCV/papers/00206.pdf) | `[PHYSICS] [EFFICIENCY]` | **S** | 从长短曝光图像提取双曝光特征（DEF），构建 EMLP / ECCC 轻量光源估计器，连接 HDR 成像与 AWB 联合设计。 |
| 2025 | [Wei et al. — **Integral Fast Fourier Color Constancy**](https://openaccess.thecvf.com/content/CVPR2025/html/Wei_Integral_Fast_Fourier_Color_Constancy_CVPR_2025_paper.html) | `[MULTI-ILLUM] [LOG-CHROMA] [EFFICIENCY]` | **S+** | FFCC 延伸到 multi-illuminant + real-time。 |
| 2025 | [Feng et al. — **Accelerated Self-Supervised Multi-Illumination Color Constancy With Hybrid Knowledge Distillation**](https://doi.org/10.1109/TPAMI.2025.3583090) | `[MULTI-ILLUM] [SELF-SUP] [ATTENTION] [EFFICIENCY]` | **S** | Self-supervised pretraining + Transformer/U-Net + knowledge distillation。 |
| 2025 | [Afifi et al. — **Time-Aware Auto White Balance in Mobile Photography**](https://www.openaccess.thecvf.com/content/ICCV2025/papers/Afifi_Time-Aware_Auto_White_Balance_in_Mobile_Photography_ICCV_2025_paper.pdf) | `[EFFICIENCY]` | **A** | 利用拍摄时间、地理位置与 ISP 信息辅助轻量光源估计；时间指拍摄上下文，不是视频帧间平滑。 |
| 2025 | [Kim et al. — **CCMNet: Leveraging Calibrated Color Correction Matrices for Cross-Camera Color Constancy**](https://openaccess.thecvf.com/content/ICCV2025/html/Kim_CCMNet_Leveraging_Calibrated_Color_Correction_Matrices_for_Cross-Camera_Color_Constancy_ICCV_2025_paper.html) | `[CROSS-CAMERA] [PHYSICS] [EFFICIENCY]` | **S** | 利用预标定 CCM 将预定义光源映射到相机 RAW 空间，编码相机指纹以适配未见相机；无需重新训练，但依赖目标相机的 CCM。 |
| 2025 | [Serrano-Lozano et al. — **Revisiting Image Fusion for Multi-Illuminant White-Balance Correction**](https://openaccess.thecvf.com/content/ICCV2025/html/Serrano-Lozano_Revisiting_Image_Fusion_for_Multi-Illuminant_White-Balance_Correction_ICCV_2025_paper.html) | `[sRGB-WB] [MULTI-ILLUM] [ATTENTION]` | **A** | 分析白平衡预设线性融合的局限，提出 Transformer 融合与多光源 sRGB 数据集；适合接在 MixedWB 2022 后阅读。 |
| 2025 | [Zhao, Afifi & Brown — **Learning Camera-Agnostic White-Balance Preferences**](https://openaccess.thecvf.com/content/ICCV2025W/MIPI/papers/Zhao_Learning_Camera-Agnostic_White-Balance_Preferences_ICCVW_2025_paper.pdf) | `[CROSS-CAMERA] [EFFICIENCY]` | **A** | ICCV Workshop 2025；在相机无关空间学习中性光源估计到目标白平衡偏好的映射，补充跨相机感知偏好与风格一致性。 |

### 2026 前沿：鲁棒性、光谱、VLM 与新架构

| 年份 | Paper | 标签 | 优先级 | 为什么读 |
|---|---|---|---|---|
| 2026 | [Kang, Baek & Kim — **Graph-Based Spectral Attention with Multi-Spectral Images for Illuminant Estimation**](https://openaccess.thecvf.com/content/WACV2026/html/Kang_Graph-Based_Spectral_Attention_with_Multi-Spectral_Images_for_Illuminant_Estimation_WACV_2026_paper.html) | `[MULTI-MODAL] [SPECTRAL] [ATTENTION]` | **A** | RGB→multispectral representation + graph spectral attention。 |
| 2026 | [Xie et al. — **Boosting Illuminant Estimation in Deep Color Constancy through Brightness Robustness Enhancement**](https://doi.org/10.1016/j.patcog.2025.112153) | `[ROBUST] [CNN]` | **A** | 系统讨论 deep CC 的 brightness vulnerability。 |
| 2026 | [Du et al. — **CSNet: A Content and Structure-Aware Approach for Color Constancy**](https://www.sciencedirect.com/science/article/pii/S1077314226000056) | `[CNN] [STRUCTURE]` | **B/A** | content + structure-aware decomposition / fusion。 |
| 2026 | [Li, Tan & Tan — **White-Balance First, Adjust Later: Cross-Camera Color Constancy via Vision-Language Evaluation**](https://openaccess.thecvf.com/content/CVPR2026/html/Li_White-Balance_First_Adjust_Later_Cross-Camera_Color_Constancy_via_Vision-Language_Evaluation_CVPR_2026_paper.html) | `[VLM] [CROSS-CAMERA]` | **A / 前沿** | VLM-CC：从直接 RGB regression 转向迭代的视觉语言反馈。 |
| 2026 | [Liu et al. — **CC-Mamba: Mamba-Based Color Constancy with Illumination Prior-Guided Dynamic Feature Modulation and Wavelet-Domain Attention Mechanism**](https://doi.org/10.1016/j.neucom.2026.133068) | `[MAMBA] [ATTENTION]` | **B / 前沿** | State-space / Mamba 架构进入 CC；更适合观察 architecture trend。 |

## Datasets

数据集推动了 AWB 从单相机、单光源估计逐步走向跨相机和多光源场景。下表中的规模按本仓库路线图所采用的版本统计；实际使用前应确认数据版本、RAW 黑电平、颜色空间、色卡掩码和官方划分。

| 年份 | 数据集 | 规模 | 相机 | 主要特点 / 用途 | 资源 |
| --- | --- | ---: | ---: | --- | --- |
| 2008 | Gehler–Shi | 568 张 RAW | 2 | 经典单光源基准；包含室内、室外与近景图像 | [数据与说明](https://www.cs.sfu.ca/~colour/data/) |
| 2014 | NUS 8-Camera | 1,736 张 | 8 | 多传感器采集，适合研究相机相关性与跨相机泛化 | [项目与数据](https://yorkucvil.github.io/projects/public_html/illuminant/illuminant.html) |
| 2017 | Cube+ | 1,707 张 | 1 | 使用 SpyderCube 标注，场景与光源分布更丰富 | [官方页面](https://ipg.fer.hr/ipg/resources/color_constancy) |
| 2019 | Rendered WB | 65K+ sRGB 图像对 | 多设备 | 面向相机渲染后图像的白平衡校正，而非仅在 RAW 域估计光源 | [项目与数据](https://yorkucvil.github.io/projects/public_html/sRGB_WB_correction/dataset.html) |
| 2019 | INTEL-TAU | 7,022 张 | 3 | 大规模高分辨率数据；支持场景不变性、相机不变性与色彩渐晕研究 | [官方数据页](https://researchportal.tuni.fi/en/datasets/intel-tau/) · [论文](https://arxiv.org/abs/1910.10404) |
| 2021 | LSMI | 7,486 张 | 3 | 包含两种或三种光源，提供逐像素照明与混合比例真值 | [项目、论文与数据](https://www.dykim.me/projects/lsmi) |
| 2023 | Shadows & Lumination | 2,500 张 | 5 | 真实双光源场景，包含照明估计值与逐像素区域掩码 | [论文与数据说明](https://doi.org/10.1016/j.eswa.2023.120045) |

数据集选择建议：

- **基础单光源评测**：Gehler–Shi、Cube+。
- **跨相机泛化**：NUS 8-Camera、INTEL-TAU。
- **sRGB 后处理白平衡**：Rendered WB。
- **多光源与局部 AWB**：LSMI、Shadows & Lumination。

## Challenges & Future Directions

![AWB 核心挑战与未来趋势](assets/challenge.png)

### 当前核心挑战

1. **多光源建模**：真实场景常由太阳、天空、室内灯和局部光源共同照明，单一全局增益容易产生局部偏色。
2. **跨相机泛化**：不同传感器具有不同的光谱响应，同一模型可能依赖训练相机，迁移到未知设备后性能下降。
3. **时序稳定性**：视频、预览和录像不仅要求单帧准确，还要避免白平衡闪烁、突变和场景切换时的震荡。
4. **数据偏差**：训练数据在场景、地域、光照、肤色和设备分布上可能不均衡，实验指标未必代表真实用户场景。
5. **效率与部署**：移动端 ISP 对延迟、吞吐、功耗、内存和硬件算子都有严格约束。
6. **目标定义冲突**：物理中性的结果不一定最自然；客观色差、用户偏好和品牌色彩风格可能彼此冲突。

### 未来发展趋势

1. **物理先验与 AI 融合**：把成像模型、光源约束与学习式模型结合，提升可解释性与稳健性。
2. **语义与上下文理解**：利用人物、天空、植被、室内外环境等语义信息辅助光源判断。
3. **偏好感知白平衡**：从唯一“正确”答案扩展到符合场景、用户审美和品牌风格的多目标结果。
4. **局部 / 稠密 AWB**：从全局 RGB 增益转向空间光照场、物体级估计和连续局部校正。
5. **相机无关泛化**：减少对已知传感器和逐设备标定数据的依赖，提升跨模组、跨平台适配能力。
6. **高效轻量 Hybrid AWB**：由传统方法提供可靠先验或回退机制，由学习模型处理复杂场景与感知决策。

这些方向是基于当前问题形态给出的研究路线展望，并不表示所有方向已经形成统一定义或成熟解决方案。

## Current Focus

当前推荐重点关注：

```text
成像物理与统计先验
    → 深度特征与结构化估计
    → 跨相机泛化
    → 多光源与时序稳定
    → 轻量部署
    → 感知偏好与系统级 Hybrid AWB
```

目标不是记住所有方法名称，而是逐步理解真实相机系统中的几个核心矛盾：

**精度 ↔ 稳定性 ↔ 泛化能力 ↔ 推理效率 ↔ 感知偏好**

> 本仓库用于记录 AWB 算法、数据集和研究方向的学习路线，并将随着学习过程持续更新。
