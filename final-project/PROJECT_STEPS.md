# 项目实施步骤

项目：UAV Crop-Weed Segmentation for Precision Agriculture

核心范围：使用 RGB 图像进行语义分割，区分背景/土壤、作物和杂草。使用 Python 3.11+、TensorFlow/Keras 和 Jupyter notebooks，遵循《Deep Learning with Python》第三版的工作流程。学生需要理解并验证所有提交代码。

## 1. 确认数据和来源

- 下载并检查 [Kaggle 数据集](https://www.kaggle.com/datasets/bouhadjer/crop-weed-segmentation-uav-rgb-indices)，确认 RGB 图像、分割标签、类别颜色及许可证。
- 确认每个 patch 对应的原始 UAV 图像，记录来源编号；如有航次信息，一并记录。
- 核对 Kaggle 修改版是否保留原始数据的 trainval/test 划分。已有文件夹名称不足以证明没有泄漏。
- 若无法恢复 patch 来源关系，考虑使用原作者的大图重新切图，并保留来源对应表。

产出：数据来源说明、目录与数量统计、patch 来源对应关系。

## 2. 划分训练、验证和测试集

- 按原始 UAV 图像分组，确保同一原图的所有 patches 只进入一个集合。
- 仅在确认来源隔离后沿用发布者的 test；从 trainval 中按原图分组划出 validation。
- 如果有航次或空间信息，进一步检查不同原图之间的拍摄区域重叠，避免夸大跨航次泛化能力。
- 保存划分清单，包含文件路径、来源图像编号、可用的航次编号及所属集合。
- 用代码验证 train、validation、test 的来源编号两两无交集；增强版本也必须与原图留在同一集合。

产出：固定划分清单、各集合的原图和 patch 数量、防泄漏检查结果。

## 3. 探索数据和建立预处理流程

- 展示 RGB 图像与标签，统计各类别像素比例，检查缺失文件、错误标签和图像与标签配对。
- 确定输入尺寸、归一化和标签编码；标签缩放使用最近邻插值。
- 数据增强只用于训练集，几何变换对图像与标签同步执行。
- 若需要从数据计算归一化统计量或类别权重，仅使用训练集。

产出：数据探索与预处理 notebook、样例图、类别分布统计。

## 4. 完成基线模型

- 评估所有像素预测为训练集最多类别的简单基线。
- 用 TensorFlow/Keras 训练小型 encoder-decoder CNN。
- 验证训练、保存、加载、评估和预测的完整流程。
- 固定随机种子，记录配置、训练时间和训练曲线。

产出：可运行的 baseline notebook、基线指标和模型权重，作为 Week 11 checkpoint 的核心。

## 5. 训练 U-Net

- 使用与 CNN 相同的数据划分和指标训练 U-Net。
- 在 Apple M4 Pro 上小规模试跑，确认可用设备、内存需求、batch size 和训练时长。
- 根据验证集选择最佳模型，记录 early stopping 等训练设置。
- 保存最佳权重、配置与训练曲线，模型选择期间保留测试集用于最终评估。

产出：U-Net notebook、最佳模型、训练记录。

## 6. 进行对比实验和错误分析

- 比较简单基线、CNN 和 U-Net。
- 做一个明确的 ablation，优先比较 U-Net 有无数据增强，其他设置保持一致。
- 使用 Mean IoU 作为主要指标，同时报告各类别 IoU、Dice 和 weed-class IoU，明确计算方式。
- 模型选择完成后评估测试集，不根据测试结果反复调参。
- 展示成功和失败案例，分析漏检小杂草、作物与杂草混淆、阴影误检及边界误差。
- 结合原图数量和拍摄条件说明结果的局限。

产出：结果对比表、训练曲线、各类别指标图、预测图及错误分析。

## 7. 整理代码和写报告

- 整理可从头运行的 notebooks、依赖版本、随机种子、数据划分清单和 README 复现说明。
- 根据 [A+ 项目思路](A_PLUS_PROJECT_PLAN.md) 写报告，包含问题与相关工作、数据与预处理、模型与训练、结果、错误分析、AI 工具披露、结论与参考文献。
- 重点解释来源分组、防泄漏检查、实验对比及局限；所有结果来自实际运行。
- 核实引用、数据许可证和 AI 生成内容，确保能解释每段训练代码。

产出：GitHub 仓库、README、最终报告 PDF（fp_final_Lastname_Firstname.pdf）。

## 8. 准备展示和提交

- 准备 8 分钟展示及 2 分钟 Q&A，覆盖问题、数据、模型、结果、局限和 AI 使用。
- 准备预测 demo，并保留离线预测图作为备用。
- 练习解释来源划分、U-Net、IoU、ablation 和训练代码。
- 核对 Moodle 提交要求，提交报告 PDF 和 GitHub 链接，并确认复现说明完整。

产出：展示材料、demo、最终提交。

## 实施优先级

先完成步骤 1-3，再建立完整的 baseline notebook，随后完成 U-Net、对比实验、报告和展示。RGB-only 是核心范围；vegetation indices 等扩展仅在核心实验与交付完成后考虑。

## 已查验的数据划分依据

- [Kaggle 发布说明](https://www.kaggle.com/datasets/bouhadjer/crop-weed-segmentation-uav-rgb-indices)：说明数据修改自 Genze 等人（2022），但未明确证明修改版保留了来源隔离。
- [原作者 save_patches.py](https://github.com/grimmlab/UAVWeedSegmentation/blob/main/save_patches.py#L10)：分别读取 trainval、test、test_different_bbch 后切图，支持原始发布流程先分集合再切 patches。
- [原作者 train.py](https://github.com/grimmlab/UAVWeedSegmentation/blob/main/train.py#L23)：使用普通 KFold 对 patch 列表划分，不能直接作为按原图分组验证的证据。
- [原作者 patch_utils.py](https://github.com/grimmlab/UAVWeedSegmentation/blob/main/utils/patch_utils.py#L95)：patch 文件名使用集合名与流水号，未直接保存原图编号。

查验状态：已查发布说明与源码，尚未完成 Kaggle 实际文件的逐项来源核验。不能据此声明 Kaggle 修改版已经无泄漏。
