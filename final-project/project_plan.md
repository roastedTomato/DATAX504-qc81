# 项目规划 / Project Plan

**UAV Crop-Weed Segmentation for Precision Agriculture**

中文：本文件统一保存项目说明和待办框架。代码、运行结果及必要的技术解释写在 `project_todo.ipynb`，全部使用英文。以下待办用于规划，不记录历史完成情况。

English: This file contains the project description and task framework. Code, outputs, and necessary technical explanations belong in `project_todo.ipynb`, in English only. The checklist is a planning framework, not a record of previous work.

## 1. 确认数据和来源 / Define the Problem and Review the Data

### 1.1 问题定义 / Problem Definition

**应用场景与目标 / Application and Goal**

中文：利用无人机拍摄的高粱田 RGB 图像，识别作物、杂草及背景/土壤的位置。输出用于展示图像内的杂草分布，为精准农业中的杂草监测提供实验性支持。

English: Identify crop, weed, and background/soil regions in UAV RGB images of sorghum fields. The output visualizes weed distribution within an image and provides experimental support for weed monitoring in precision agriculture.

**研究问题 / Research Question**

中文：在按原始 UAV 图像隔离的数据划分下，RGB-only 分割模型能否有效区分作物与杂草，并优于简单基线？

English: With data splits separated by original UAV image, can an RGB-only segmentation model distinguish crops from weeds and outperform simple baselines?

**问题类型 / Problem Type**

中文：多类别语义分割，为每个有效像素分配一个类别。

English: Multiclass semantic segmentation, assigning one class to each valid pixel.

**输入 / Input**

中文：UAV RGB 图像或由原始图像裁剪的图块，形状为 H × W × 3。仅使用红、绿、蓝通道。训练时提供对齐的人工分割标签；预测时仅输入 RGB 图像。输入尺寸与预处理在步骤 3 确定。

English: UAV RGB images or patches cropped from original images, with shape H × W × 3. Only red, green, and blue channels are used. Training requires aligned human-annotated masks; inference requires only RGB images. Input size and preprocessing will be defined in Step 3.

**输出 / Output**

中文：H × W × 3 的类别概率图，沿类别维度取最大概率，得到 H × W 的整数分割图。

English: An H × W × 3 class probability map. Taking the highest-probability class at each pixel produces an H × W integer segmentation mask.

**类别 / Classes**

| ID | 中文 | English | 定义 / Definition | 标签 RGB / Mask RGB |
| --- | --- | --- | --- | --- |
| 0 | 背景/土壤 | Background / Soil | 标注中的土壤与背景 / Annotated soil and background | (199, 199, 199) |
| 1 | 作物/高粱 | Crop / Sorghum | 标注中的高粱作物 / Annotated sorghum plants | (31, 119, 180) |
| 2 | 杂草 | Weeds | 所有标注杂草统一为一类 / All annotated weeds treated as one class | (255, 127, 14) |

中文：类别与颜色依据为[数据集说明](https://www.kaggle.com/datasets/bouhadjer/crop-weed-segmentation-uav-rgb-indices)和原始 readme。类别 ID 是项目约定。边缘补齐区域应使用独立忽略标记，不属于上述三类。

English: Classes and colors follow the [dataset description](https://www.kaggle.com/datasets/bouhadjer/crop-weed-segmentation-uav-rgb-indices) and original readme. Class IDs are project conventions. Padded regions must use a separate ignore label and do not belong to these three classes.

**评价目标 / Evaluation Goal**

中文：以三个类别的 Mean IoU 为主要指标，单独关注 weed-class IoU，辅以 Dice 和各类别 IoU。比较多数类别基线、小型 encoder-decoder CNN 和 U-Net。指标实现与实验设置在后续步骤确定。

English: Use Mean IoU across the three classes as the primary metric, with particular attention to weed-class IoU. Report Dice and per-class IoU as supporting metrics. Compare a majority-class baseline, a small encoder-decoder CNN, and U-Net. Metric implementations and experimental settings will be defined in later steps.

**范围与约束 / Scope and Constraints**

中文：使用 Python 3.11+、TensorFlow/Keras 和 Jupyter notebook，遵循《Deep Learning with Python》第三版的项目流程。核心范围为 RGB-only 分割、模型对比与错误分析。不包含植被指数、杂草物种识别、实例计数、地理定位、自动喷药或无人机部署。学生必须理解并验证所有代码。

English: Use Python 3.11+, TensorFlow/Keras, and a Jupyter notebook, following the workflow in Deep Learning with Python, third edition. The core scope covers RGB-only segmentation, model comparison, and error analysis. Vegetation indices, weed species identification, instance counting, geolocation, automatic spraying, and UAV deployment are outside the core scope. The student must understand and verify all code.

### 1.2 数据获取 / Data Acquisition

- [ ] 记录数据集名称、来源链接和下载方式。 / Record the dataset name, source URL, and download method.
- [ ] 获取 RGB 图像及标签，确定存放路径。 / Obtain RGB images and masks and define storage paths.

### 1.3 来源与许可 / Provenance and Licensing

- [ ] 确认原始研究及修改版来源。 / Identify the original research and modified dataset provenance.
- [ ] 核实许可证、署名要求和伦理事项。 / Review licensing, attribution requirements, and ethical considerations.

### 1.4 文件检查 / File Inspection

- [ ] 统计目录、数量、尺寸，检查缺失文件和图像/标签配对。 / Inspect directories, counts, dimensions, missing files, and image-mask pairing.
- [ ] 核实标签颜色与类别定义。 / Verify mask colors and class definitions.

### 1.5 来源与泄漏风险 / Source Mapping and Leakage Risks

- [ ] 确认 patches 与原始 UAV 图像的对应关系。 / Confirm patch-to-original-image mapping.
- [ ] 核实已有 train/test 划分依据。 / Verify the basis of the supplied train/test split.
- [ ] 检查可用航次及空间重叠信息，记录未知限制。 / Review available flight and spatial overlap information and document unresolved limitations.

## 2. 数据划分 / Data Splitting

- [ ] 切图和增强前按原图分组，同一原图及增强版本只属于一个集合。 / Split by original image before patching or augmentation; keep every derivative in the same split.
- [ ] 固定随机种子与来源清单，检查航次和空间相关性。 / Fix the random seed and source manifest; review flight and spatial correlations.
- [ ] 记录裁剪坐标、有效区域及边缘处理。 / Record crop coordinates, valid regions, and edge handling.
- [ ] 验证来源编号无交集，统计原图和 patch 数量。 / Verify disjoint source IDs and report original-image and patch counts.
- [ ] 保留测试集用于最终评估。 / Reserve the test set for final evaluation.

## 3. 数据探索与预处理 / Data Exploration and Preprocessing

- [ ] 展示 RGB、标签和叠加图，检查质量与类别比例。 / Display RGB images, masks, and overlays; inspect quality and class proportions.
- [ ] 检查缺失、错误颜色和尺寸不一致。 / Check missing files, invalid colors, and dimension mismatches.
- [ ] 确定输入尺寸、归一化和整数标签编码。 / Define input size, normalization, and integer label encoding.
- [ ] 标签缩放用最近邻，忽略补齐像素。 / Use nearest-neighbor mask resizing and exclude padded pixels.
- [ ] 建立 TensorFlow 加载、batch 和预取流程。 / Build TensorFlow loading, batching, and prefetching pipelines.
- [ ] 仅训练集增强，图像与标签几何变换同步。 / Augment training data only and synchronize image-mask geometric transforms.
- [ ] 仅训练集计算统计量与类别权重；检查 batch 形状和数值。 / Compute statistics and class weights from training data only; inspect batch shapes and values.

## 4. 基线模型 / Baseline Models

- [ ] 建立训练集多数类别预测基线。 / Build a majority-class baseline using the training set.
- [ ] 定义 IoU、Dice 和忽略像素处理。 / Define IoU, Dice, and ignored-pixel handling.
- [ ] 实现小型 Keras encoder-decoder CNN。 / Implement a small Keras encoder-decoder CNN.
- [ ] 设置损失、优化器、batch size、轮数与 early stopping。 / Configure loss, optimizer, batch size, epochs, and early stopping.
- [ ] 固定种子，记录环境、训练设置和曲线。 / Fix seeds and record the environment, training settings, and curves.
- [ ] 验证保存、加载与预测，准备 checkpoint。 / Verify saving, loading, and inference; prepare the checkpoint submission.

## 5. U-Net / U-Net Training

- [ ] 实现并解释 encoder、decoder 和 skip connections。 / Implement and explain the encoder, decoder, and skip connections.
- [ ] 使用相同划分和指标，M4 Pro 小规模试跑。 / Use the same splits and metrics; perform a small trial on the M4 Pro.
- [ ] 确认设备、内存与训练时长。 / Check device availability, memory requirements, and training time.
- [ ] 根据验证集选模型，保存权重、配置和曲线。 / Select models using validation data; save weights, configurations, and curves.
- [ ] 检查预测尺寸、类别和重新加载后的结果。 / Verify output dimensions, classes, and predictions after reloading.

## 6. 对比与错误分析 / Comparison and Error Analysis

- [ ] 比较多数类别基线、CNN 和 U-Net。 / Compare the majority-class baseline, CNN, and U-Net.
- [ ] 做有无数据增强的 ablation，保持其他设置一致。 / Run an augmentation ablation with other settings held constant.
- [ ] 模型选择结束后评估测试集，不用测试结果调参。 / Evaluate the test set after model selection; do not tune on test results.
- [ ] 报告 Mean IoU、各类别 IoU、weed-class IoU 和 Dice。 / Report Mean IoU, per-class IoU, weed-class IoU, and Dice.
- [ ] 整理对比表、曲线、指标图和预测图。 / Prepare comparison tables, curves, metric plots, and prediction examples.
- [ ] 分析小杂草漏检、类别混淆、阴影误检与边界误差。 / Analyze missed small weeds, class confusion, shadow-related false positives, and boundary errors.
- [ ] 讨论原图数量、来源相关性、不平衡与泛化限制。 / Discuss source-image count, correlations, imbalance, and generalization limitations.

## 7. 代码与报告 / Code and Report

- [ ] 从头运行 notebook，整理依赖、种子、划分与配置。 / Run the notebook from the beginning and document dependencies, seeds, splits, and configurations.
- [ ] 编写 README 的环境、数据获取和复现步骤。 / Document setup, data acquisition, and reproduction in the README.
- [ ] 撰写引言与相关工作、数据与预处理、模型与训练、结果、错误分析、AI 披露、结论与参考文献。 / Write introduction and related work, data and preprocessing, models and training, results, error analysis, AI disclosure, conclusion, and references.
- [ ] 使用真实结果，核实引用与许可，并理解所有提交代码。 / Use actual results, verify references and licensing, and understand all submitted code.
- [ ] 导出 fp_final_Lastname_Firstname.pdf，整理课程 GitHub 仓库。 / Export fp_final_Lastname_Firstname.pdf and organize the course GitHub repository.

## 8. 展示与提交 / Presentation and Submission

- [ ] 准备 8 分钟展示和 2 分钟 Q&A。 / Prepare an eight-minute presentation and two-minute Q&A.
- [ ] 展示问题、数据、模型、结果、局限与 AI 使用。 / Present the problem, data, models, results, limitations, and AI usage.
- [ ] 准备 demo 及离线结果备用，练习解释代码和关键概念。 / Prepare a demo and offline backup results; practice explaining code and key concepts.
- [ ] 核对评分细则、报告、仓库链接与复现说明，完成 Moodle 提交。 / Check the rubric, report, repository link, and reproduction instructions; submit through Moodle.
