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
