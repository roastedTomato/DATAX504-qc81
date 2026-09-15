# DATAX504 Midterm Question Bank | 中英文复习题库：150 题

本题库依据课程复习大纲、Assignment 1–3 和教材第三版第 1–8 章自行编写，用于理解与练习，不是老师原题或官方评分标准。不包含模拟卷。考试有 12 道选择题，并有简答及较长案例题；本题库的各类数量不代表考试题量。

范围以原始大纲为准：不包括第 9–10 章的 Grad-CAM、filter visualisation 和超出第 8 章的现代 ConvNet 架构。Embedding 只解释 A3 已使用的基本操作，不扩展到后续 NLP 章节。题目重在解释、选择、诊断，不要求推导微积分证明。

**阅读约定：** EN 为英文题干；中文为对照题干；English answer 可用于练习英文作答；中文解析帮助理解；得分点为自评参考。选择题均为四选一。作业单元格编号从 **1** 开始，包含 Markdown 单元格，不是 `In [执行次数]`。代码摘录省略注释或无关行时会说明。案例除明确标记“实际输出”外，均为自拟情境。来源缩写均可点击，详见文末。

## 目录

- [A. 选择题：Q001–Q054](#multiple-choice)
- [B. 简答题：Q055–Q108](#short-response)
- [C. 代码解读题：Q109–Q132](#code-reading)
- [D. 案例分析题：Q133–Q150](#case-analysis)
- [大纲考点覆盖表](#coverage)
- [来源与真实结果核对](#sources)

<a id="multiple-choice"></a>
## A. Multiple Choice | 选择题（54 题）

### Q001 · Hierarchical representations

**EN:** What best describes hierarchical representation learning?

**中文：** 哪项最准确描述分层表示学习？

- A. Memorising every label / 记住所有标签
- B. Learning successive transformations from simple to more abstract features / 逐层从简单特征学习到更抽象特征
- C. Manually coding every feature / 手工编写所有特征
- D. Increasing only the number of training examples / 只增加样本数

**English answer:** B. Later layers can combine features learned by earlier layers.

**中文解析：** B 正确，重点是表示逐层变化。A 是记忆，不等于分层学习；C 不符合自动学习特征；D 改变数据量，不定义网络深度。

**得分点：** successive layers；learned features。**来源：** [G]；[C1] 分层表示。

### Q002 · Linear stacks

**EN:** What happens if every hidden activation in a Dense stack is linear?

**中文：** Dense 网络所有隐藏激活均为线性时，会怎样？

- A. It automatically becomes a ConvNet / 自动变成卷积网络
- B. It can express every nonlinear boundary / 能表达所有非线性边界
- C. The stack is equivalent to one affine transformation / 可合并为一个仿射变换
- D. Its weights cannot be updated / 权重无法更新

**English answer:** C. Composing affine layers still gives an affine mapping.

**中文解析：** C 正确，含 bias 严格称“仿射”，通常口语称线性层。A 没有卷积；B 需要非线性；D 仍可计算梯度和更新。

**得分点：** 多层线性不能替代非线性。**来源：** [G]；[A1] 单元格 11。

### Q003 · ReLU calculation

**EN:** What is ReLU applied to `[-2, 0, 3]`?

**中文：** 对 `[-2, 0, 3]` 使用 ReLU 得到什么？

- A. `[0, 0, 3]`
- B. `[-2, 0, 3]`
- C. `[2, 0, 3]`
- D. `[0, 0, 1]`

**English answer:** A. ReLU returns the maximum of zero and each input.

**中文解析：** A 正确；B 是恒等操作；C 是绝对值；D 错把正数截断为 1。ReLU 的正数输出没有 1 这个上限。

**得分点：** `max(0,x)`。**来源：** [A1] 单元格 7。

### Q004 · Vanishing gradients

**EN:** Why can ReLU help compared with sigmoid in deep hidden stacks?

**中文：** 深层隐藏网络中，ReLU 相比 sigmoid 有什么帮助？

- A. It guarantees perfect accuracy / 保证完全正确
- B. Its derivative is one everywhere / 所有位置导数都为 1
- C. It removes the need for backpropagation / 不再需要反向传播
- D. Its positive branch avoids sigmoid-like saturation / 正数分支避免 sigmoid 式饱和

**English answer:** D. ReLU has derivative one for positive inputs, so those units do not shrink gradients through their activation derivative.

**中文解析：** D 正确。sigmoid 很平时梯度小，多层相乘会进一步缩小。A 无保证；B 负数区域导数为 0；C 仍要反向传播。ReLU 不能保证整个网络没有梯度问题。

**得分点：** saturation；positive inputs；非万能。**来源：** [G]；[A1] 单元格 7。

### Q005 · Loss

**EN:** What does training loss primarily measure?

**中文：** training loss 主要衡量什么？

- A. Training duration / 训练时间
- B. Prediction error under the chosen objective / 所选目标下预测的误差
- C. Number of layers / 层数
- D. Guaranteed future accuracy / 保证的未来准确率

**English answer:** B. It scores predictions against targets according to the selected loss function.

**中文解析：** B 正确；A 是耗时，C 是结构，D 需要独立评估。较低训练 loss 不直接保证新数据效果。

**得分点：** predictions vs targets。**来源：** [G]；[A1] 单元格 11。

### Q006 · Training order

**EN:** Which order matches one ordinary training update?

**中文：** 一次常规训练更新的正确顺序是什么？

- A. Update → loss → forward → gradients / 先更新再计算
- B. Save → test → predict → stop / 保存、测试、预测、停止
- C. Forward → loss → gradients → update / 前向、loss、梯度、更新
- D. Gradients → delete labels → forward / 梯度、删标签、前向

**English answer:** C. Predictions and loss are computed before their gradients are used to update weights.

**中文解析：** C 与 A1 的训练循环一致；A 顺序颠倒；B 不是训练更新；D 缺少监督目标，顺序也不对。

**得分点：** 四步顺序。**来源：** [A1] 单元格 13。

### Q007 · Mini-batch trade-off

**EN:** Why are mini-batches commonly used?

**中文：** 为什么常用 mini-batch？

- A. They balance computational efficiency, memory and gradient noise / 平衡效率、内存与梯度噪声
- B. They always remove all gradient noise / 消除所有梯度噪声
- C. They use validation labels to update weights / 用验证标签更新权重
- D. They require exactly 100 samples / 必须恰好 100 个样本

**English answer:** A. Mini-batches offer a practical compromise between single-example and full-dataset updates.

**中文解析：** A 正确；B 仍有采样噪声；C 验证集不参与梯度更新；D 大小可选，例如 A2 的 128、A3 的 512。

**得分点：** practical compromise。**来源：** [G]；[A2] 单元格 12；[A3] 单元格 7。

### Q008 · Dense weight shape

**EN:** A `Dense(64)` receives 10 features. What is the kernel shape, excluding bias?

**中文：** 输入 10 个特征的 `Dense(64)`，权重矩阵形状是什么？不计 bias。

- A. `(64, 10)`
- B. `(10,)`
- C. `(64,)`
- D. `(10, 64)`

**English answer:** D. An input shaped `(batch,10)` multiplies a `(10,64)` kernel.

**中文解析：** D 正确；A 方向相反；B 是输入向量长度；C 可对应 bias 形状，不是 kernel。

**得分点：** input features × units。**来源：** [G]；[A1] 单元格 9、11。

### Q009 · Parameter count

**EN:** How many trainable parameters are in A1's `2 → 16 → 1` network, including biases?

**中文：** A1 的 `2 → 16 → 1` 网络含偏置有多少参数？

- A. 48
- B. 65
- C. 32
- D. 1000

**English answer:** B. The count is `2*16 + 16 + 16*1 + 1 = 65`.

**中文解析：** B 正确；A 只算两组权重；C 只算第一层权重；D 是样本数，不是参数数。

**得分点：** 权重与偏置均计入。**来源：** [A1] 单元格 9 代码及输出。

### Q010 · Update direction

**EN:** If `w=1`, gradient `=0.2`, and learning rate `=0.1`, what is one gradient-descent update?

**中文：** 给定这些值，一次梯度下降后的 w 是多少？

- A. 0.98
- B. 1.02
- C. 0.80
- D. 1.20

**English answer:** A. Subtract `0.1*0.2` from 1.

**中文解析：** A 正确；B 加了梯度，是上升方向；C 忘记乘学习率；D 既漏学习率又方向错误。数值为自拟练习。

**得分点：** `w -= lr*gradient`。**来源：** [A1] 单元格 13。

### Q011 · Tensor axes

**EN:** What does `(128,28,28,1)` represent for a channels-last grayscale image batch?

**中文：** channels-last 灰度图批次的 `(128,28,28,1)` 表示什么？

- A. 128 classes / 128 个类别
- B. One image with 128 channels / 一张 128 通道图
- C. 128 images, each 28 by 28 with one channel / 128 张 28×28 单通道图
- D. 28 images with 128 labels / 28 张图、128 个标签

**English answer:** C. The axes are batch, height, width and channels.

**中文解析：** C 正确；A 混淆 batch 与类别；B 轴顺序错误；D 无法从输入形状推断这种标签关系。

**得分点：** batch/H/W/C。**来源：** [G] 张量与 ConvNet 形状；[C2] 张量。

### Q012 · Learning rate

**EN:** What is a plausible symptom of a learning rate that is too large?

**中文：** 学习率过大可能出现什么现象？

- A. Loss must decrease smoothly / loss 必然平滑下降
- B. Labels automatically become one-hot / 标签自动变 one-hot
- C. The number of parameters changes / 参数数量改变
- D. Loss oscillates strongly or diverges / loss 强烈震荡或发散

**English answer:** D. Updates can overshoot useful regions of the loss surface.

**中文解析：** D 是可能现象；A 不保证；B 与标签编码无关；C 学习率不改变结构。震荡也可能有其他原因，需要检查。

**得分点：** overshooting；非唯一诊断。**来源：** [A1] 单元格 9、13；[G] 训练行为。

### Q013 · Binary sentiment

**EN:** Which standard probability-output pairing fits A3's positive/negative sentiment task?

**中文：** A3 正负情感二分类适合哪种概率输出与 loss 搭配？

- A. `Dense(1, sigmoid)` + binary cross-entropy
- B. `Dense(1, softmax)` + sparse categorical cross-entropy
- C. `Dense(10, softmax)` + mean squared error
- D. `Dense(1, relu)` + accuracy as the loss

**English answer:** A. One sigmoid output models the probability of the positive class.

**中文解析：** A 正确；B 单个 softmax 输出恒为 1；C 类别数和目标不匹配；D ReLU 不限制为概率，accuracy 也不是这里的训练 loss。

**得分点：** 二分类、一个概率、BCE。**来源：** [A3] 单元格 7。

### Q014 · Sparse multiclass

**EN:** Which pairing fits ten exclusive classes with labels such as `7`?

**中文：** 十个互斥类别，标签形如整数 `7`，应选什么？

- A. One sigmoid + mean squared error
- B. Ten sigmoid outputs + no loss
- C. Ten softmax outputs + sparse categorical cross-entropy
- D. One ReLU + binary cross-entropy

**English answer:** C. Softmax provides ten class probabilities, while the sparse loss accepts an integer target.

**中文解析：** C 正确；一张数字图片只对应一个数字。A、D 输出数不对；B 没有训练 loss，也没有表达互斥归一化。sparse 指标签格式，不是类别稀少。

**得分点：** single-label；integer ID。**来源：** [A2] 单元格 10、20。

### Q015 · One-hot targets

**EN:** If ten-class labels are changed to one-hot vectors, which probability-based loss directly matches them?

**中文：** 十分类标签改成 one-hot 后，哪种基于概率的 loss 直接匹配？

- A. Sparse categorical cross-entropy with unchanged label shape
- B. Categorical cross-entropy
- C. Binary accuracy as a loss
- D. No loss is needed

**English answer:** B. Categorical cross-entropy accepts the full target vector with a softmax output.

**中文解析：** B 正确；A 要整数 ID，不能原样输入 one-hot；C 混淆指标和目标；D 监督训练仍需 loss。例：三分类 ID 2 对应 `[0,0,1]`。

**得分点：** sparse vs one-hot。**来源：** [G]；[A2] 单元格 20。

### Q016 · Multilabel

**EN:** A document can have both “sport” and “health” tags. Which standard output fits?

**中文：** 文档可同时有“运动”和“健康”标签，哪种输出合适？

- A. One softmax over all tags / 所有标签共用 softmax
- B. One integer with MSE / 一个整数配 MSE
- C. No activation because labels overlap / 标签重叠所以不用激活
- D. One sigmoid per tag with binary cross-entropy / 每标签独立 sigmoid 配 BCE

**English answer:** D. Each tag is a separate binary decision, so several tags can receive high probabilities.

**中文解析：** D 正确；A 让标签竞争同一份总概率；B 不能直接表达多个标签；C 没提供合适的概率和 loss 搭配。

**得分点：** 多标签不互斥。**来源：** [G] 分类搭配。

### Q017 · Regression

**EN:** Which standard setup predicts an unrestricted continuous target?

**中文：** 预测无额外范围限制的连续数值，哪种设置常用？

- A. One linear output + mean squared error / 一个线性输出配 MSE
- B. Ten softmax outputs + sparse loss
- C. One sigmoid + categorical cross-entropy
- D. One softmax output + binary accuracy

**English answer:** A. A linear output can represent continuous values without forcing class probabilities.

**中文解析：** A 正确；B、C 是分类结构或错误搭配；D 单个 softmax 恒为 1，且 accuracy 不适合一般连续值预测。

**得分点：** continuous target；linear/MSE。**来源：** [G] 第 3–4 章主题；[C4] 回归。

### Q018 · Softmax guarantee

**EN:** What does softmax guarantee for finite logits, up to numerical precision?

**中文：** 对有限 logits，softmax 保证什么？忽略数值精度误差。

- A. The largest entry is always the true class / 最大项必是真实类
- B. Exactly one probability is 1 / 必定恰有一个 1
- C. Nonnegative probabilities sum to 1 / 非负概率之和为 1
- D. Every class has equal probability / 所有类别概率相等

**English answer:** C. Normalisation does not guarantee a correct prediction.

**中文解析：** C 正确；A 仍可能预测错；B 概率通常不是 one-hot；D 只有特定相同 logits 才会均匀。

**得分点：** 分布约束不等于正确性。**来源：** [G]；[A2] 单元格 20。

### Q019 · Cross-entropy preference

**EN:** The true class is 2. Which predicted probability for class 2 gives the smallest single-example categorical cross-entropy?

**中文：** 真类为 2，分给类 2 的概率是多少时，该样本交叉熵最小？

- A. 0.10
- B. 0.80
- C. 0.25
- D. 0.01

**English answer:** B. The loss is `-log(p_true)`, which decreases as the true-class probability increases.

**中文解析：** B 给正确类最多概率；A、C 更低所以 loss 更高；D 最低且错误最严重。其余类概率须共同补足到 1。

**得分点：** 给真类概率越高，交叉熵越小。**来源：** [A2] 单元格 20；自拟概率。

### Q020 · Adam

**EN:** What does Adam adapt using moving estimates of gradients and squared gradients?

**中文：** Adam 利用梯度与梯度平方的移动估计来调整什么？

- A. The true labels / 真实标签
- B. The number of classes / 类别数
- C. The validation split automatically / 自动验证划分
- D. Parameter-specific update sizes / 各参数的更新尺度

**English answer:** D. Adam rescales updates using gradient history; it does not choose the architecture.

**中文解析：** D 正确；A、B、C 都不是优化器负责修改的对象。Adam 自适应更新不等于 ReduceLROnPlateau 根据验证表现调整全局学习率。

**得分点：** per-parameter updates。**来源：** [G] Adam；[A2] 单元格 10。

### Q021 · Confusion matrix orientation

**EN:** In A2's confusion matrix, what does `cm[4,9]=11` mean?

**中文：** A2 混淆矩阵中 `cm[4,9]=11` 是什么意思？

- A. Eleven true 4s were predicted as 9 / 11 个真实 4 被预测成 9
- B. Eleven true 9s were predicted as 4 / 11 个真实 9 被预测成 4
- C. Eleven 4s were correct / 11 个 4 预测正确
- D. Overall accuracy is 11% / 总准确率 11%

**English answer:** A. Rows are true labels and columns are predictions in this call.

**中文解析：** A 正确；B 把方向倒置；C 应看 `[4,4]`；D 单个计数不是准确率。此数值来自实际输出中的最差方向。

**得分点：** true row / predicted column。**来源：** [A2] 单元格 18。

### Q022 · Test set

**EN:** What is the main purpose of an untouched test set?

**中文：** 保留未使用测试集的主要目的是什么？

- A. Select dropout after every epoch / 每轮用它选 dropout
- B. Compute training gradients / 计算训练梯度
- C. Estimate performance after model choices are fixed / 模型选择完成后估计泛化表现
- D. Guarantee deployment never fails / 保证部署不会失败

**English answer:** C. It provides an evaluation not directly used to choose the model.

**中文解析：** C 正确；A、B 会污染评估；D 无法保证分布变化下的表现。

**得分点：** independent final evaluation。**来源：** [G]；[A3] 单元格 15–16。

### Q023 · Validation set

**EN:** Which use of validation data is appropriate?

**中文：** 哪种验证集用途合适？

- A. Updating weights through its loss each batch / 用其 loss 每批更新权重
- B. Selecting hyperparameters and stopping time / 选择超参数与停止时机
- C. Calling it training data after every trial / 每次试验都改称训练集
- D. Proving the model is universally optimal / 证明全局最优

**English answer:** B. Validation guides model choices without ordinary gradient updates on its examples.

**中文解析：** B 正确；A 是训练用途；C 不能解决数据复用问题；D 验证成绩只能支持当前评估条件下的比较。

**得分点：** tuning，非反向传播。**来源：** [G]；[A2] 单元格 8、12。

### Q024 · Overfitting evidence

**EN:** Which sustained trend most strongly suggests overfitting, assuming a sound split?

**中文：** 假设划分可靠，哪个持续趋势最支持过拟合？

- A. Both losses fall / 两种 loss 都下降
- B. Training has not started / 尚未训练
- C. Both losses are high and flat / 两者高且平
- D. Training loss falls while validation loss rises / 训练 loss 降、验证 loss 升

**English answer:** D. Training improvement is no longer translating to held-out performance.

**中文解析：** D 正确；A 仍在共同改善；B 无数据；C 更像欠拟合或训练失败。应看趋势而非单轮波动。

**得分点：** divergence over epochs。**来源：** [G]；[A2] 单元格 12、16。

### Q025 · Underfitting response

**EN:** Both scores remain poor after checking labels and preprocessing. The model is very small. What is a reasonable next experiment?

**中文：** 检查标签与预处理后，两边成绩仍差且模型很小，下一步合理试验是什么？

- A. Increase capacity moderately / 适度增加容量
- B. Increase dropout to 0.95 / dropout 增到 0.95
- C. Tune on test labels / 用测试标签调参
- D. Declare overfitting solely from low scores / 仅凭低分判过拟合

**English answer:** A. Greater capacity may help the model fit the training signal.

**中文解析：** A 是合理试验而非必然修复；B 可能使学习更困难；C 污染测试；D 低分本身不支持过拟合。

**得分点：** 检查后针对容量试验。**来源：** [G] 欠拟合。

### Q026 · Test leakage

**EN:** Why is repeatedly choosing the model with the highest test score a problem?

**中文：** 为什么反复挑测试分最高的模型有问题？

- A. It changes the tensor rank / 改变张量阶数
- B. Test sets cannot contain labels / 测试集不能有标签
- C. Model selection adapts to test data, making the estimate optimistic / 选择适应了测试集，估计偏乐观
- D. It prevents gradients on training data / 阻止训练集梯度

**English answer:** C. The test set becomes part of the tuning process.

**中文解析：** C 正确；A、D 无关；B 评估当然可有标签，关键是不据此调模型。

**得分点：** selection leakage。**来源：** [G] 综合题 5。

### Q027 · Baseline

**EN:** What should usually precede a very large model?

**中文：** 尝试巨大模型前通常应该做什么？

- A. Hide all validation results / 隐藏验证结果
- B. Define the task, metric, split and simple baseline / 确定任务、指标、划分与简单基线
- C. Choose the largest layer count available / 先选最多层
- D. Assume data quality is perfect / 假设数据完美

**English answer:** B. A baseline tests whether the data and workflow contain useful predictive signal.

**中文解析：** B 正确；A 无助评估；C 跳过可行性检查；D 忽略标签和数据问题。

**得分点：** task → evaluation → baseline。**来源：** [G]；[C6] Beating a baseline。

### Q028 · Dropout mechanism

**EN:** What does `Dropout(0.3)` do during ordinary training?

**中文：** `Dropout(0.3)` 在常规训练时做什么？

- A. Permanently removes 30% of neurons / 永久删掉 30% 神经元
- B. Deletes 30% of training labels / 删掉 30% 标签
- C. Freezes 30% of weights forever / 永久冻结 30% 权重
- D. Randomly zeroes activations with probability 0.3 / 以 0.3 概率随机将激活置零

**English answer:** D. Keras also rescales retained activations during training to preserve their expected value.

**中文解析：** D 正确；A、C 都错在永久结构变化；B 不修改标签。每批遮罩可变化，并非固定删除某组单元。

**得分点：** random activations；training only。**来源：** [A2] 单元格 10；[KD]。

### Q029 · Dropout at evaluation

**EN:** What normally happens to dropout in `model.evaluate()` and `model.predict()`?

**中文：** 常规 `evaluate()`、`predict()` 时 dropout 怎样？

- A. It is inactive / 关闭随机丢弃
- B. It doubles its rate / 比率翻倍
- C. It deletes the output layer / 删除输出层
- D. It updates validation labels / 修改验证标签

**English answer:** A. Standard evaluation and prediction use inference behaviour.

**中文解析：** A 正确；B、C、D 都不发生。显式 `training=True` 是不同情境，不属于本题默认调用。

**得分点：** training vs inference。**来源：** [A2] 单元格 14、18；[KD]。

### Q030 · L2

**EN:** What does L2 weight regularisation add to the objective?

**中文：** L2 权重正则化向目标函数加入什么？

- A. A reward for huge weights / 奖励大权重
- B. A rule that all weights must equal zero / 强制所有权重为零
- C. A penalty proportional to squared weight values / 与权重平方和成比例的惩罚
- D. Extra true labels / 额外真实标签

**English answer:** C. Large weights become more costly under the regularised objective.

**中文解析：** C 正确；A 方向相反；B L2 不强制全零；D 不增加数据。惩罚过强也会欠拟合。

**得分点：** squared-weight penalty。**来源：** [G]；[A3] 单元格 10 的方法表。

### Q031 · Too much regularisation

**EN:** After greatly increasing dropout, both training and validation scores become poor. What is plausible?

**中文：** 大幅提高 dropout 后，两边分数都差，合理解释是什么？

- A. Dropout always improves every model / dropout 必定改善
- B. Excessive regularisation is causing underfitting / 正则过强造成欠拟合
- C. The test set is now larger / 测试集变大
- D. Softmax no longer sums to one / softmax 不再归一化

**English answer:** B. Too much noise can make it difficult to learn useful patterns.

**中文解析：** B 合理；A 错在“必定”；C 数据划分未因此变化；D dropout 不改变 softmax 的定义。

**得分点：** regularisation trade-off。**来源：** [G] 欠拟合与缓解。

### Q032 · Early stopping monitor

**EN:** What does `EarlyStopping(monitor="val_loss", restore_best_weights=True)` select by?

**中文：** 该 early stopping 设置以什么选择最佳权重？

- A. Largest training batch / 最大 batch
- B. Highest training accuracy regardless of validation / 只看最高训练准确率
- C. Latest timestamp / 最新时间
- D. Best monitored validation loss / 最佳验证 loss

**English answer:** D. With the usual minimisation setting, it restores weights from the lowest monitored validation loss.

**中文解析：** D 正确；A 无关；B 指标错误；C 最后保存时间不表示表现最好。最低 val_loss 不一定是最高 val_accuracy。

**得分点：** monitor 决定 best。**来源：** [A3] 单元格 11；[KE]。

### Q033 · Checkpoints

**EN:** What is `ModelCheckpoint(save_best_only=True, monitor="val_loss")` mainly useful for?

**中文：** 该 checkpoint 的主要用途是什么？

- A. Keeping a saved model at the best monitored validation loss / 保存监控指标最优时的模型
- B. Guaranteeing no overfitting / 保证不过拟合
- C. Generating new labels / 生成标签
- D. Automatically changing the number of classes / 自动改类别数

**English answer:** A. It preserves a recoverable best checkpoint; it does not itself stop training.

**中文解析：** A 正确；B 不保证学习质量；C、D 不属回调功能。停止训练是 early stopping 的职责。

**得分点：** 保存与停止区分。**来源：** [G]；[C7] callbacks。

### Q034 · Learning-rate reduction

**EN:** A reduction is triggered at learning rate 0.001 with `factor=0.8`, above the floor. What is the new rate?

**中文：** 学习率 0.001、factor=0.8，已触发下降且未到下限，新值是多少？

- A. 0.2
- B. 0.0018
- C. 0.0008
- D. 0.0002

**English answer:** C. The callback multiplies the current rate by 0.8.

**中文解析：** C 正确；A 不是学习率乘积；B 加了数；D 错把“保留 80%”理解成“减少到 20%”。数值为条件练习，不声称 A3 实际触发。

**得分点：** multiplicative factor。**来源：** [A3] 单元格 13；[KR]。

### Q035 · Compile

**EN:** Which call configures the optimizer, loss and metrics?

**中文：** 哪个调用配置优化器、loss 和指标？

- A. `predict()`
- B. `compile()`
- C. `save()`
- D. `summary()`

**English answer:** B. Compilation configures how training and evaluation will work.

**中文解析：** B 正确；A 生成预测；C 保存；D 展示结构。compile 本身不执行完整训练。

**得分点：** configuration。**来源：** [A2] 单元格 10；[G]。

### Q036 · Fit

**EN:** Which call normally performs repeated training updates and returns a History object?

**中文：** 哪个调用进行反复训练更新并返回 History？

- A. `summary()`
- B. `load_model()`
- C. `predict()`
- D. `fit()`

**English answer:** D. The History records metrics across epochs.

**中文解析：** D 正确；A 只看结构；B 读取模型；C 只预测。History 里的数字不是保存的每轮权重。

**得分点：** training loop；History。**来源：** [A3] 单元格 7。

### Q037 · Evaluate versus predict

**EN:** Which statement is correct for the assignment classifiers?

**中文：** 对作业分类器，哪项正确？

- A. `evaluate(x,y)` reports loss/metrics; `predict(x)` returns outputs / 前者评估，后者输出预测
- B. Both always update weights / 都会更新权重
- C. `predict()` directly returns accuracy without labels / 无标签直接得准确率
- D. `evaluate()` always returns a confusion matrix / 总返回混淆矩阵

**English answer:** A. Accuracy requires comparing predictions with labels; prediction alone does not do that.

**中文解析：** A 正确；B 常规推理不更新；C 缺少真值；D 混淆矩阵需另外计算。

**得分点：** outputs vs scored outputs。**来源：** [A2] 单元格 14、18。

### Q038 · Model APIs

**EN:** Which API naturally handles a model with two inputs merged into one output?

**中文：** 两个输入合并成一个输出，哪种 API 更自然？

- A. Only a plain Sequential stack / 只能 Sequential
- B. A confusion matrix / 混淆矩阵
- C. The Functional API / Functional API
- D. `pad_sequences` alone / 只用 padding

**English answer:** C. The Functional API connects tensors in a graph and supports branching or merging.

**中文解析：** C 正确；A 适合单条堆叠路径；B 是评估工具；D 是预处理工具。

**得分点：** multi-input graph。**来源：** [G]；[C7] model APIs。

### Q039 · Saved model state

**EN:** After training without checkpoint restoration, what does `model.save(...)` save?

**中文：** 训练后没有恢复 checkpoint，`model.save(...)` 保存哪个状态？

- A. Automatically the highest validation-accuracy epoch / 自动最高验证准确率轮
- B. The model's current state / 当前模型状态
- C. Only the best number in History / 只有 History 最大数字
- D. Every previous epoch's weights / 所有历史轮权重

**English answer:** B. Saving does not search History to recover earlier weights.

**中文解析：** B 正确；A、D 需要事先保存对应权重；C 保存的是模型而非单一成绩。

**得分点：** current state ≠ automatically best epoch。**来源：** [A3] 单元格 16。

### Q040 · Model summary

**EN:** What can `model.summary()` help verify?

**中文：** `model.summary()` 能帮助检查什么？

- A. Guaranteed test accuracy / 保证的测试准确率
- B. Whether all labels are correct / 全部标签是否正确
- C. The best learning rate without experiments / 无实验确定最优学习率
- D. Layer outputs and parameter counts / 层输出与参数数量

**English answer:** D. It helps check architecture, but it does not establish predictive quality.

**中文解析：** D 正确；A 要评估；B 要数据检查；C 要试验。A3 还展示 optimizer params，不能把总数都当网络可训练权重。

**得分点：** architecture evidence。**来源：** [A3] 单元格 9。

### Q041 · Image inductive bias

**EN:** Why can convolution be more suitable than flattening images into a Dense stack?

**中文：** 为什么卷积往往比展平图片后接 Dense 更适合图像？

- A. It uses local spatial patterns and shared weights / 利用局部空间模式和共享权重
- B. It never has parameters / 完全没有参数
- C. It ignores all spatial information / 忽略所有空间信息
- D. It guarantees perfect translation invariance / 保证完全平移不变

**English answer:** A. The same learned detector can be applied at multiple image positions.

**中文解析：** A 正确；B 卷积核可训练；C 与卷积目的相反；D 权重共享不等于任何位移下最终预测必定相同。

**得分点：** locality；weight sharing。**来源：** [G] ConvNets。

### Q042 · Filters

**EN:** What does `filters=32` usually determine in a Conv2D layer?

**中文：** Conv2D 的 `filters=32` 通常决定什么？

- A. Batch size / batch 大小
- B. Image height / 图片高度
- C. Number of output channels / 输出通道数
- D. Number of target classes in all cases / 永远是目标类别数

**English answer:** C. Each filter produces one feature map.

**中文解析：** C 正确；A 是训练批次设置；B 由空间尺寸计算；D 中间层通道不等于分类标签数。

**得分点：** filters → feature maps。**来源：** [G] filters。

### Q043 · Valid padding

**EN:** What is the output of a 3×3 Conv2D with 32 filters, stride 1 and valid padding on a 64×64×3 image?

**中文：** 上述卷积作用于 64×64×3 图像，输出形状是什么？忽略 batch。

- A. `64×64×3`
- B. `62×62×32`
- C. `32×32×32`
- D. `64×64×32`

**English answer:** B. Each spatial dimension becomes `64-3+1=62`.

**中文解析：** B 正确；A 没更新通道；C 像是做了二倍下采样；D 对应 stride 1 的 same 情形。

**得分点：** spatial formula；output channels。**来源：** [G] 64×64×3 练习。

### Q044 · Same padding

**EN:** At stride 1, what does same padding preserve?

**中文：** stride 为 1 时，same padding 保持什么？

- A. All pixel values / 所有像素值
- B. The number of classes / 类别数
- C. All weights at zero / 权重全零
- D. Spatial height and width / 空间高和宽

**English answer:** D. Padding preserves spatial size, while channel count is set by the filters.

**中文解析：** D 正确；A 卷积改变特征值；B 与 padding 无关；C 不涉及初始化。

**得分点：** H/W 保持，通道可变。**来源：** [G] padding。

### Q045 · Max pooling

**EN:** What does 2×2 max pooling with stride 2 do to a 32×32×16 feature map?

**中文：** 2×2、stride 2 的最大池化作用于 32×32×16，输出什么？

- A. `16×16×16`
- B. `32×32×8`
- C. `16×16×32`
- D. `64×64×16`

**English answer:** A. It downsamples each spatial axis and keeps the channels.

**中文解析：** A 正确；B 错减通道；C 无故加通道；D 是放大而非池化。

**得分点：** spatial downsampling。**来源：** [G] pooling。

### Q046 · Receptive field

**EN:** Two consecutive 3×3, stride-1 convolutions have what theoretical receptive-field size for an interior output, with no dilation or pooling?

**中文：** 无 dilation、无池化的两个连续 3×3、stride 1 卷积，内部输出的理论感受野多大？

- A. 3×3
- B. 6×6
- C. 5×5
- D. 9×9

**English answer:** C. The second layer combines neighbouring first-layer features, extending the field by two pixels per dimension.

**中文解析：** C 正确；A 没计第二层；B 直接相加错误；D 直接相乘错误。这里问理论覆盖范围，不是各像素实际贡献强弱。

**得分点：** stacked neighbourhoods。**来源：** [G] receptive field。

### Q047 · Augmentation split

**EN:** Where should random label-preserving augmentation normally be applied in this course workflow?

**中文：** 课程常规流程中，随机且保持标签的增强应主要用于哪里？

- A. Only test data / 只测试集
- B. Training data / 训练集
- C. Validation labels / 验证标签
- D. All splits before separating related images / 先增强全部再划分相关图

**English answer:** B. Keep validation and test preprocessing deterministic for a stable comparison.

**中文解析：** B 正确；A 不改善训练样本；C 不是图像增强；D 可能让原图与近似副本跨集合，造成泄漏。固定 resize、归一化仍可用于所有集合。

**得分点：** training only；先划分。**来源：** [G] augmentation。

### Q048 · Small image datasets

**EN:** Why consider a pretrained image model with few labelled examples?

**中文：** 标注图片很少时，为什么考虑预训练模型？

- A. It needs no validation / 不需要验证
- B. It makes every domain identical / 所有领域变相同
- C. It removes all overfitting risk / 消除所有过拟合
- D. It supplies features learned from a larger dataset / 提供大数据上学到的特征

**English answer:** D. Reusing useful features can reduce the amount of task-specific learning required.

**中文解析：** D 正确；A、C 没有保证；B 领域差异仍需检查。

**得分点：** reusable features。**来源：** [G] pretrained models。

### Q049 · Feature extraction

**EN:** Which setup is feature extraction with a frozen base?

**中文：** 哪个设置是冻结基座的特征提取？

- A. Train the new head while keeping base weights fixed / 只训练新分类头，基座权重固定
- B. Randomise the base each epoch / 每轮随机重置基座
- C. Train all base weights from scratch / 从头训练全部基座
- D. Use test labels to train the head / 用测试标签训练头

**English answer:** A. The base supplies fixed learned representations for the new classifier.

**中文解析：** A 正确；B 丢失稳定特征；C 不是冻结；D 泄漏测试信息。

**得分点：** fixed base；trainable head。**来源：** [G] feature extraction。

### Q050 · Fine-tuning order

**EN:** Which order is safest for an ordinary small-data transfer-learning experiment?

**中文：** 小数据迁移学习试验，哪个顺序更合理？

- A. Unfreeze everything before creating a head / 加头前先全部解冻
- B. Tune using test accuracy first / 先用测试准确率调参
- C. Train a head with base frozen, then unfreeze selected layers and recompile / 先冻基座训头，再解冻部分层并重新 compile
- D. Delete pretrained weights after loading / 加载后删除预训练权重

**English answer:** C. A trained head provides a more useful learning signal before base features are adjusted.

**中文解析：** C 正确；A 随机头可能扰乱特征；B 评估污染；D 浪费预训练信息。微调还应采用较低学习率。

**得分点：** freeze → head → unfreeze → recompile。**来源：** [G]；[C8] fine-tuning。

### Q051 · Fine-tuning learning rate

**EN:** Why usually lower the learning rate during fine-tuning?

**中文：** 微调为什么通常降低学习率？

- A. To force all predictions to zero / 强制预测为零
- B. To limit disruption of pretrained features / 限制对预训练特征的破坏
- C. To eliminate the loss function / 不再需要 loss
- D. To increase image resolution / 增加分辨率

**English answer:** B. Smaller updates can adapt useful features more gently.

**中文解析：** B 正确；A 不相关；C 仍需优化目标；D 图像大小由预处理决定。

**得分点：** small feature adjustments。**来源：** [G] 微调学习率。

### Q052 · IMDB padding

**EN:** What is the main role of `pad_sequences(..., maxlen=500)` in A3?

**中文：** A3 中该操作主要做什么？

- A. Makes every review positive / 所有影评变正面
- B. Converts labels to one-hot / 标签变 one-hot
- C. Learns 500 classes / 学习 500 类
- D. Pads or truncates token sequences to length 500 / 把 token 序列补齐或截断到 500

**English answer:** D. Equal lengths allow reviews to be batched in a rectangular tensor.

**中文解析：** D 正确；A 不变情感标签；B 操作的是输入序列；C 500 是长度，不是类别数。

**得分点：** fixed sequence length。**来源：** [A3] 单元格 5。

### Q053 · Embedding shape

**EN:** In A3, what shape follows `Embedding(10000,32)` for inputs `(batch,500)`?

**中文：** A3 输入 `(batch,500)` 经该 Embedding 后是什么形状？

- A. `(batch,500,32)`
- B. `(batch,10000)`
- C. `(batch,32,500,1)`
- D. `(batch,1)`

**English answer:** A. Each token ID is replaced by a learned vector of length 32.

**中文解析：** A 正确；B 把词表大小误当输出；C 多了轴且轴顺序错误；D 是最终情感输出形状。

**得分点：** token → vector。**来源：** [A3] 单元格 7、9。

### Q054 · Mean pooling

**EN:** What is the main effect of A3's `GlobalAveragePooling1D()` on `(batch,500,32)`?

**中文：** 该层对 `(batch,500,32)` 的主要作用是什么？

- A. It outputs 500 sentiment labels / 输出 500 个情感标签
- B. It preserves every token position separately / 分别保留所有位置
- C. It averages across sequence positions to produce `(batch,32)` / 沿序列位置平均得到 `(batch,32)`
- D. It adds 500 trainable weights / 新增 500 个可训练权重

**English answer:** C. It summarises the sequence without trainable parameters and loses explicit word order.

**中文解析：** C 正确；A 混淆序列与标签；B 平均后不再分别保存位置；D 该池化层参数为 0。A3 未设置 mask，不能默认 padding 被忽略。

**得分点：** average over time；order limitation。**来源：** [A3] 单元格 7、9。

<a id="short-response"></a>
## B. Short Response | 简答题（54 题）

### Q055 · What makes learning “deep”?

**EN:** Explain what “deep” means and give one example of progressively useful features.

**中文：** 解释 deep 的含义，并举逐层有用的特征例子。

**English answer:** Deep refers to successive learned representations, not human-like understanding. For an image task, early features may describe edges, while later combinations may describe shapes useful for classification.

**中文解析：** deep 指多层表示学习，不是机器理解得像人一样“深”。例如识别图片时，先学边缘，再组合成形状；这是直观例子，不代表每层必定有可命名的固定职责。

**得分点：** 多层自动学习；具体例子；不等同人类理解。**来源：** [G] 分层表示；[C1]。

### Q056 · Why does A1 need a nonlinear hidden layer?

**EN:** Relate A1's two-moons data to the need for a nonlinear activation.

**中文：** 结合 A1 两个月牙的数据，解释非线性激活的必要性。

**English answer:** The classes have a curved boundary, so one straight decision boundary is limited. ReLU lets the hidden layer create a piecewise nonlinear mapping that can better separate the classes.

**中文解析：** 两个月牙互相弯绕，一条直线很难分好。隐藏层 ReLU 让多个线性部分组合成更灵活的边界；删掉隐藏非线性，即使增加线性层，最后仍受线性分界限制。

**得分点：** 数据边界；非线性表达。**来源：** [A1] 单元格 5、7、11。

### Q057 · Loss versus accuracy

**EN:** Why can loss change even when classification accuracy stays the same?

**中文：** 为什么准确率不变时 loss 仍能变化？

**English answer:** Accuracy counts correct class decisions, whereas cross-entropy also depends on probability confidence. Raising the true-class probability from 0.6 to 0.9 can reduce loss without changing the predicted class.

**中文解析：** accuracy 只看判对多少；交叉熵还看给真类多少概率。二分类真类为 1，0.6 和 0.9 都判对，但 0.9 的 loss 更低。自拟例子。

**得分点：** 决策 vs 概率；例子。**来源：** [A2] 单元格 12、20。

### Q058 · Forward, backward and optimizer

**EN:** Explain the separate roles of the forward pass, backward pass and optimizer.

**中文：** 分别解释前向、反向和优化器的职责。

**English answer:** The forward pass produces predictions and the loss. Backpropagation computes gradients of that loss with respect to parameters. The optimizer uses the gradients to update the parameters.

**中文解析：** 前向是“算出预测与误差”，反向是“算出各参数影响误差的方向和强度”，优化器是“据此改参数”。反向求梯度与实际改权重不是同一步。

**得分点：** 三个职责清晰。**来源：** [A1] 单元格 11、13。

### Q059 · Chain rule without proof

**EN:** Explain why the chain rule is needed for an early-layer weight.

**中文：** 为什么计算早期层权重的影响需要链式法则？

**English answer:** An early weight affects the loss through several intermediate operations. The chain rule combines local derivatives along this path, allowing backpropagation to assign a gradient to that weight.

**中文解析：** 第一层权重先影响隐藏激活，隐藏激活影响输出，输出才影响 loss。链式法则把这一连串影响连接起来。不要求展开微积分证明，但应理解为什么能从后往前传。

**得分点：** intermediate operations；local derivatives。**来源：** [G] chain rule；[A1] 单元格 11。

### Q060 · Epoch, batch and step

**EN:** Distinguish an epoch, a batch and an update step using A2's 50,000 examples and batch size 128.

**中文：** 用 A2 的 50,000 个训练样本和 batch size 128 区分 epoch、batch、step。

**English answer:** An epoch is one pass through the training examples. A batch contains up to 128 examples here, and each batch normally produces one update. With the final partial batch retained, one epoch has `ceil(50000/128)=391` updates.

**中文解析：** 一轮不是只改一次参数。A2 每轮分 391 批，最后一批有 80 个；A1 反而每轮把全部 X 一起送入，所以是 full-batch 更新。

**得分点：** 一遍数据；一批样本；一次更新；391。**来源：** [A2] 单元格 8、12；[A1] 单元格 13。

### Q061 · Sigmoid saturation in plain language

**EN:** Explain sigmoid saturation and why it matters in deep hidden layers.

**中文：** 用通俗语言解释 sigmoid 饱和及其对深层隐藏层的影响。

**English answer:** For very positive or negative inputs, sigmoid changes very little when its input changes. Its derivative is then small. Repeated small derivatives can weaken the gradient reaching early layers, making learning slow.

**中文解析：** 想象一条两端几乎平的曲线：输入改一点，输出几乎不动，这就叫饱和。反传时，小量逐层相乘会更小，例如自拟的 `0.1^5=0.00001`。ReLU 正数区斜率为 1，避免这一种缩小来源。

**得分点：** 平坦；小导数；多层相乘。**来源：** [G] ReLU vs sigmoid；[A1] 单元格 7。

### Q062 · Does ReLU solve every gradient problem?

**EN:** State one limitation of ReLU and explain why sigmoid can still be useful at a binary output.

**中文：** 说出 ReLU 的一个局限，并解释二分类输出仍能用 sigmoid 的原因。

**English answer:** ReLU has zero gradient for negative inputs, so persistently inactive units may stop learning. Sigmoid is still useful at a binary output because it maps a score to a probability between zero and one. Hidden-layer optimisation and output interpretation have different requirements.

**中文解析：** 某单元总落在负数区时，可能长期不更新，即常说的 dead ReLU。不能因此把二分类输出也全改成 ReLU：A1、A3 需要 0–1 概率，而 ReLU 正数无上限。

**得分点：** 负区零导数；隐藏与输出角色区分。**来源：** [A1] 单元格 7、11；[A3] 单元格 7。

### Q063 · Rank versus shape

**EN:** Explain tensor rank and shape using A1's `X.shape=(1000,2)`.

**中文：** 用 A1 的 X 解释 tensor rank 和 shape。

**English answer:** The shape `(1000,2)` gives the length of each axis: 1,000 samples and two features. Its tensor rank is two because it has two axes. This use of rank is different from matrix rank in linear algebra.

**中文解析：** shape 是每个轴多长；tensor rank 是有几个轴。`(1000,2)` 是二维张量，不是“1000 维特征”，也不是在线性代数里判断独立行列数量。

**得分点：** 轴数；样本轴；特征轴。**来源：** [A1] 单元格 5；[C2] tensors。

### Q064 · Random initialisation

**EN:** Why not initialise all hidden-layer weights to the same value?

**中文：** 为什么不把隐藏层权重全部初始化成同一个值？

**English answer:** Identically initialised hidden units can receive identical gradients and learn the same feature. Random initialisation breaks this symmetry. A suitable scale also helps keep activations and gradients manageable.

**中文解析：** 多个隐藏单元若起点完全相同，更新也可能相同，相当于重复做同一件事。A1 用随机值和按输入数缩放；He 常用于 ReLU。A1 对 W2 的改动只说明该次试验结果，不证明 He 对 sigmoid 输出总最好。

**得分点：** symmetry breaking；scale。**来源：** [A1] 单元格 9、16。

### Q065 · Learning-rate trade-off

**EN:** Contrast a learning rate that is too small with one that is too large.

**中文：** 对比学习率过小与过大。

**English answer:** A very small learning rate can make progress unnecessarily slow. A very large rate can overshoot and make loss unstable. Compare learning curves under controlled settings to choose a useful range.

**中文解析：** 步子太小走得慢，太大可能来回越过较好位置。单次低分不够诊断；应固定数据和结构，只改变学习率，看训练 loss 是否稳定下降。

**得分点：** slow vs unstable；controlled comparison。**来源：** [A1] 单元格 9、13；[G] 训练诊断。

### Q066 · What A1 actually demonstrates

**EN:** A1's recorded training loss falls from 0.6130 to 0.2890. What can and cannot be concluded?

**中文：** A1 记录的训练 loss 从 0.6130 降到 0.2890，能与不能说明什么？

**English answer:** The network improved its binary cross-entropy on the training examples. This supports that learning occurred under this objective. It does not establish generalisation because the shown loop has no held-out validation evaluation.

**中文解析：** 模型在做过的训练题上进步了，但还没证明新题也会。不能把 0.2890 当准确率，也不能单凭它断定无过拟合。

**得分点：** training improvement；held-out evidence missing。**来源：** [A1] 单元格 13 实际输出。

### Q067 · Verifying AI-assisted code

**EN:** Give one useful role and one risk of an AI coding assistant, plus a verification step from A1.

**中文：** 给出 AI 编程助手一项帮助、一项风险，以及结合 A1 的验证方法。

**English answer:** An assistant can help inspect shape errors or explain gradients. It can also suggest a plausible but incorrect implementation. I would check dimensions, rerun the notebook, and compare analytical gradients with element-wise numerical checks where needed.

**中文解析：** 有用之处是帮助定位和解释；风险是“讲得像对的”但代码或结论错。A1 反思提到逐元素梯度检查；该检查过程未保存在当前代码单元中，不能把反思中的误差数值当本次已验证结果。

**得分点：** help；risk；independent verification。**来源：** [G] A1；[A1] 单元格 16 的反思与当前代码对照。

### Q068 · One sigmoid output

**EN:** If IMDB label 1 means positive, how do you interpret a sigmoid output of 0.8?

**中文：** IMDB 标签 1 为正面时，sigmoid 输出 0.8 如何解释？

**English answer:** The model assigns an estimated probability of 0.8 to the positive class. With a threshold of 0.5, it predicts positive. This confidence is a model estimate, not a guarantee that 80% of all such predictions are correct.

**中文解析：** 这是模型对当前样本正面的概率估计，默认阈值 0.5 时判为正面。负面概率对应 0.2；不要把单个输出直接说成整个模型准确率 80%。自拟输出。

**得分点：** probability；threshold；not overall accuracy。**来源：** [A3] 单元格 7。

### Q069 · Softmax plus sparse loss, explained simply

**EN:** Explain the pairing using an image whose true digit is 7.

**中文：** 用真实数字为 7 的图片解释 softmax + sparse categorical cross-entropy。

**English answer:** The model gives ten probabilities, one for each digit. The label is the integer 7, so sparse categorical cross-entropy uses the probability at position 7 to measure error. Softmax is appropriate because the image has one correct class among ten.

**中文解析：** 模型提交十个概率，但老师的正确答案只写一个数字 `7`。loss 会找到第 7 类的概率，给得越少罚得越多。“多分类”指有十种可选答案；“单标签”指这一张只能选一个；“sparse”指直接接收整数答案。

**得分点：** ten probabilities；one true class；integer target。**来源：** [A2] 单元格 10、20。

### Q070 · Integer labels versus one-hot

**EN:** Compare integer and one-hot labels for three classes, and name the matching categorical losses.

**中文：** 比较三分类的整数与 one-hot 标签，并指出相应 loss。

**English answer:** Class 2 can be represented as integer `2` or one-hot `[0,0,1]`. Sparse categorical cross-entropy expects the integer form, while categorical cross-entropy expects the vector form. With equivalent targets and predictions, they express the same classification objective.

**中文解析：** 两种方式表达同一正确类别，不是两种任务。sparse 省去显式 one-hot 编码；不能因为类多就说一定 sparse，要看标签实际格式。

**得分点：** 对应例子；loss 匹配；目标等价。**来源：** [A2] 单元格 20。

### Q071 · Single-label versus multilabel

**EN:** Contrast classifying a digit with assigning several topic tags to an article.

**中文：** 对比数字分类和给文章加多个主题标签。

**English answer:** A digit image has one class, so its softmax probabilities compete across classes. An article may have several valid tags, so separate sigmoid outputs can represent several positive decisions. Binary cross-entropy can train those independent tag decisions.

**中文解析：** 数字识别是“十选一”；文章标签是“可多选”。softmax 总和为 1，会让类别竞争；多标签不要求所有 sigmoid 输出相加为 1。这里的“独立”指输出判定方式，不声称现实标签统计上毫无关联。

**得分点：** exclusive vs multiple；output/loss。**来源：** [G] 多标签问题。

### Q072 · Does softmax prove correctness?

**EN:** A wrong class receives probability 0.99. Has softmax failed mathematically?

**中文：** 错误类别获得 0.99 概率，softmax 数学上失效了吗？

**English answer:** No. Softmax can produce a valid probability distribution that is confidently wrong. The problem is predictive quality, which must be assessed using labels, loss and held-out evaluation.

**中文解析：** 加起来等于 1，只代表格式对，不代表答案对。模型很自信地错时，交叉熵可能很大；不要把“归一化”理解成“验证了正确性”。

**得分点：** valid distribution ≠ correct prediction。**来源：** [G] softmax；[A2] 单元格 20。

### Q073 · Adam versus a learning-rate callback

**EN:** How is Adam's adaptation different from ReduceLROnPlateau?

**中文：** Adam 自适应与 ReduceLROnPlateau 有什么不同？

**English answer:** Adam uses gradient statistics to scale parameter updates. ReduceLROnPlateau changes the optimizer's learning rate when a monitored metric stops improving. They can be used together because they operate on different information.

**中文解析：** Adam 看“各参数过去的梯度”，回调看“验证表现是否停滞”。所以 A3 使用 Adam 并不意味着回调已经触发，是否降过还要看学习率日志。

**得分点：** gradient history vs monitored performance。**来源：** [G] Adam；[A3] 单元格 7、13。

### Q074 · MNIST normalisation

**EN:** Why convert MNIST pixels to float32 and divide by 255?

**中文：** 为什么把 MNIST 像素转 float32 并除以 255？

**English answer:** Division by 255 scales pixel intensities from 0–255 to 0–1. Floating-point values support the numerical operations used in training. Consistent scaling makes optimisation easier to manage and must also be applied at evaluation.

**中文解析：** 改变数值尺度，不是增加图像信息，也不是把标签归一化。训练除以 255、测试不除，模型就收到不一致输入。float32 是常用精度，不表示所有模型只能用它。

**得分点：** range；float；consistent preprocessing。**来源：** [A2] 单元格 6。

### Q075 · Flattening images

**EN:** What does flattening 28×28 to 784 preserve and what does it stop representing explicitly?

**中文：** 28×28 展平成 784，保留什么，失去什么显式表达？

**English answer:** Flattening preserves the pixel values and their fixed ordering. It no longer presents them as a two-dimensional grid to this classifier. A Dense model can still learn, but it does not automatically use the locality and sharing built into convolutions.

**中文解析：** 不是把像素丢了，而是把“二维格子”变成“一长排”。A2 这样做是为了每图输入一个特征向量；Dense 也能处理高阶输入的最后一轴，所以“Dense 永远只能收一维”说得过头。

**得分点：** values retained；spatial structure not explicit。**来源：** [A2] 单元格 6、10；[G] ConvNets。

### Q076 · Reading confusion matrices

**EN:** Explain the diagonal, off-diagonal entries and A2's “worst pair.”

**中文：** 解释混淆矩阵对角、非对角与 A2 的 worst pair。

**English answer:** Diagonal entries count correct predictions, and off-diagonal entries count specific errors. A2 zeros the diagonal and finds the largest remaining entry. Its recorded result is true 4 predicted as 9, with 11 errors; this is directional rather than the sum of both directions.

**中文解析：** 对角是答对，不该参与“最多误判”搜索。A2 的 `4-9` 不是笼统说两个数最相似，更不等于 `4→9 + 9→4` 的总和最大。

**得分点：** 方向；对角排除；真实结果 11。**来源：** [A2] 单元格 18。

### Q077 · Underfitting with caveats

**EN:** Define underfitting and name one alternative explanation for poor scores on both splits.

**中文：** 定义欠拟合，并给出两边分数都差的另一种可能原因。

**English answer:** Underfitting means the model has not captured enough useful structure even in training data. Training and validation performance are both poor relative to a suitable baseline. Misaligned labels or failed optimisation can produce a similar pattern, so the curves alone do not identify the cause.

**中文解析：** 连训练题都没学好，可能容量小、训练不够或正则过强；也可能标签错位，扩大模型没有意义。先查数据与训练过程，再针对性调整。

**得分点：** poor training fit；baseline；alternative cause。**来源：** [G] 欠拟合与综合题 2。

### Q078 · Overfitting with evidence

**EN:** Define overfitting using two learning-curve observations.

**中文：** 用两个学习曲线现象定义过拟合。

**English answer:** Overfitting occurs when improved fitting of training data no longer improves generalisation. Training loss may continue falling while validation loss rises. A widening training–validation performance gap is another warning, assuming the splits and evaluation conditions are comparable.

**中文解析：** “练过的题越做越好，新题反而更差”是核心。训练准确率高、验证低也支持判断，但要排除分布不同或预处理不一致等原因。

**得分点：** loss divergence；gap；conditions。**来源：** [G] 过拟合。

### Q079 · Does validation train the model?

**EN:** Does passing `validation_data` to `fit()` normally update weights using validation examples?

**中文：** 向 `fit()` 传 validation_data，会用验证样本更新权重吗？

**English answer:** No, ordinary validation computes metrics without gradient updates on those examples. Its results can still influence stopping or hyperparameter choices. This indirect use is why a separate final test set remains useful.

**中文解析：** 验证集不参加常规反传，但你根据它选 dropout 或停止轮数，也是一种信息使用。因此验证分不是完全独立的最终证据。

**得分点：** no gradient；indirect selection；test needed。**来源：** [A2] 单元格 12；[G] validation。

### Q080 · Untouched test data

**EN:** Explain why tuning on a test set invalidates its role as a final independent estimate.

**中文：** 为什么在测试集上调参会破坏它作为最终独立估计的作用？

**English answer:** Repeated choices based on test results adapt the development process to that particular dataset. The selected score can therefore be overly optimistic. Use validation for choices and reserve a genuinely unused set for the final evaluation.

**中文解析：** 看一次答案后改模型，再考同一份卷，就不是完全没见过的考试。即使测试标签未参与梯度，也会通过你的选择影响模型。

**得分点：** selection is information use；optimism。**来源：** [G] 综合题 5。

### Q081 · Two overfitting mitigations

**EN:** Give two concrete overfitting mitigations and explain their mechanisms.

**中文：** 给出两种具体缓解过拟合的方法，并解释机制。

**English answer:** Dropout randomly removes some activation signals during training, discouraging reliance on a narrow set of features. Early stopping limits training once validation performance stops improving and can restore a better earlier model. Both should be chosen using validation results.

**中文解析：** 不能只写“加 regularisation”。应说 dropout 让模型不能总依赖固定少数单元；early stopping 防止继续只记训练细节。也可答减容量、L2、更多有代表性数据，并说明原因。

**得分点：** 两种方法；各自机制；验证选择。**来源：** [G]；[A3] 单元格 11。

### Q082 · Two underfitting mitigations

**EN:** Give two targeted actions for underfitting after basic data checks pass.

**中文：** 基本数据检查通过后，给出两种针对欠拟合的措施。

**English answer:** If the model is too small, increase its capacity moderately. If useful progress is still occurring, train longer; if regularisation is excessive, reduce it instead. Choose the action that matches the observed limitation and track both training and validation curves.

**中文解析：** 欠拟合不是固定“多训练”。loss 仍下降才有理由延长；已经卡住需考虑容量、学习率或特征；dropout 很大则先减弱它。

**得分点：** actions tied to evidence；两边都看。**来源：** [G] 欠拟合。

### Q083 · Universal workflow

**EN:** Outline the steps before increasing model complexity.

**中文：** 概述增加模型复杂度之前的步骤。

**English answer:** Define the prediction target and a meaningful success metric. Inspect data and labels, establish an appropriate split, and build a simple baseline. Confirm that the pipeline can learn useful signal before spending effort on a larger model.

**中文解析：** 先知道“预测什么、怎样算成功”，再确认数据正确、划分可靠、简单模型至少有用。没有这几个前提，大模型的高分可能只是数据泄漏。

**得分点：** task；metric；data；split；baseline。**来源：** [G] workflow；[C6]。

### Q084 · Accuracy and class balance

**EN:** In a binary dataset with 95% negative labels, why is 95% accuracy not automatically impressive?

**中文：** 负类占 95% 的二分类数据，95% 准确率为何不一定好？

**English answer:** Always predicting negative already reaches 95% accuracy. Compare with that baseline and inspect the confusion matrix, particularly missed positive examples. The metric should reflect which mistakes matter for the task.

**中文解析：** 只答“全是负面”也有高分，可能正类一个都没找出来。自拟数据提醒你：chance 或 baseline 不能无条件固定成 50%。

**得分点：** majority baseline；class-specific errors。**来源：** [G] workflow、confusion matrix；自拟扩展。

### Q085 · Explain compile without code

**EN:** Explain `compile()` to someone who confuses it with training.

**中文：** 向把 compile 当训练的人解释这个方法。

**English answer:** Compile sets the rules for training: the optimizer, objective and reported metrics. It does not itself run through the training dataset to learn weights. The actual repeated learning happens in fit.

**中文解析：** 像先规定“怎么改答案、用什么标准判错、记录什么分数”，并未开始刷题。不要把代码能成功 compile 等同模型已经学会。

**得分点：** configure；no dataset training。**来源：** [A2] 单元格 10；[G] compile。

### Q086 · Explain fit and History

**EN:** What does `fit()` do, and what is stored in History?

**中文：** fit 做什么，History 存什么？

**English answer:** Fit processes training batches and updates weights across epochs. It may also run validation and callbacks. History stores recorded metric values, not a full archive of weights from every epoch.

**中文解析：** A3 的 `max(hist_baseline.history["val_accuracy"])` 找到最好分数，不能把模型自动退回那轮。要保留对应模型，需 checkpoint 或最佳权重恢复。

**得分点：** updates；metrics history ≠ weight history。**来源：** [A3] 单元格 7。

### Q087 · Explain evaluate

**EN:** What does `evaluate(x_test,y_test)` return for the assignment models, and does it train?

**中文：** 作业模型 evaluate 返回什么，会训练吗？

**English answer:** With loss and accuracy configured, it returns their evaluated values on the supplied data. It normally uses inference behaviour and does not update model weights. Interpretation still depends on whether the test set was kept independent.

**中文解析：** A2 的 `test_loss, test_acc` 是两个成绩，不是新模型。名称叫 x_test 不保证它真的没被用来调参，要看实际流程。

**得分点：** loss/accuracy；no update；independence。**来源：** [A2] 单元格 12、14。

### Q088 · Explain predict

**EN:** Why does A2 apply argmax after predict, while A3 would use a threshold?

**中文：** 为什么 A2 在 predict 后用 argmax，而 A3 通常用阈值？

**English answer:** A2 outputs ten probabilities per image, so argmax selects the highest-probability digit. A3 outputs one positive-class probability, so a threshold such as 0.5 converts it to a binary decision. Neither operation alone computes accuracy without labels.

**中文解析：** 十个数里找最大位置，与一个数判断是否超过 0.5，是两种输出格式。不能对 `(batch,1)` 做 axis=1 argmax 来实现二分类，结果会全是 0。

**得分点：** output shape；decision rule。**来源：** [A2] 单元格 18；[A3] 单元格 7。

### Q089 · Save and reload

**EN:** Why save and reload a `.keras` model before final evaluation?

**中文：** 为什么保存 `.keras` 后还要重载再评估？

**English answer:** Reloading checks that the saved artifact can reconstruct the intended model for later use. Compare its predictions or metrics with the selected in-memory state under the same preprocessing. Saving alone does not ensure that the chosen state was the best epoch.

**中文解析：** 这是检查“交出去的文件是否真能用”，不是再训练。A3 确实保存后 load，再得到 test accuracy 0.8529；不能只看到文件存在就认为模型选择也正确。

**得分点：** usable artifact；same preprocessing；selection caveat。**来源：** [A3] 单元格 16。

### Q090 · Early stopping for a classmate

**EN:** Explain early stopping to a classmate who watches only training loss.

**中文：** 向只看训练 loss 的同学解释 early stopping。

**English answer:** Training loss can keep improving even after performance on new examples stops improving. Early stopping monitors a held-out metric and stops after a specified period without improvement. Restoring best weights keeps the state selected by that monitored metric.

**中文解析：** 训练题越熟不代表新题越好，所以要旁边放一套验证题。patience 是允许连续几轮没有足够改善，不是从开始算总共训练几轮。

**得分点：** validation；patience；restore best。**来源：** [G]；[A3] 单元格 11。

### Q091 · Checkpoint versus early stopping

**EN:** Why might a training run use both ModelCheckpoint and EarlyStopping?

**中文：** 为什么一次训练可能同时用 checkpoint 和 early stopping？

**English answer:** A checkpoint saves a model state to disk when its saving condition is met. Early stopping decides when to end training. Using both can preserve a recoverable model while avoiding unnecessary later epochs; align their monitors when they should select the same state.

**中文解析：** 一个负责“存档”，一个负责“停止”。若一个看 val_loss、另一个看 val_accuracy，选出的 best 可能不是同一轮，应事先明确标准。

**得分点：** persistence vs stopping；monitor alignment。**来源：** [G]；[C7] callbacks。

### Q092 · Plateau scheduling

**EN:** Explain factor, patience and min_lr in A3's learning-rate callback.

**中文：** 解释 A3 学习率回调的 factor、patience、min_lr。

**English answer:** The callback watches validation loss and waits for lack of sufficient improvement. Factor 0.8 multiplies the current rate by 0.8 when a reduction is triggered, patience 1 controls that wait, and min_lr `1e-6` sets a floor. Registering the callback does not prove that a reduction occurred.

**中文解析：** 不是每轮固定减，也不是每次减去 0.8。只有满足停滞条件才动作；本次 A3 的 val_loss 一直改善，日志没有实际下降。

**得分点：** conditional action；三个参数。**来源：** [A3] 单元格 13；[KR]。

### Q093 · Why training accuracy may fall with dropout

**EN:** Why can dropout lower training accuracy while improving held-out performance?

**中文：** 为什么 dropout 可能降低训练准确率却改善新数据表现？

**English answer:** Dropout makes training harder by randomly removing activation signals. This can discourage brittle dependence on particular units. At evaluation, dropout is inactive, and the learned representation may generalise better even if the training score was lower.

**中文解析：** 练习时随机少一些信息，训练分可能低，但学到的规则可能更稳。不能只看训练分选择模型，也不能保证 dropout 必定提高验证分。

**得分点：** noisy training；reduced dependence；held-out selection。**来源：** [A2] 单元格 12、14；[G]。

### Q094 · L2 versus dropout

**EN:** Compare what L2 and dropout change during learning.

**中文：** 对比 L2 与 dropout 在学习中改变什么。

**English answer:** L2 adds a cost for large weights to the objective. Dropout randomly zeroes activation signals during training. Both can reduce overfitting, but excessive strength can prevent adequate training fit.

**中文解析：** L2 是让“大权重更贵”，dropout 是让“部分激活暂时不可用”。两者都不是自动清洗错误标签。A3 代码实际 USE_L2=False，不能声称这次已测试 L2。

**得分点：** penalty vs masking；trade-off；实际开关。**来源：** [A3] 单元格 10–11；[G]。

### Q095 · Sequential or Functional?

**EN:** Choose an API for the assignment stacks and for a two-input model, with reasons.

**中文：** 为作业顺序堆叠模型和双输入模型选择 API 并解释。

**English answer:** Sequential is concise for the assignments' single path of layers. The Functional API is more suitable when tensors branch, merge, or support multiple inputs and outputs. The API choice describes connectivity, not an automatic improvement in accuracy.

**中文解析：** A2、A3 是一层接一层，Sequential 清楚够用；两路输入合并需要图式连接。换 Functional 不会凭空增加模型学习能力。

**得分点：** topology determines choice。**来源：** [G]；[A3] 单元格 1、7。

### Q096 · Explain A3's parameter summary

**EN:** A3 reports 321,089 trainable parameters and 963,269 total parameters after training. Why should you distinguish them?

**中文：** A3 summary 两个数字不同，为什么必须区分？

**English answer:** The network's learned layer parameters total 321,089. This summary also counts 642,180 optimizer-state parameters, giving 963,269 in the displayed total. Optimizer bookkeeping is not extra feature-producing neurons or layers.

**中文解析：** `320000 + 1056 + 33 = 321089` 是网络层参数；训练后 Adam 的状态也列进 summary 总数。问“网络有多少可训练参数”时答 321,089，而不是把优化器状态也算进去。

**得分点：** trainable vs optimizer state；正确加总。**来源：** [A3] 单元格 9 实际 summary。

### Q097 · ConvNet versus Dense

**EN:** Explain two image-related advantages of convolutions over an ordinary flattened Dense model.

**中文：** 解释卷积相比普通展平 Dense 的两个图像优势。

**English answer:** Convolutions examine local neighbourhoods, which matches the spatial organisation of images. Sharing the same filter across positions reuses a detector and usually reduces parameters compared with full dense connectivity at comparable widths. This helps data efficiency but does not guarantee a better result for every task.

**中文解析：** 看邻近像素有助识别局部形状；同一检测器可在各位置使用，不必每个位置单独学一套。优势来自结构假设，不是“卷积模型永远小、永远好”。

**得分点：** locality；sharing；qualified claim。**来源：** [G] ConvNets。

### Q098 · Kernel size versus filter count

**EN:** Distinguish kernel size from number of filters in Conv2D.

**中文：** 区分 Conv2D 的 kernel size 和 filters 数量。

**English answer:** Kernel size specifies the spatial neighbourhood used for each local computation. The number of filters specifies how many different output feature maps are produced. For RGB input, a standard convolutional filter spans all input channels as well as its spatial window.

**中文解析：** `3×3` 说每次看多大一块；`32 filters` 说学多少种检测器。RGB 的单个普通卷积核实际还跨 3 个输入通道，不是每个只看一种颜色。

**得分点：** spatial window；output channels；input depth。**来源：** [G] kernel/filter。

### Q099 · Pooling and information

**EN:** Why use max pooling, and what is the trade-off?

**中文：** 为什么用最大池化，代价是什么？

**English answer:** Max pooling downsamples a feature map by keeping the strongest activation in each window. This reduces later computation and can make the representation less sensitive to small shifts. It also discards precise spatial information, which can hurt tasks needing exact locations.

**中文解析：** 每小块只留最大响应，图变小、后续运算变少；但不知道最大响应在块内哪个位置。池化不学习像 Dense 那样的权重，也不是无损压缩。

**得分点：** downsample；local shift tolerance；information loss。**来源：** [G] max pooling。

### Q100 · Padding and shape

**EN:** Compare valid and same padding at stride 1 using a 64×64 input and 3×3 kernel.

**中文：** 用该输入与核大小比较 valid 和 same，stride 为 1。

**English answer:** Valid adds no padding, so the spatial output is 62×62. Same adds boundary padding to retain 64×64 at stride 1. The output channel count still depends on the number of filters.

**中文解析：** valid 要整个窗口落在图内，所以边缘可放的位置少；same 在边界补值。不要把 same 说成“所有维度和所有数值都保持不变”。

**得分点：** no pad vs pad；62/64；channels separate。**来源：** [G] padding、形状练习。

### Q101 · Growing receptive fields

**EN:** Explain how stacking local convolutions lets a later feature depend on a larger input region.

**中文：** 为什么堆叠局部卷积会让后层特征依赖更大输入范围？

**English answer:** Each first-layer feature already summarises an input neighbourhood. A later convolution combines neighbouring features, so it indirectly covers the union of their input regions. For two stride-1 3×3 layers without dilation, an interior output has a 5×5 theoretical receptive field.

**中文解析：** 第二层看的不是原始单像素，而是“已经看过一小块的特征”，合并后范围扩大。不要把层数和窗口大小直接相乘。

**得分点：** indirect coverage；5×5 example。**来源：** [G] receptive field。

### Q102 · Safe augmentation

**EN:** Explain label-preserving augmentation and why split order matters.

**中文：** 解释保持标签的数据增强，以及先后划分顺序的重要性。

**English answer:** An augmentation should change appearance without changing the correct target. Separate original examples into splits first, then randomly augment only training examples in the normal workflow. Otherwise related versions of one image may leak into validation and inflate performance.

**中文解析：** 例如任务允许时的小幅平移；不能盲目把数字 6 旋成 9 后仍贴 6 标签。先划分原图，再增强训练集，验证和测试保留稳定处理。

**得分点：** label preservation；split before augmentation。**来源：** [G] augmentation；自拟反例。

### Q103 · Pretraining limitations

**EN:** Why can pretraining help with limited labels, and when might it help less?

**中文：** 为什么预训练能帮助少标签任务，什么情况下帮助可能有限？

**English answer:** It starts from features learned on a larger dataset rather than learning all features from scratch. This can reduce the task-specific data requirement. Benefits may be smaller when the new images differ greatly from the pretraining domain, so validation remains necessary.

**中文解析：** 相当于已有一些视觉基础，再学新分类规则。若来源图像与新任务差异很大，已有特征未必合适；不能只凭“pretrained”这个词保证效果。

**得分点：** reuse；limited data；domain mismatch。**来源：** [G] pretrained models。

### Q104 · Feature extraction versus fine-tuning

**EN:** What changes in the base network in feature extraction versus fine-tuning?

**中文：** 特征提取与微调时，基座网络有什么不同？

**English answer:** In feature extraction, the base weights remain fixed while the new head learns. In fine-tuning, selected base weights also update to adapt the representation. Fine-tuning can improve fit to the new domain but increases the need to control overfitting.

**中文解析：** 冻结阶段只是用已有特征；微调允许特征本身稍微变化。不是只有全模型解冻才叫微调，解冻后面部分层也算。

**得分点：** fixed vs updating base；trade-off。**来源：** [G] feature extraction/fine-tuning。

### Q105 · Train the new head first

**EN:** Why train a randomly initialised head while the pretrained base is frozen?

**中文：** 为什么随机初始化的新头应先在冻结基座时训练？

**English answer:** An untrained head initially produces an unreliable error signal for the new task. Keeping the base frozen prevents those early updates from disrupting useful pretrained features. Once the head is useful, controlled fine-tuning can begin.

**中文解析：** 新头还不会分类时，不宜让它的大幅错误信号立即改变整套已有视觉特征。先让头适应，再温和调整基座。

**得分点：** random head；protect base；ordered training。**来源：** [G] frozen base before fine-tuning。

### Q106 · Recompile after unfreezing

**EN:** After unfreezing selected base layers, what training configuration should be reconsidered?

**中文：** 解冻部分基座层后，应重新考虑哪些训练设置？

**English answer:** Recompile so the intended trainable variables are included in the training setup. Use a lower learning rate to limit changes to pretrained features. Monitor validation performance and keep a checkpoint rather than assuming every extra epoch helps.

**中文解析：** trainable 改完后再 compile，明确哪些参数参加更新。微调学习率通常更低；监控验证与保存最佳状态仍然需要。

**得分点：** trainable set；recompile；low LR；validation。**来源：** [G] 微调；[C8] fine-tuning 代码。

### Q107 · Describe A3 accurately

**EN:** State A3's baseline architecture, reported metric and observed regularised result.

**中文：** 说明 A3 基线结构、报告指标及正则模型的真实结果。

**English answer:** The baseline uses Embedding, GlobalAveragePooling1D, Dense(32, ReLU) and one sigmoid output. Its best validation accuracy was 0.8525. The dropout-plus-early-stopping configuration reached 0.8546, a small observed increase, but this run does not isolate the causal contribution of each technique.

**中文解析：** 报的是各轮最高 val_accuracy，不是训练分或测试分。A3 用了 dropout 0.3 和 early stopping，没有实际用 L2；提高 0.0021，即 0.21 个百分点，不能把小幅单次差异说成已证明稳定改善。

**得分点：** architecture；best val metric；accurate techniques；caution。**来源：** [A3] 单元格 7、11 实际输出。

### Q108 · Did A3's callbacks act?

**EN:** What do A3's saved logs actually show about early stopping and learning-rate reduction?

**中文：** A3 保存的日志对提前停止和降低学习率实际说明什么？

**English answer:** The regularised run completed all ten requested epochs while validation loss kept improving, so it did not stop early. The scheduled run logged learning rate 0.0010 throughout and improving validation loss, so no reduction is evidenced. Its higher validation accuracy cannot therefore be credited to an observed learning-rate reduction.

**中文解析：** “回调已配置”是真的；“它已触发并改善结果”并不成立。A3 反思写了缩小步长带来改善，但当前日志不支持；应据日志解释，而不是背那句话。

**得分点：** configured vs triggered；actual evidence；no causal overclaim。**来源：** [A3] 单元格 11、13 日志，对照 14、18 反思。

<a id="code-reading"></a>
## C. Code Reading | 代码解读题（24 题）

代码是用于解释的摘录，不是需要复制运行的独立程序。下列摘录保留真实表达式，可省略无关行和注释；只有明确写“改编”的片段才是假设修改。A1、A2、A3 各 8 题。

### Q109 · A1: Input and target dimensions

**EN:** What shapes do X and y have, and why are targets kept as a column?

**中文：** X、y 形状是什么，为什么标签保留列向量？

**代码摘录：** [A1] 单元格 5。

```python
X, y = make_moons(1000)
print("X shape:", X.shape, "y shape:", y.shape)
```

**English answer:** X is `(1000,2)` and y is `(1000,1)`, matching 1,000 two-feature inputs and one target per sample. The network output also has shape `(1000,1)`. Flattening y to `(1000,)` can make subtraction broadcast to an unintended `(1000,1000)` array.

**中文解析：** 这里 y 列形状是为了与 A2 输出逐样本相减，而不是“所有分类标签必须二维”。本题 A2 指 A1 网络变量 A2，不是 Assignment 2。`(1000,1)-(1000,)` 会发生错误的广播，代码可能不报错但逻辑错。

**得分点：** 两个形状；输出对齐；广播风险。**来源：** [A1] 单元格 5、11。

### Q110 · A1: Parameters and biases

**EN:** Find the shapes of W1, b1, W2 and b2, and calculate the parameter count.

**中文：** 求四组参数形状，并计算总参数数。

**代码摘录：** [A1] 单元格 9，省略注释。

```python
input_size, hidden_size, output_size = 2, 16, 1
W1 = np.random.randn(input_size, hidden_size).astype(np.float32) * np.sqrt(2 / input_size)
b1 = np.zeros((1, hidden_size), dtype=np.float32)
W2 = np.random.randn(hidden_size, output_size).astype(np.float32) * np.sqrt(2 / hidden_size)
b2 = np.zeros((1, output_size), dtype=np.float32)
```

**English answer:** The shapes are `(2,16)`, `(1,16)`, `(16,1)` and `(1,1)`. There are `32+16+16+1=65` trainable parameters. Biases are shared across samples rather than independently stored for each example.

**中文解析：** 每个输出单元有一个 bias，通过广播加到整批样本；1000 个样本不会让参数数乘 1000。W2 当前也采用同形式缩放，这是代码事实，不是对所有输出层的通用最优建议。

**得分点：** 四个形状；65；bias sharing。**来源：** [A1] 单元格 9。

### Q111 · A1: Trace the forward pass

**EN:** Trace the shapes for a batch of m examples and explain `@` versus activation.

**中文：** 追踪 m 个样本的形状，并解释 @ 与激活的区别。

**代码摘录：** [A1] 单元格 11，函数内部。

```python
Z1 = X @ W1 + b1
A1 = relu(Z1)
Z2 = A1 @ W2 + b2
A2 = sigmoid(Z2)
```

**English answer:** Z1 and A1 are `(m,16)`, while Z2 and A2 are `(m,1)`. The `@` operations combine input features through matrix multiplication. ReLU adds hidden nonlinearity, and sigmoid turns the final score into a binary probability.

**中文解析：** `@` 不是逐元素乘法：`(m,2)@(2,16)=(m,16)`。激活逐元素处理，不改变这些形状。即使 X、y 标签命名相似，A1/A2 此处都是激活变量。

**得分点：** shape trace；matmul；activation roles。**来源：** [A1] 单元格 11。

### Q112 · A1: Output gradient

**EN:** Why divide by m, and what shapes do dW2 and db2 have?

**中文：** 为什么除以 m，dW2、db2 形状是什么？

**代码摘录：** [A1] 单元格 11，backward 内部。

```python
m = X.shape[0]
dZ2 = (A2 - y) / m
dW2 = A1.T @ dZ2
db2 = np.sum(dZ2, axis=0, keepdims=True)
```

**English answer:** Division by m matches a mean binary cross-entropy objective. The resulting dW2 is `(16,1)` and db2 is `(1,1)`, matching their parameters. The familiar `(A2-y)` simplification applies to the standard sigmoid-plus-BCE objective.

**中文解析：** 每样本误差取平均，避免 batch 变大时只因求和就放大梯度。`A1.T` 为 `(16,m)`。严格说当前 compute_loss 在 log 内加 eps，显示的损失与标准 BCE 有微小差别，这个梯度不是该 eps 改写式的严格精确导数；作为通常概率区间下的教学近似应知道边界。

**得分点：** mean；matching dimensions；pairing assumption。**来源：** [A1] 单元格 11。

### Q113 · A1: Backpropagating through ReLU

**EN:** Explain the transpose and the ReLU mask. What is the resulting dW1 shape?

**中文：** 解释转置与 ReLU mask，dW1 形状是什么？

**代码摘录：** [A1] 单元格 11。

```python
dA1 = dZ2 @ W2.T
dZ1 = dA1 * relu_derivative(cache["Z1"])
dW1 = X.T @ dZ1
```

**English answer:** Multiplication by W2.T sends the output gradient back to 16 hidden units. The element-wise ReLU derivative blocks it where Z1 is negative or zero under this implementation. dW1 has shape `(2,16)`.

**中文解析：** `dA1`、`dZ1` 都为 `(m,16)`；中间 `*` 是逐元素乘，不是 @。A1 采用 `x>0`，因此在 0 处也选导数 0；ReLU 在 0 不可微，代码选了一个约定。

**得分点：** reverse dimensions；mask；dW1 shape。**来源：** [A1] 单元格 7、11。

### Q114 · A1: Numerical protection

**EN:** What numerical problem is eps intended to reduce? Is this an accuracy formula?

**中文：** eps 试图减少什么数值问题？这是不是准确率公式？

**代码摘录：** [A1] 单元格 11，compute_loss 内部。

```python
eps = 1e-8
return -np.mean(y * np.log(A2 + eps) + (1 - y) * np.log(1 - A2 + eps))
```

**English answer:** Eps helps avoid taking the logarithm of zero at extreme probabilities. This computes a numerically modified binary cross-entropy, not the fraction of correct predictions. For production code, a stable library loss is preferable to assuming this guard handles every numerical issue.

**中文解析：** 概率接近 0 时 log 会很负，恰为 0 则可能出现无穷；eps 是保护尝试，不是提高准确率的超参数。A1 sigmoid 还用了 clip，但 float32 极端指数仍需留意，不能声称所有溢出都已解决。

**得分点：** log(0)；BCE not accuracy；numerical limitation。**来源：** [A1] 单元格 7、11。

### Q115 · A1: Full-batch loop and last loss

**EN:** Is this mini-batch training? Does `loss_history[-1]` evaluate the weights after the very last update?

**中文：** 这是 mini-batch 训练吗？最后一个记录 loss 是否评估最后更新后的权重？

**代码摘录：** [A1] 单元格 13，保留完整循环逻辑，省略打印。

```python
for epoch in range(epochs):
    cache = forward(X, W1, b1, W2, b2)
    loss = compute_loss(y, cache["A2"])
    loss_history.append(loss)
    dW1, db1, dW2, db2 = backward(X, y, cache, W1, W2)
    W1 -= learning_rate * dW1
    b1 -= learning_rate * db1
    W2 -= learning_rate * dW2
    b2 -= learning_rate * db2
final_loss = loss_history[-1]
```

**English answer:** It is full-batch training because each update uses all of X. Loss is recorded before the update in each iteration, so the final recorded value is before the last update. A new forward pass would be needed to measure the final weights exactly.

**中文解析：** 这是读执行顺序题，不是说已记录的 0.2890 没用。它仍是训练改善证据，但不是严格意义上最后更新后的再评估值。

**得分点：** full-batch；record before update；recompute。**来源：** [A1] 单元格 13。

### Q116 · A1: Seed and repeated experiments

**EN:** What does the seed help with, and why is one run insufficient to prove an initialization change is generally better?

**中文：** seed 有何作用，为什么单次试验不足以证明某初始化普遍更好？

**代码摘录：** [A1] 单元格 3，省略注释。

```python
np.random.seed(504)
```

**English answer:** The seed makes NumPy's pseudorandom sequence repeatable under the same execution order. It helps reproduce the generated data and initial weights. A general comparison still needs controlled settings and repeated seeds because one initialisation can be unusually favourable.

**中文解析：** 同一个 seed 后反复单独运行随机单元，随机状态仍会前进，不是每次自动同一结果。反思提到初始化改善，只能描述该次条件，不能推出所有网络都如此。

**得分点：** repeatability；execution order；multiple runs。**来源：** [A1] 单元格 3、9、16。

### Q117 · A2: Preprocessing trace

**EN:** What is the resulting training input shape before the validation split, and what does -1 mean?

**中文：** 验证划分前训练输入形状是什么，-1 代表什么？

**代码摘录：** [A2] 单元格 6，省略测试集对应行。

```python
x_train = x_train.astype("float32") / 255.0
x_train_flat = x_train.reshape(-1, 28 * 28)
```

**English answer:** The result is `(60000,784)` in the saved MNIST run. The -1 asks NumPy to infer the sample dimension from the number of elements. Reshaping keeps the data values, while division changes their scale.

**中文解析：** `60000*28*28` 个像素重新排成 60000 行，每行 784；-1 不是“删除一维”。训练和测试必须一致处理。

**得分点：** 60000×784；inferred dimension；scale vs shape。**来源：** [A2] 单元格 6 代码与输出。

### Q118 · A2: Split roles

**EN:** State the split sizes and explain why validation slicing precedes training on the remainder.

**中文：** 说出划分大小，并解释保留验证部分的作用。

**代码摘录：** [A2] 单元格 8。

```python
val_samples = 10000
x_val = x_train_flat[:val_samples]
y_val = y_train[:val_samples]
x_train_flat = x_train_flat[val_samples:]
y_train = y_train[val_samples:]
```

**English answer:** The run has 50,000 training, 10,000 validation and 10,000 separate test examples. The held-out examples support model choices without training-gradient updates. The slicing assumes the first 10,000 form a suitable validation sample; ordered or grouped data would require more careful splitting.

**中文解析：** x、y 必须同样切片，不能只切输入。此写法对当前作业清楚，但不是所有数据都能无条件取前 10000；若前半全为某类，验证就失真。

**得分点：** sizes；matching x/y；representativeness。**来源：** [A2] 单元格 8。

### Q119 · A2: Architecture count

**EN:** Calculate trainable parameters in the no-dropout architecture.

**中文：** 计算无 dropout 模型的可训练参数数。

**改编摘录：** 从 [A2] 单元格 10 的 builder 展开成无 dropout 的静态层清单；宽度与真实代码一致。

```python
model = keras.Sequential([
    layers.Input(shape=(784,)),
    layers.Dense(512, activation="relu"),
    layers.Dense(256, activation="relu"),
    layers.Dense(10, activation="softmax"),
])
```

**English answer:** The layers contain `(784+1)*512=401920`, `(512+1)*256=131328`, and `(256+1)*10=2570` parameters. The total is 535,818. Input does not add trainable parameters.

**中文解析：** Dense 每层是“输入数×输出数 + 输出偏置”。不要把 batch 大小算进去，也不要把激活函数算成额外的一组权重。

**得分点：** 三层公式；535818。**来源：** [A2] 单元格 10；按代码计算，非 notebook 打印值。

### Q120 · A2: Conditional dropout

**EN:** How many Dropout layers are added when use_dropout is true, and does the parameter count increase?

**中文：** 开关为 true 时加几个 Dropout 层，参数数增加吗？

**代码摘录：** [A2] 单元格 10，省略前面创建模型及最后输出层。

```python
if use_dropout:
    model.add(layers.Dropout(dropout_rate))
model.add(layers.Dense(256, activation="relu"))
if use_dropout:
    model.add(layers.Dropout(dropout_rate))
```

**English answer:** Two Dropout layers are used, one after each hidden Dense layer. They add no trainable parameters and keep the activation shapes unchanged. They change training behaviour rather than the widths of the Dense layers.

**中文解析：** 看似增加“两层”，但不是新增两层带权重的神经元。A2 的两个模型 Dense 宽度相同，dropout 训练时随机遮挡，推理时关闭。

**得分点：** two；zero parameters；same shapes。**来源：** [A2] 单元格 10、14。

### Q121 · A2: Fit arguments

**EN:** Explain each training argument and estimate the number of updates if all 20 epochs finish.

**中文：** 解释训练参数，并估算完成 20 轮的更新次数。

**代码摘录：** [A2] 单元格 12，省略 verbose 参数。

```python
EPOCHS = 20
BATCH_SIZE = 128
history_baseline = model_baseline.fit(
    x_train_flat, y_train,
    epochs=EPOCHS,
    batch_size=BATCH_SIZE,
    validation_data=(x_val, y_val),
)
```

**English answer:** Epochs sets 20 passes over training data, and batch_size sets up to 128 examples per update. Validation is evaluated separately without training updates. With 50,000 training examples and partial batches retained, there are `391*20=7820` updates.

**中文解析：** val 不追加到训练样本数里；不能用 60000/128 算本次 training steps，因为 10000 已拿去验证。此调用无提前停止回调。

**得分点：** arguments；391 per epoch；7820。**来源：** [A2] 单元格 8、12。

### Q122 · A2: Curves versus test scores

**EN:** What is compared by this call, and what do the recorded test scores show?

**中文：** 该调用比较什么？已记录测试分说明什么？

**代码摘录：** [A2] 单元格 16。

```python
plot_histories([history_baseline, history_dropout], ["baseline", "dropout"])
```

**English answer:** It compares training and validation loss and accuracy for both runs across epochs. The recorded final test accuracies are 0.9799 without dropout and 0.9845 with dropout, an increase of 0.46 percentage points. That is an observed run comparison, not proof of an improvement for every seed.

**中文解析：** 曲线里的 val_accuracy 与这里另报的 test_accuracy 不同。可说本次 dropout 测试更好，但开发下一轮应依据验证集，不反复用测试分优化。

**得分点：** four curve types；correct numbers；0.46 percentage points。**来源：** [A2] 单元格 12、14、16 实际输出。

### Q123 · A2: Why axis=1?

**EN:** What shape does predict return for 10,000 test images, and why use axis=1?

**中文：** 10000 张测试图的 predict 形状是什么，为什么 axis=1？

**代码摘录：** [A2] 单元格 18。

```python
y_pred = np.argmax(model_dropout.predict(x_test_flat, verbose=0), axis=1)
```

**English answer:** Predict returns `(10000,10)` probabilities. Axis 1 selects the largest class probability within each image's row, producing `(10000,)` integer predictions. Axis 0 would instead select image indices for each class and would not produce one label per image.

**中文解析：** 行是图片，列是数字类别。每行挑最大列位置才是这张图的预测数字，不是把全数组只找一个最大值。

**得分点：** input/output shapes；axis interpretation。**来源：** [A2] 单元格 18。

### Q124 · A2: Worst directional error

**EN:** Explain why copy, fill_diagonal and unravel_index are used.

**中文：** 分别解释 copy、fill_diagonal、unravel_index 的作用。

**代码摘录：** [A2] 单元格 18，省略注释。

```python
cm_off = cm.copy()
np.fill_diagonal(cm_off, 0)
true_i, pred_j = np.unravel_index(np.argmax(cm_off), cm_off.shape)
```

**English answer:** Copy preserves the original matrix, while zeroing the diagonal removes correct predictions from the search. Argmax finds the largest error count, and unravel_index converts its flat position into row and column. If all off-diagonal counts were zero, the returned position would not represent an actual error pair.

**中文解析：** 正确预测通常最多，不清对角会选到“答对最多的类”。全无错误时 argmax 仍会返回某位置，应先判断最大误判计数是否大于 0；有并列时只返回其中一个。

**得分点：** preserve；exclude correct；coordinates；zero-error caveat。**来源：** [A2] 单元格 18。

### Q125 · A3: Vocabulary, length and split

**EN:** Distinguish 10,000 vocabulary size, 500 sequence length and 10,000 validation examples.

**中文：** 区分词表 10000、长度 500、验证样本 10000。

**代码摘录：** [A3] 单元格 5，省略标签与测试处理行。

```python
max_features = 10000
max_len = 500
(x_train, y_train), (x_test, y_test) = keras.datasets.imdb.load_data(num_words=max_features)
x_train = keras.utils.pad_sequences(x_train,maxlen = max_len)
x_val = x_train[:10000]
x_train = x_train[10000:]
```

**English answer:** Max_features limits the token vocabulary, max_len controls tokens per padded review, and slicing selects the number of validation reviews. The saved shapes are train `(15000,500)`, validation `(10000,500)` and test `(25000,500)`. None of those counts changes the two sentiment classes.

**中文解析：** 三个数字属于不同维度：词的编号范围、每条影评多长、影评有多少条。当前默认 padding/truncating 在前端补或截；不是全部短评都真有 500 个词。

**得分点：** vocabulary/length/examples；three shapes；binary target。**来源：** [A3] 单元格 5；[KP] 默认值核对。

### Q126 · A3: Embedding and masking

**EN:** Trace the output shapes and state whether this code explicitly excludes padded zeros from averaging.

**中文：** 追踪输出形状，并判断代码是否明确排除 padding 的 0。

**代码摘录：** [A3] 单元格 7 的相邻层。

```python
layers.Embedding(max_features, 32, input_length=max_len),
layers.GlobalAveragePooling1D(),
layers.Dense(32, activation="relu"),
layers.Dense(1, activation="sigmoid"),
```

**English answer:** The shapes are `(batch,500,32)`, `(batch,32)`, `(batch,32)` and `(batch,1)`. The Embedding call does not set mask_zero=True, so padded IDs are not automatically excluded by an embedding mask here. Padding length can therefore affect the mean representation.

**中文解析：** 编号 0 也可查到一个向量，并不自动是全零向量。平均池化本身不会凭空知道哪些位置是补齐。`input_length` 是原作业写法，当前 Keras 中并非必需；这里读其功能，不要求照抄为新代码。

**得分点：** four shapes；mask not configured；padding impact。**来源：** [A3] 单元格 7、9；[KM]。

### Q127 · A3: Maximum metric versus model state

**EN:** Does computing this maximum recover the corresponding model weights?

**中文：** 计算这个最大值，会恢复对应模型权重吗？

**代码摘录：** [A3] 单元格 7。

```python
baseline_val_acc = max(hist_baseline.history["val_accuracy"])
```

**English answer:** No. It extracts only the largest recorded validation-accuracy value. The model retains its current weights unless a checkpoint is loaded or best weights are restored. In this saved baseline run, the maximum happens at epoch 10, but that coincidence is not guaranteed in later runs.

**中文解析：** 成绩单上的最高分不是当时脑中的全部状态。若最好在第 6 轮、第 10 轮更差，max 不会自动回退。

**得分点：** scalar vs weights；observed epoch vs general rule。**来源：** [A3] 单元格 7 实际记录。

### Q128 · A3: Does the L2 flag implement L2?

**EN:** If only USE_L2 is changed to True, does this shown Dense layer gain L2 regularisation?

**中文：** 若只把 USE_L2 改为 True，下面 Dense 会真的加 L2 吗？

**代码摘录：** [A3] 单元格 11，省略中间无关代码；if 块只构建方法名称列表。

```python
USE_L2 = False
USE_DROPOUT = True
USE_EARLY_STOP = True
layers.Dense(32, activation="relu"),
if USE_L2:
    techniques.append("L2")
```

**English answer:** No. The displayed Dense layer has no kernel_regularizer, and changing a reporting flag does not add a penalty. L2 would need to be attached to the intended layer when constructing the model. The current run uses dropout and early stopping, not L2.

**中文解析：** 这是省略式代码摘录，不能直接独立运行。开关若没有接入层构造，只会让报告列表写上 L2，形成“文字说用了、代码没用”。本题不修改原作业。

**得分点：** flag vs implementation；kernel_regularizer；actual methods。**来源：** [A3] 单元格 11。

### Q129 · A3: EarlyStopping trace

**EN:** For hypothetical val_loss values `0.50,0.40,0.42,0.43`, when does this stop and which epoch is restored? Assume default min_delta and no baseline.

**中文：** 自拟 val_loss 如上，默认 min_delta、无 baseline，何时停，恢复哪轮？

**代码摘录：** [A3] 单元格 11，原 if 块内部，省略注释。

```python
cb.append(callbacks.EarlyStopping(
    monitor="val_loss",
    patience=2,
    restore_best_weights=True
))
```

**English answer:** Epoch 2 is best at 0.40. Epochs 3 and 4 are two consecutive non-improvements, so training stops at the end of epoch 4 and restores epoch 2 weights. These values are a hypothetical trace, not A3's actual output.

**中文解析：** patience=2 不表示只训两轮；先有最佳，再累计两轮没改善。真实 A3 完成了 10 轮且 val_loss 持续下降。

**得分点：** epoch 4 stop；epoch 2 restore；hypothetical distinction。**来源：** [A3] 单元格 11；[KE]。

### Q130 · A3: A fresh model with a callback

**EN:** Does this continue baseline training or build a fresh model? Did the saved run actually reduce its learning rate?

**中文：** 这是续训 baseline 还是新建？保存的运行有没有真的降学习率？

**代码摘录：** [A3] 单元格 13，省略注释。

```python
lr_model = build_baseline()
lr_cb = callbacks.ReduceLROnPlateau(
   monitor="val_loss", factor=0.8, patience=1, min_lr=1e-6, verbose=1
)
```

**English answer:** Calling build_baseline creates a fresh model, not a continuation of baseline's trained weights. The saved log stays at 0.0010 for all ten epochs, with improving validation loss. Thus the run tests a configured callback but does not demonstrate an actual reduction.

**中文解析：** 函数名字相同不等于同一个对象或同一初始权重。全局 seed 只在开头设置一次，后建模型会消耗后续随机数；0.8578 不能直接归因为学习率下降。

**得分点：** fresh model；actual LR log；confounded comparison。**来源：** [A3] 单元格 3、7、13。

### Q131 · A3: Save, reload, evaluate

**EN:** Explain this sequence and what the saved test accuracy does and does not verify.

**中文：** 解释该顺序，以及保存后的测试分能和不能验证什么。

**代码摘录：** [A3] 单元格 16，省略注释及打印。

```python
MODEL_FILE = "imdb_sentiment_best.keras"
best_model = lr_model
best_model.save(MODEL_FILE)
loaded = keras.models.load_model(MODEL_FILE)
_, test_acc = loaded.evaluate(x_test, y_test, verbose=0)
```

**English answer:** It selects the current lr_model, saves it, reconstructs it and evaluates it on test data. The recorded test accuracy is 0.8529. This shows the saved artifact was loadable and evaluable in that run; it does not itself prove equivalence to every earlier checkpoint or that scheduling caused its performance.

**中文解析：** 变量名 best_model 是人的选择，不是 Keras 自动搜索最佳。当前日志最高 val_accuracy 在最后一轮，恰好一致；未来运行不保证。要检查保存前后完全一致，可比较同批预测。

**得分点：** sequence；0.8529；current-state caveat。**来源：** [A3] 单元格 13、16。

### Q132 · A3: Architecture evidence

**EN:** What can the summary and diagram confirm, and how many layer parameters should this architecture have?

**中文：** summary 与图能确认什么，该结构的层参数应有多少？

**代码摘录：** [A3] 单元格 9，plot_model 位于原代码的条件与 try 块中。

```python
baseline.summary()
plot_model(
    baseline,
    to_file=DIAGRAM_FILE,
    show_shapes=True,
    show_layer_names=True,
)
```

**English answer:** They confirm the forward path, layer shapes and parameter counts, not predictive success. Embedding contributes 320,000, the hidden Dense contributes 1,056, and the output contributes 33, totalling 321,089 trainable layer parameters. Pooling contributes zero; optimizer state is a separate category.

**中文解析：** 架构图是说明“信息怎样流动”，曲线和指标是说明“学得如何”，两种证据不能互相代替。图能画出来不证明训练有效。

**得分点：** architecture vs behaviour；321089；optimizer distinction。**来源：** [A3] 单元格 9 实际输出。

<a id="case-analysis"></a>
## D. Case Analysis | 案例分析题（18 题）

下列题型练习较长回答。五步为组织思路的方式，不是老师规定格式。可以连成一个英文段落；不能只有诊断名称而没有证据与措施。

### Q133 · Falling training loss, rising validation loss

**EN:** Diagnose this sustained pattern from a sound, representative split. Give two mitigations, explain why, and say how you would check them.

**中文：** 划分可靠且有代表性，根据以下持续趋势诊断，给出两种措施、原因和验证方法。

**自拟数据：**

| Epoch | Train loss | Val loss | Train accuracy | Val accuracy |
|---|---:|---:|---:|---:|
| 2 | 0.40 | 0.43 | 0.82 | 0.80 |
| 6 | 0.18 | 0.35 | 0.94 | 0.86 |
| 10 | 0.07 | 0.48 | 0.98 | 0.83 |
| 14 | 0.02 | 0.65 | 0.998 | 0.80 |

**English answer:** Diagnosis: the later epochs show overfitting. Evidence: training keeps improving while validation loss worsens after epoch 6. Mitigations: use early stopping with a best-validation checkpoint and try moderate dropout. These limit harmful late training and discourage reliance on narrow features. Verify with the same validation split, compare complete curves, and use the test set only after selecting the configuration.

**中文解析：** 判断是后期过拟合；证据不只是训练分高，而是两边趋势背离。先保存验证较好状态，再试 dropout，不是继续一味加轮数。表中第 6 轮是列出的最低 val_loss；没有所有轮数据，不能断定它是全部轮数中绝对最佳。

**得分点：** 诊断；两条趋势；两种合理措施及原因；验证与测试分工。**来源：** [G] 综合题 1；[A2] 单元格 16 的分析方式。

### Q134 · Both splits near chance

**EN:** A balanced ten-class task stays near 10% train and validation accuracy. Inputs and labels have not been audited. Is “make the network bigger” sufficient? Propose an ordered investigation.

**中文：** 平衡十分类两边约 10%，尚未检查数据。只增大模型够吗？给出有顺序的排查。

**English answer:** Diagnosis: the model is not learning useful signal, but capacity underfitting is not yet established. Evidence: both scores are near the balanced-task chance level. First check label alignment, input scale, output size and loss, then test whether a small subset can be fitted. If the pipeline works, try more capacity or adjust learning rate and training duration. Verify improved training fit and held-out performance before accepting the change.

**中文解析：** 先判“没学到有效规律”，不要直接判“模型太小”。10 个类别都均衡才有这个约 10% 基线。能否拟合很小子集是调试检查，不是最终泛化证据；连小子集都学不了时更应查实现。

**得分点：** evidence insufficient for root cause；ordered checks；targeted changes；held-out verification。**来源：** [G] 综合题 2；[A2] 单元格 6、10。

### Q135 · Excessive dropout

**EN:** On the same balanced task, dropout 0.3 gives train/val accuracy 0.91/0.89. After changing only the dropout rate to 0.9, both stay near 0.60 across repeated controlled runs. Diagnose and propose a remedy.

**中文：** 同任务仅把 dropout 从 0.3 改为 0.9，重复对照运行后两边都降至约 0.60。诊断并改进。

**English answer:** Diagnosis: excessive regularisation is a likely cause of underfitting. Evidence: the large dropout increase consistently reduces both scores. Reduce the rate and compare moderate values rather than adding more dropout. Preserving more activation signals makes useful patterns easier to learn. Verify with controlled runs and choose the rate by validation performance, not by training accuracy alone.

**中文解析：** 这里比“只看两边都低”多了受控改动和重复结果，所以更支持正则过强。不能因为“dropout 抗过拟合”就越大越好；恢复适度强度，再看验证。

**得分点：** too much regularisation；controlled evidence；reduce rate；validate。**来源：** [G] 欠拟合；[A2] 单元格 10。

### Q136 · Unstable optimisation

**EN:** After increasing learning rate from 0.001 to 1.0, loss jumps between large values and eventually becomes NaN. What would you do before adding layers?

**中文：** 学习率从 0.001 增至 1.0 后，loss 剧烈跳动并出现 NaN。加层前应做什么？

**English answer:** Diagnosis: unstable optimisation is more immediately plausible than a simple capacity problem. Evidence: failure began after a large learning-rate increase. Restart from a clean initial state with a smaller rate and inspect input scaling and non-finite values. Smaller updates can avoid overshooting, while data checks address alternative numerical causes. Verify finite, improving losses and held-out metrics under controlled settings.

**中文解析：** NaN 不是“分数低一点”，继续训练坏掉的权重可能无效。需重新初始化或恢复有效状态再试小学习率；也要查输入中的 NaN/Inf，不把所有数值问题都归因学习率。

**得分点：** optimisation vs capacity；restart；data checks；finite-loss validation。**来源：** [A1] 单元格 9、11、13；[G] 训练行为。

### Q137 · Validation accuracy exceeds training accuracy

**EN:** With dropout enabled, fit reports train accuracy 0.84 and validation accuracy 0.87. A teammate says this proves leakage. Evaluate the claim and propose checks.

**中文：** dropout 训练分 0.84、验证分 0.87，同学说必然泄漏。评价并提出检查。

**English answer:** Diagnosis: the gap alone does not prove leakage. Evidence: training uses dropout and records performance while weights are changing, whereas validation normally uses inference behaviour after the epoch. Evaluate training data in inference mode and inspect split overlap and representativeness. These checks separate evaluation-condition effects from contamination. Confirm whether the gap remains under comparable conditions before changing the architecture.

**中文解析：** 练习时有随机遮挡，验证时没有，分数可以倒过来；训练分还通常是整轮过程的汇总。仍应排查重复样本，但不能直接下结论。此时“加 dropout 修复”没有明确依据。

**得分点：** no automatic diagnosis；dropout mode；comparable evaluation；leakage audit。**来源：** [A3] 单元格 11；[G] train/val interpretation。

### Q138 · Wrong multiclass output

**EN:** A MNIST classifier has integer labels 0–9 but ends in `Dense(1, activation="softmax")`. It uses sparse categorical cross-entropy. Identify the error and propose the correct setup.

**中文：** MNIST 整数标签 0–9，却用一个 softmax 输出配 sparse loss，哪里错，如何改？

**English answer:** Diagnosis: the output dimension does not match the ten-class task. A one-unit softmax always returns one, and targets 1–9 are outside its one-class range, typically causing a loss error. Use ten softmax outputs with integer targets and sparse categorical cross-entropy. This supplies one probability per class. Verify output shape `(batch,10)`, valid label IDs and a decreasing training objective before held-out evaluation.

**中文解析：** 这不是先判断过拟合，而是模型搭配错误。一个 softmax 只会算自己除以自己，得到 1。改为 10 个输出；若标签改 one-hot，则对应 categorical loss。

**得分点：** one-softmax degeneracy；out-of-range labels；10 outputs；checks。**来源：** [A2] 单元格 10、20；[G] 综合题 3。

### Q139 · Test-driven tuning

**EN:** A student tries 40 dropout settings, selects the highest test accuracy and reports that test score as an unbiased final result. Explain the problem and redesign the evaluation.

**中文：** 尝试 40 种 dropout，用最高测试分选模型并称无偏最终结果。指出问题并重设评估。

**English answer:** Diagnosis: test-driven model selection has contaminated the final estimate. Evidence: the test score determined which configuration won. Use a validation set for model choices and evaluate the fixed final model on new, unused data if available. This separates tuning information from final assessment. If no untouched data remains, report the limitation rather than relabelling the reused test set as independent.

**中文解析：** 测试标签没反传也可能泄漏，因为人的选择已受到它影响。不能把原测试集换名字就洗掉历史使用；需新独立评估或如实说明限制。

**得分点：** selection leakage；redesign；honest limitation。**来源：** [G] 综合题 5。

### Q140 · High accuracy, useless minority-class behaviour

**EN:** A binary classifier achieves 95% accuracy on a dataset containing 950 negatives and 50 positives. Its confusion matrix shows every prediction is negative. Diagnose its usefulness and propose next steps.

**中文：** 950 个负类、50 个正类，模型全部预测负类却有 95% 准确率。评价并改进。

**English answer:** Diagnosis: the model matches the majority baseline and fails to identify positives; the accuracy is misleading for that goal. Evidence: it misses all 50 positive examples. Inspect labels, class-specific errors and a suitable positive-class metric, then consider balanced sampling or class weighting if appropriate. These changes give minority errors more attention. Select settings on validation data and verify that improved positive detection does not create unacceptable false positives.

**中文解析：** 不能只说“95% 很好”或立刻说过拟合，因为没有训练对照。核心是目标与指标不匹配。正类召回率为 0；改进后还要看误报代价，不能只追求更多报正。

**得分点：** baseline；50 missed；appropriate metric；trade-off。**来源：** [G] workflow、confusion matrix；自拟评估扩展。

### Q141 · Few labelled images

**EN:** You have 600 labelled photos for three exclusive classes. Outline a transfer-learning workflow, including output/loss, freezing, fine-tuning and final evaluation.

**中文：** 600 张标注照片、三个互斥类，规划迁移学习流程，包括输出/loss、冻结、微调与评估。

**English answer:** Diagnosis: limited labels make training a large model from scratch risky, so transfer learning is a reasonable starting point. Establish representative train, validation and test splits, and augment training images with valid transformations. Use a suitable pretrained base with matching preprocessing, freeze it and train a three-output softmax head with sparse categorical cross-entropy for integer labels. If validation supports further adaptation, unfreeze selected layers, recompile and use a low learning rate. Save the best validation-selected state and evaluate it once on the held-out test set.

**中文解析：** 判断是“小数据下的建模风险”，不是未训练就宣判已过拟合。新头先学，基座再微调；三个类用三输出。理由是复用特征、减少从零学习量；验证改善才接受微调，测试最后用。

**得分点：** split/augment；3-class pairing；freeze then tune；low LR/recompile；checkpoint/test。**来源：** [G] 综合题 4；[C8] 迁移学习流程。

### Q142 · Augmentation leaks between splits

**EN:** A student makes five transformed copies of every image, then randomly splits all copies into train and validation. Validation accuracy is 99%, but genuinely new photos score 72%. Diagnose and repair the workflow.

**中文：** 每图先生成五个增强副本，再随机划分，验证 99%、真正新图 72%。诊断并修复。

**English answer:** Diagnosis: related-image leakage is a strong concern, although domain shift could also contribute. Evidence: copies of the same original can appear in both splits. Group by original image, split those groups first and generate training augmentation only within the training split. This prevents near-duplicate examples from inflating validation performance. Retrain and assess on clean, representative held-out originals, also checking whether the new photos differ in domain.

**中文解析：** 不是见到 99/72 就只加 dropout；先修划分。增强副本与原图必须同组，避免模型在验证中看到近似熟题。新图差异也需要查，不能把所有差距都确定归因泄漏。

**得分点：** grouped split；train-only augmentation；alternative cause；clean reassessment。**来源：** [G] augmentation、evaluation。

### Q143 · Interpret A2's real results

**EN:** Use the recorded outputs below to discuss dropout's effect. Is a smaller training score a reason to reject it? What further evidence would strengthen the conclusion?

**中文：** 根据真实输出解释 dropout 效果。训练分更低是否应淘汰？还需要哪些证据？

**实际输出，已四舍五入：**

| Model | Epoch-20 train accuracy | Epoch-20 val loss | Final test accuracy |
|---|---:|---:|---:|
| Baseline | 0.9983 | 0.1109 | 0.9799 |
| Dropout | 0.9923 | 0.0734 | 0.9845 |

**English answer:** Diagnosis: these runs are consistent with dropout improving generalisation despite lower training accuracy. Evidence: the dropout model has lower final validation loss and a 0.46-percentage-point higher test accuracy. Keep model choices validation-based and inspect the full curves, rather than rejecting it for a lower training score. Dropout makes training harder while discouraging brittle feature dependence. Repeated controlled runs and validation-selected checkpoints would strengthen the conclusion; this single comparison does not prove a universal gain.

**中文解析：** 训练 99.23% 比 99.83% 低，不等于模型更差。关键是验证与新数据效果。这里引用已经存在的测试结果作解释，不建议再反复据测试调参；也不把单次差异当因果定论。

**得分点：** evidence-based interpretation；0.46 pp；dropout mechanism；repeatability。**来源：** [A2] 单元格 12、14 实际日志。

### Q144 · Challenge A3's scheduler claim

**EN:** A3 reports baseline best validation accuracy 0.8525, regularised 0.8546 and scheduled-run 0.8578. The scheduled log stays at learning rate 0.0010 throughout. Evaluate “reducing the learning rate caused the improvement” and design a better test.

**中文：** 三组真实验证结果如上，但调度组学习率始终 0.0010。评价“降学习率导致提升”，并设计更好试验。

**English answer:** Diagnosis: the causal claim is unsupported by the saved evidence. The scheduled run scored highest, but no reduction occurred while validation loss kept improving. Compare fresh runs with matched initial weights, data order and training settings, changing only the callback and repeating the comparison across seeds. Log the actual learning rate and ensure a plateau occurs before assessing the effect of a reduction. Use validation results for the comparison and do not repeatedly retune against the saved test score.

**中文解析：** 可以说“配置调度回调的这次运行最高”，不能说“已经证明调度降步长有效”。改善可能来自随机初始状态等因素；新试验要观察回调确实动作，再讨论动作前后与对照差异。若始终不触发，应报告“本条件下没检验到降学习率作用”。

**得分点：** observed ranking vs causality；no trigger；matched comparison；logged action。**来源：** [A3] 单元格 7、11、13 日志，对照 14、18 反思。

### Q145 · Best loss, best accuracy, last epoch

**EN:** An experiment records the values below. Which epoch does a val_loss checkpoint select? Which maximises val_accuracy? What does a plain save after epoch 8 retain if no weights were restored?

**中文：** 下表中 val_loss checkpoint、最高 val_accuracy、未恢复权重的最后保存各对应哪轮？说明怎样避免选错。

**自拟数据；假设表中列出全部候选最佳，epoch 8 为训练结束：**

| Epoch | Val loss | Val accuracy |
|---|---:|---:|
| 4 | 0.30 | 0.88 |
| 6 | 0.32 | 0.90 |
| 8 | 0.40 | 0.89 |

**English answer:** Diagnosis: “best” is ambiguous unless the criterion is defined. The best loss is epoch 4, best accuracy is epoch 6, and a plain final save retains epoch 8 here. Choose the selection metric in advance, use a matching checkpoint and reload it before evaluation. This ties the saved weights to the reported criterion rather than to a variable name. Verify the loaded state's metrics and record its selected epoch.

**中文解析：** 三种 best 不能混用。先决定主要指标，再同步 checkpoint 的 monitor/mode。History 的最大值、最后模型与最低 loss 模型可能全不同；这不是 Keras 出错，而是选择规则不同。

**得分点：** 4/6/8；criterion；checkpoint reload；verification。**来源：** [G] save/checkpoint；[A3] 单元格 7、16。

### Q146 · Distribution mismatch or overfitting?

**EN:** A model scores 98% on training scans and 70% on validation phone photos. No validation scans or training phone photos are available. Can you conclude ordinary overfitting? What would you investigate?

**中文：** 训练扫描图 98%、验证手机照片 70%，没有同来源交叉对照。能直接认定普通过拟合吗？应查什么？

**English answer:** Diagnosis: there is a generalisation gap, but the design confounds overfitting with source mismatch. Evidence: the split changes both seen status and image source. Inspect preprocessing, resolution, lighting and labels, and collect a representative split that permits within-source evaluation. Adapt the training data or augmentation to the intended deployment domain when justified. Verify gains on independent examples from that domain before attributing the original gap to model capacity alone.

**中文解析：** 一边是扫描、一边是手机，相当于同时改了“是否见过”和“题目风格”，原因混在一起。减模型可能有帮助，但不能替代收集代表性数据或统一处理。

**得分点：** confounding；domain checks；representative data；no premature label。**来源：** [G] generalisation、ML workflow；自拟案例。

### Q147 · ConvNet design and parameter explosion

**EN:** A proposed RGB model uses input 64×64×3, Conv2D(32,3,valid,stride 1), MaxPooling2D(2,stride 2), Flatten and Dense(64). Trace shapes and explain why the Dense layer may be risky with few images. Propose an alternative to test.

**中文：** 追踪上述网络形状，说明少图时 Dense 层的风险，并提出可测试替代方案。

**English answer:** Diagnosis: the design has a large Dense parameter burden, though overfitting requires observed evidence. Shapes are 62×62×32, 31×31×32, 30,752 and 64. Dense alone has `(30752+1)*64=1,968,192` parameters. Try additional sensible downsampling or global average pooling to reduce the representation size, recognising that spatial detail may be lost. Compare learning curves and validation performance to check whether reduced capacity improves generalisation.

**中文解析：** 卷积后 62，池化后 31，展平为 30752；Dense 接近 197 万参数。参数多是风险，不等于已经过拟合。若改 GlobalAveragePooling2D，32 通道变 32 维，Dense 参数变 `(32+1)*64=2112`，但空间表达也变了，仍需比较。

**得分点：** shapes；1968192；risk not proof；alternative/trade-off；validation。**来源：** [G] 64×64×3 草图练习、pooling；按规则自拟结构与计算。

### Q148 · A1 generalisation claim

**EN:** A student points to A1's decreasing training loss and says the model will classify new points well. Evaluate the evidence and design the missing check without treating training loss as accuracy.

**中文：** 同学凭 A1 训练 loss 下降就说新点也能分好。评价证据，设计缺少的检查。

**English answer:** Diagnosis: training improvement is supported, but generalisation is untested in the shown loop. The recorded loss decreases from 0.6130 to 0.2890 on the same training data. Reserve independently generated representative examples for validation and final testing, and use the correct probability threshold when computing accuracy. This checks unseen-point behaviour rather than memorisation of the original sample. Compare held-out loss and accuracy after fixing the selected settings.

**中文解析：** 0.2890 是误差目标，不是 28.90% 准确率。新点要独立生成且噪声等条件有代表性；验证用于选设置，测试最后做。这里缺证据，不直接判模型已过拟合。

**得分点：** supported/unsupported separation；independent data；loss vs accuracy；selection discipline。**来源：** [A1] 单元格 5、13；[G] A1 falling loss。

### Q149 · Directional versus combined confusion

**EN:** A confusion matrix has error counts `cm[4,9]=11`, `cm[9,4]=2`, `cm[3,5]=8`, `cm[5,3]=7`, with all other off-diagonal entries zero. What would A2's code report? Which unordered pair has the largest combined confusion? Suggest a targeted investigation.

**中文：** 自拟矩阵误判计数如上，其余非对角为 0。A2 代码报哪组？双向合计最大是哪组？提出针对性调查。

**English answer:** Diagnosis: two different definitions of worst confusion give different answers. A2's directional maximum reports 4→9 with 11 errors, while the combined pair 3/5 has 15 errors versus 13 for 4/9. Inspect the misclassified images and check class sample counts and labels. Target any recurring data-quality or visual-pattern issue rather than changing all model settings at once. Verify that the chosen intervention reduces the intended validation errors without harming other classes.

**中文解析：** 11 是单格最大，8+7=15 是双向最大，不矛盾。还应看该类有多少样本：大计数不必等于大错误率。题中只有 11 与 A2 实际相同，其余数字均为自拟。

**得分点：** directional 4→9；combined 3/5；counts vs rates；targeted checking。**来源：** [A2] 单元格 18 的算法；自拟矩阵。

### Q150 · Small-image overfitting: a complete response

**EN:** A cats-versus-dogs model trained from scratch reaches 100% training accuracy but 74% validation accuracy. Validation loss worsens during later epochs. A teammate proposes only doubling epochs. Give a coherent alternative using augmentation, regularisation and evaluation.

**中文：** 从头训练猫狗分类，训练 100%、验证 74%，后期验证 loss 恶化。同学只想把轮数翻倍。用增强、正则与评估提出完整替代方案。

**English answer:** Diagnosis: assuming a clean representative split, the trend supports overfitting. Perfect training fit alongside worsening validation loss suggests that extra epochs alone may amplify the problem. First retain a best-validation checkpoint, then test label-preserving training augmentation and moderate dropout; a smaller model or pretrained feature extractor is another reasonable experiment. Augmentation broadens training variation, while dropout discourages reliance on fragile features. Change settings in controlled comparisons, inspect both curves, select on validation and evaluate the saved selected model on untouched test data.

**中文解析：** 不是笼统说“加正则”，而是把措施接到问题：增强增加合理变化、dropout 降低固定特征依赖、early stopping/checkpoint 避免后期恶化。不要同时改一堆后声称知道哪项有效；分类搭配仍是 sigmoid+BCE，不需因过拟合换成十类 softmax。

**得分点：** diagnosis/evidence；reject unsupported extra epochs；mechanisms；controlled comparison；saved-model test。**来源：** [G] 综合题 8；[A3] 正则与保存流程。

<a id="coverage"></a>
## 大纲考点覆盖表

以下将原始大纲各提问逐项映射到本题库。编号 G01–G64 是本题库为核对添加的索引，不是老师的考试题号；与已有双语提纲的 64 项顺序相对应。每行给出代表题，不表示该考点只在这些题出现。

| 索引 | 原大纲考点 | 本题库代表题 |
|---|---|---|
| G01 | Hierarchical representations | Q001、Q055 |
| G02 | Nonlinear activations | Q002、Q056 |
| G03 | What loss measures | Q005、Q057、Q114 |
| G04 | Forward vs backward | Q006、Q058、Q111 |
| G05 | Chain rule | Q059、Q112、Q113 |
| G06 | Mini-batches and trade-offs | Q007、Q060、Q121 |
| G07 | ReLU vs sigmoid in deep stacks | Q004、Q061、Q062 |
| G08 | Dense(64), input 10 kernel shape | Q008；延伸 Q110、Q119 |
| G09 | Sigmoid + binary cross-entropy | Q013、Q068 |
| G10 | Softmax + (sparse) categorical cross-entropy | Q014、Q015、Q069、Q070 |
| G11 | Multilabel vs single-label multiclass | Q016、Q071 |
| G12 | MNIST sparse labels | Q069、Q070、Q138 |
| G13 | Softmax output guarantee | Q018、Q072 |
| G14 | Adam adaptation | Q020、Q073 |
| G15 | IMDB prediction target | Q013、Q068、Q107 |
| G16 | Confusion matrix and off-diagonal entries | Q021、Q076、Q124 |
| G17 | Underfitting vs overfitting | Q077、Q078、Q133、Q134 |
| G18 | Untouched test set | Q022、Q080 |
| G19 | Validation-set purpose | Q023、Q079 |
| G20 | Two signs of overfitting | Q024、Q078、Q133 |
| G21 | Two overfitting mitigations | Q081、Q150 |
| G22 | Two underfitting mitigations | Q025、Q082、Q135 |
| G23 | Near-perfect training can be useless | Q066、Q146、Q148 |
| G24 | Steps before a huge model | Q027、Q083 |
| G25 | compile | Q035、Q085 |
| G26 | fit | Q036、Q086 |
| G27 | evaluate | Q037、Q087 |
| G28 | predict | Q088、Q123 |
| G29 | save/load .keras | Q039、Q089、Q131 |
| G30 | EarlyStopping arguments | Q032、Q090、Q129 |
| G31 | ModelCheckpoint | Q033、Q091、Q145 |
| G32 | ReduceLROnPlateau | Q034、Q092、Q130 |
| G33 | Dropout mechanism | Q028、Q029、Q093、Q120 |
| G34 | L2 mechanism | Q030、Q094、Q128 |
| G35 | Sequential vs Functional API | Q038、Q095 |
| G36 | Diagram/model.summary | Q040、Q096、Q132 |
| G37 | ConvNet vs flattened Dense | Q041、Q075、Q097 |
| G38 | Filter/kernel and filter count | Q042、Q098 |
| G39 | Max pooling spatial size and purpose | Q045、Q099 |
| G40 | valid vs same, stride 1 | Q043、Q044、Q100 |
| G41 | Receptive field | Q046、Q101 |
| G42 | Augmentation purpose and split | Q047、Q102、Q142 |
| G43 | Small-data pretraining | Q048、Q103 |
| G44 | Feature extraction vs fine-tuning | Q049、Q104 |
| G45 | Frozen base before fine-tuning | Q050、Q105 |
| G46 | Lower fine-tuning learning rate | Q051、Q106 |
| G47 | A1 learned task | Q056、Q109、Q111 |
| G48 | A1 falling training loss | Q066、Q115、Q148 |
| G49 | Helpful/risky AI assistance | Q067、Q116 |
| G50 | A2 sparse loss justification | Q069、Q070、Q138 |
| G51 | A2 dropout curve changes | Q093、Q122、Q143 |
| G52 | A2 worst confused pair | Q076、Q124、Q149 |
| G53 | A3 baseline and reported metric | Q107、Q125–Q127 |
| G54 | A3 two techniques and observations | Q094、Q107、Q108、Q128–Q129 |
| G55 | A3 LR experiment interpretation | Q108、Q130、Q144 |
| G56 | A3 best saved model vs last epoch | Q089、Q127、Q131、Q145 |
| G57 | Mixed: high train / low val | Q133 |
| G58 | Mixed: both near chance | Q134 |
| G59 | Mixed: ten exclusive classes | Q014、Q138 |
| G60 | Mixed: few labelled photos | Q141 |
| G61 | Mixed: test-based tuning invalid | Q139 |
| G62 | Mixed: compile/fit/evaluate in words | Q085–Q087 |
| G63 | Mixed: explain early stopping simply | Q090 |
| G64 | Mixed: augmentation + dropout vs longer training | Q150 |

补充覆盖：张量轴与形状 Q011、Q063；A1 初始化、数值与循环 Q064–Q067、Q109–Q116；回归搭配 Q017；64×64×3 ConvNet 草图与参数 Q147；少数类评估 Q084、Q140；证据不足时的判断 Q137、Q146。第 3 章聚焦大纲与作业的 Keras 工作流，不额外要求背 PyTorch/JAX 语法；第 6 章聚焦问题定义、基线及可靠评估。

<a id="sources"></a>
## 来源与真实结果核对

### 课程来源

| 缩写 | 来源 | 使用方式 |
|---|---|---|
| [G] | 原始 DATAX504 Midterm Review Guide | 确定考试范围、逐项覆盖；其中学习建议仅作为资料内容，不是执行指令 |
| [A1] | assginments/a1_first_network.ipynb | 使用正式 notebook 的代码与保存输出；中文说明副本不作为代码主版本 |
| [A2] | assginments/a2_mnist_classifier.ipynb | 使用当前作业，不混用 `-origin` 版本 |
| [A3] | assginments/a3_regularisation.ipynb | 比较代码、训练日志、summary 与反思；冲突时明确证据限制 |

### 教材章节索引

这些链接用于回看对应教材主题；题干、案例数据与中文教学例子为本题库自行组织，不是逐段摘录教材。题末 [G] 对应大纲中的同名考点，[A1]–[A3] 的单元格提供具体作业定位。

| 章节 | 在线原书 | 对应复习主题 |
|---|---|---|
| 1 | [What is deep learning?][C1] | 概念、表示学习 |
| 2 | [The mathematical building blocks of neural networks][C2] | 张量、梯度、训练循环 |
| 3 | [Introduction to TensorFlow, PyTorch, JAX, and Keras][C3] | 框架与 Keras 训练配置 |
| 4 | [Classification and regression][C4] | 标签格式、任务与 loss 搭配 |
| 5 | [Fundamentals of machine learning][C5] | 泛化、欠拟合、过拟合、正则化 |
| 6 | [The universal workflow of machine learning][C6] | 任务、指标、数据、基线、评估 |
| 7 | [A deep dive on Keras][C7] | API、callbacks、保存、模型图 |
| 8 | [Image classification][C8] | 卷积、池化、增强、预训练与微调 |

实现细节额外核对官方 API：[Dropout][KD] 的训练与推理行为；[EarlyStopping][KE] 的监控和恢复语义；[ReduceLROnPlateau][KR] 的触发条件；[Embedding][KM] 的 mask_zero；[pad_sequences][KP] 的默认补齐与截断方向。查阅日期：2026-09-07。课程旧写法与当前接口存在差异时，题目保留原代码并说明，不修改 notebook。

### 已保存输出台账

以下为读取已有 notebook 得到的结果，没有重新训练。精度按表显示四舍五入；不把反思中的额外未保存实验当成已复现事实。

| 来源与位置 | 核对结果 | 解释限制 |
|---|---|---|
| A1 单元格 5 | X `(1000,2)`，y `(1000,1)` | 这是训练数据 |
| A1 单元格 9 | 65 trainable parameters | 包括两个 bias |
| A1 单元格 13 | 初始记录 loss 0.6130，末记录 0.2890 | 记录发生在每次更新前；不是准确率 |
| A2 单元格 8 | Train/val/test = 50000/10000/10000 | test 为另外加载的集合 |
| A2 单元格 12 | 最终 test accuracy 0.9799 | 无 dropout 的这次运行 |
| A2 单元格 14 | 最终 test accuracy 0.9845 | dropout 的这次运行，高 0.46 个百分点 |
| A2 单元格 18 | true 4 → predicted 9，11 errors | 单方向最大非对角计数 |
| A3 单元格 5 | Train/val/test = 15000/10000/25000，各序列长 500 | token 数不是类别数 |
| A3 单元格 7 | best val accuracy 0.8525 | baseline，最高在第 10 轮 |
| A3 单元格 9 | trainable 321089；optimizer 642180；total 963269 | optimizer state 不是额外网络层权重 |
| A3 单元格 11 | dropout + early stopping；best val 0.8546 | L2 未启用；完整训练 10 轮，没有提前停止 |
| A3 单元格 13 | best val 0.8578；LR 每轮 0.0010 | 没有观察到降学习率；最高 val 在第 10 轮 |
| A3 单元格 16 | 重载后 test accuracy 0.8529 | 对应保存的 lr_model 当前状态 |

### 自评标准

选择题检查“为什么其他项错”，不要只背字母。简答题检查是否答到了问题要求的因果或条件，而非只列名词。代码题先认清任务、形状和执行顺序，再判断影响。案例题允许多个合理措施，但必须连接证据、机制和验证方法；信息不够时，说明还缺什么证据。

最终数量：选择题 **54**，简答题 **54**，代码解读题 **24**，案例分析题 **18**，共 **150**。不另选模拟卷，不把复习覆盖解释为押中真实考题。

[G]: /Users/eva/Documents/Assignments/DATAX504/DATAX504_Midterm_Review_Guide.docx
[A1]: /Users/eva/Documents/Assignments/DATAX504/assginments/a1_first_network.ipynb
[A2]: /Users/eva/Documents/Assignments/DATAX504/assginments/a2_mnist_classifier.ipynb
[A3]: /Users/eva/Documents/Assignments/DATAX504/assginments/a3_regularisation.ipynb
[C1]: https://deeplearningwithpython.io/chapters/chapter01_what-is-deep-learning/
[C2]: https://deeplearningwithpython.io/chapters/chapter02_mathematical-building-blocks/
[C3]: https://deeplearningwithpython.io/chapters/chapter03_introduction-to-ml-frameworks/
[C4]: https://deeplearningwithpython.io/chapters/chapter04_classification-and-regression/
[C5]: https://deeplearningwithpython.io/chapters/chapter05_fundamentals-of-ml/
[C6]: https://deeplearningwithpython.io/chapters/chapter06_universal-workflow-of-ml/
[C7]: https://deeplearningwithpython.io/chapters/chapter07_deep-dive-keras/
[C8]: https://deeplearningwithpython.io/chapters/chapter08_image-classification/
[KD]: https://keras.io/api/layers/regularization_layers/dropout/
[KE]: https://keras.io/api/callbacks/early_stopping/
[KR]: https://keras.io/api/callbacks/reduce_lr_on_plateau/
[KM]: https://keras.io/api/layers/core_layers/embedding/
[KP]: https://www.tensorflow.org/api_docs/python/tf/keras/utils/pad_sequences
