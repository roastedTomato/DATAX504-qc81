# 第一步：数据来源与文件核验

查验日期：2026-09-30。项目范围：RGB-only 作物/杂草语义分割。

## 完成情况

已下载公开 Kaggle 压缩包，检查全部文件的 CRC 完整性，提取原始 RGB 大图、标签和说明文件。未创建训练/验证划分，也未开始训练。

- 下载文件：`dataset.zip`，2,590,700,651 bytes。
- SHA-256：`1cd9eddab9c5e132f679f0b223591c2ef549282920b447a693525f973e4775e3`。
- 压缩包共 239,620 个文件，包含 RGB、标签、指数、大图及 patches。
- 原始 RGB 与标签已提取到 `data/raw/`，未解压指数或预先增强的 patches。
- 全部 21 张 RGB 大图成功解码，均有尺寸一致的标签；原始标签未知颜色像素数均为 0。
- 21 张 RGB 大图的解码像素哈希无重复。这只能排除完全重复，不能排除空间重叠。

## 数据数量与目录

| 发布集合 | RGB 大图 | 对应标签 | RGB patches | 说明 |
| --- | ---: | ---: | ---: | --- |
| trainval | 12 | 12 | 31,680 | 3,960 个未增强图块，每个另有 7 个增强版本 |
| test | 7 | 7 | 2,310 | 每张大图有 330 个 patches |
| test_different_bbch/BBCH15 | 1 | 1 | 110 | 不同生长阶段的额外测试数据 |
| test_different_bbch/BBCH19 | 1 | 1 | 110 | 不同生长阶段的额外测试数据 |

19 张指主要 trainval/test 大图；包括两张额外测试图后共 21 张。主要大图与标签尺寸为 5472 × 3648；两张额外测试图及标签为 2560 × 2816。抽查 RGB/标签 patches 为 256 × 256；没有逐个解码全部 patches。

大图编号：

- trainval：`trainval_00` 至 `trainval_11`。
- test：`test_01` 至 `test_07`。
- 额外测试：`bbch15_img` 和 `bbch19_img`，标签对应 `bbch15_msk` 和 `bbch19_msk`。

来源编号应包含原始集合或目录，不能只使用末尾数字；例如 `trainval/trainval_01` 与 `test/test_01` 是两个不同的文件身份。

## 标签定义

| 类别 | 标签 RGB | 建议训练 class ID |
| --- | --- | ---: |
| Background / Soil | (199, 199, 199) | 0 |
| Crop / Sorghum | (31, 119, 180) | 1 |
| Weeds | (255, 127, 14) | 2 |

这些定义来自压缩包 `readme.txt`，并已在全部原始标签中核验。训练时需要把 RGB 标签转换为整数类别标签。

## 来源、许可证与伦理

发布页面：[Crop-Weed Segmentation UAV RGB+Indices](https://www.kaggle.com/datasets/bouhadjer/crop-weed-segmentation-uav-rgb-indices)。

原始研究：Genze et al. (2022), *Deep learning-based early weed segmentation using motion blurred UAV images of sorghum fields*, [DOI](https://doi.org/10.1016/j.compag.2022.107388)。修改版说明提到 Bouhadjer 的 RGB 植被指数研究。本项目仅使用 RGB 和标签。

Kaggle 页面标注 CC BY 4.0。包内 `License.txt` 使用自定义授权文字，允许研究、教育和商业使用，并要求同时注明原始作者和修改者；它没有提供标准 CC BY 4.0 全文。报告应准确记录这两种说明，保留原许可证，注明修改与预处理，不将两者表述为完全相同的文本。包内许可证含 example.com 联系地址，不应作为真实作者联系信息使用。

本项目使用公开农业影像，不采集个人数据。尚未逐张进行隐私内容目视审查；展示图片时检查是否存在可识别个人或敏感地点信息。结果仅作为课程实验，不据此作出实际喷药决策。

## 防泄漏结论与下一步

原作者 [save_patches.py](https://github.com/grimmlab/UAVWeedSegmentation/blob/main/save_patches.py#L10) 分别读取既有 trainval/test 集合后切图。Kaggle 实际压缩包也保留了 12 张 trainval 和 7 张 test 原图，因此具备从大图建立明确来源分组的条件。

Kaggle patch 文件名按大图前缀统计，trainval 每张对应 2,640 个文件，test 每张对应 330 个文件。但本次没有逐像素验证已有 patch 与原图裁剪位置的关系，也没有核实全部增强方法。因此这些前缀关系是候选映射，不作为已经验证的来源证明。

第二步采用原始 RGB 大图与标签重新生成 patches，保存原图编号及裁剪坐标；先按原图划分 train/validation，再切图和增强。test 大图维持独立用于最终评估，并对生成的来源清单执行无交集检查。

仍未确认：19 张主要大图的完整航次、GPS 和空间重叠情况。文件不同、哈希不同不等于地块无重叠，报告需要保留这个限制。原作者 [train.py](https://github.com/grimmlab/UAVWeedSegmentation/blob/main/train.py#L23) 对 patches 使用普通 KFold，本项目的验证划分需显式按原图分组。

## 可复核文件

- `01_data_audit.ipynb`：课程 notebook 入口。
- `audit_dataset.py`：完整性、解码、颜色与重复核验逻辑；需要 Pillow 和 NumPy。
- `DATA_AUDIT.json`：实际运行产生的目录数量、原图尺寸、哈希、标签像素数与候选前缀统计。
- `data/raw/License.txt`、`data/raw/readme.txt`、`data/raw/folder structure.txt`：压缩包原始说明。

在项目目录运行 `python3 audit_dataset.py` 可重新生成核验结果；或运行 notebook。下载压缩包和原始数据已在项目 `.gitignore` 中排除，不应上传到课程 GitHub 仓库。
