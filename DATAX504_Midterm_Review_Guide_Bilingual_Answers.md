# DATAX504 Midterm Review Guide 中英文逐题答案

## Exam Scope 考试范围

**English:** Closed-book midterm in the evening of Week 7. Covers *Deep Learning with Python* Chapters 1-8, lectures Weeks 1-6, and Assignments 1-3. Week 7 lecture topics from Chapters 9-10, such as Grad-CAM, filter visualisation, and modern ConvNet architecture patterns beyond Chapter 8, are not on the midterm.

**中文：** 第 7 周晚上的闭卷期中考试。范围包括《Deep Learning with Python》第 1-8 章、第 1-6 周讲座、Assignment 1-3。第 7 周的第 9-10 章内容不考，比如 Grad-CAM、filter visualisation，以及超过第 8 章范围的现代 ConvNet 架构模式。

## How To Study 高效复习方法

**English:** For each question, practise answering in 2-5 sentences without notes. Then compare your answer with lecture notes, Chollet, and your assignment notebooks. Focus on why, when, and what can go wrong rather than memorising formulas. Re-read or re-run Assignments 1-3 and explain every plot and printed metric.

**中文：** 每个问题先不看笔记，用 2-5 句话回答。然后再对照课件、Chollet 教材和自己的 assignment notebooks。重点不是背公式，而是能解释为什么、什么时候用、哪里容易出错。重新阅读或运行 Assignment 1-3，确保每张图和每个指标都能说清楚。

## Chapters 1-2: Deep Learning, Tensors, And Training Loop

### 1. What does it mean that deep learning learns hierarchical representations?

**English answer:** Deep learning learns representations in layers. Early layers often learn simple patterns, and later layers combine them into more abstract features. For images, this might mean edges first, then textures or parts, then object-level patterns.

**中文答案：** 深度学习会一层一层学习表示。前面的层通常学习简单模式，后面的层把简单模式组合成更抽象的特征。比如图像中，低层可能学边缘，高层可能学纹理、局部结构，最后学到物体级别的模式。

### 2. Why do we need nonlinear activations between layers?

**English answer:** Without nonlinear activations, stacking many Dense layers is still equivalent to one linear transformation. Nonlinear activations let the network model complex relationships. They make deep networks expressive enough to learn useful patterns.

**中文答案：** 如果没有非线性激活函数，多个 Dense 层叠在一起本质上仍然只是一个线性变换。非线性激活让网络可以表达复杂关系。这样深层网络才有能力学习真实数据中的复杂模式。

### 3. In plain language, what is a loss function measuring?

**English answer:** A loss function measures how bad the model's predictions are compared with the true answers. Lower loss means the predictions are closer to the target according to the chosen criterion. Training tries to adjust weights to reduce this loss.

**中文答案：** loss function 衡量模型预测和真实答案之间“差得有多远”。loss 越低，说明按照当前标准预测越接近目标。训练的目的就是不断调整权重，让 loss 下降。

### 4. What is the difference between a forward pass and a backward pass?

**English answer:** In the forward pass, input data moves through the network to produce predictions and compute loss. In the backward pass, gradients of the loss with respect to weights are computed. The optimizer then uses those gradients to update the weights.

**中文答案：** forward pass 是输入数据经过网络，得到预测并计算 loss。backward pass 是计算 loss 对各个权重的梯度。优化器再根据这些梯度更新权重。

### 5. What role does the chain rule play in backpropagation?

**English answer:** The chain rule lets us compute how each earlier weight contributes to the final loss through a chain of operations. Backpropagation applies the chain rule layer by layer from the output back to the input. This gives the gradients needed for learning.

**中文答案：** chain rule 让我们能计算前面每个权重如何通过一连串操作影响最终 loss。backpropagation 就是从输出层往前逐层应用 chain rule。这样才能得到每个权重需要更新的方向和大小。

### 6. What is a mini-batch, and why not update on one example only or on the whole dataset every time?

**English answer:** A mini-batch is a small group of training examples processed together before one weight update. One example at a time can be noisy and inefficient. The whole dataset gives stable gradients but is often slow and memory-heavy, so mini-batches are a practical compromise.

**中文答案：** mini-batch 是一次训练中一起送入模型的一小组样本，然后模型更新一次权重。每次只用一个样本会很 noisy，也不高效。每次用完整数据集更稳定，但速度慢、占内存，所以 mini-batch 是折中方案。

### 7. What problem can ReLU help with compared with sigmoid in deep stacks?

**English answer:** Sigmoid can saturate, producing very small gradients in deep networks. This can make learning slow or cause vanishing gradients. ReLU is simpler and keeps stronger gradients for positive inputs, so it often trains deep networks more effectively.

**中文答案：** sigmoid 在输入很大或很小时容易饱和，梯度会变得非常小。深层网络中这会导致学习很慢，甚至出现 vanishing gradients。ReLU 对正数部分梯度更直接，通常更容易训练深层模型。

### 8. If a `Dense(64)` layer receives input features of size 10, what is the shape of its weight matrix, ignoring bias?

**English answer:** The weight matrix shape is `(10, 64)`. There is one row for each input feature and one column for each output unit. The layer maps a 10-dimensional input to a 64-dimensional output.

**中文答案：** 权重矩阵形状是 `(10, 64)`。10 对应输入特征数，64 对应输出神经元数量。这个层把 10 维输入映射成 64 维输出。

## Chapters 3-4: Frameworks, Classification, And Regression Pairings

### 9. When is sigmoid + binary cross-entropy the right pairing?

**English answer:** Use sigmoid with binary cross-entropy for binary classification, where each example belongs to one of two classes. Sigmoid outputs a probability-like value between 0 and 1. Binary cross-entropy measures the error for that binary decision.

**中文答案：** 当任务是二分类时，通常使用 sigmoid + binary cross-entropy。sigmoid 输出 0 到 1 之间的概率式结果。binary cross-entropy 用来衡量这个二分类预测和真实标签之间的差异。

### 10. When is softmax + sparse categorical cross-entropy the right pairing?

**English answer:** Use softmax with sparse categorical cross-entropy for single-label multiclass classification when labels are integer class IDs. Softmax produces a probability distribution over mutually exclusive classes. Sparse categorical cross-entropy works directly with integer labels.

**中文答案：** 当任务是单标签多分类，而且标签是整数类别编号时，用 softmax + sparse categorical cross-entropy。softmax 输出互斥类别上的概率分布。sparse categorical cross-entropy 可以直接处理整数标签，不需要 one-hot。

### 11. How is a multilabel problem different from a multiclass single-label problem, and what output/loss pairing fits multilabel?

**English answer:** In multiclass single-label classification, each example belongs to exactly one class. In multilabel classification, each example can belong to multiple classes at the same time. Multilabel problems usually use independent sigmoid outputs and binary cross-entropy.

**中文答案：** 单标签多分类中，每个样本只能属于一个类别。多标签分类中，一个样本可以同时属于多个类别。多标签通常使用多个独立的 sigmoid 输出，并搭配 binary cross-entropy。

### 12. Why does MNIST use sparse categorical cross-entropy comfortably?

**English answer:** MNIST has 10 mutually exclusive digit classes, and each label is an integer from 0 to 9. Sparse categorical cross-entropy is designed for this label format. It avoids converting labels into one-hot vectors.

**中文答案：** MNIST 有 10 个互斥的数字类别，每个标签是 0 到 9 的整数。sparse categorical cross-entropy 正好适合这种整数标签格式。它不需要把标签转换成 one-hot。

### 13. What does softmax guarantee about its outputs?

**English answer:** Softmax guarantees that all output values are non-negative and sum to 1. This lets us interpret the outputs as a probability distribution across classes. It is appropriate when classes are mutually exclusive.

**中文答案：** softmax 保证所有输出都是非负数，并且总和等于 1。因此可以把输出理解为各类别的概率分布。它适合类别互斥的分类任务。

### 14. What is Adam adapting during training?

**English answer:** Adam adapts the effective learning rate for each parameter using estimates of recent gradient averages and squared gradients. This helps training be more stable and often faster than plain gradient descent.

**中文答案：** Adam 会根据最近梯度的一阶和二阶信息，为每个参数调整有效学习率。这样训练通常比普通 gradient descent 更稳定，也经常更快。

### 15. For IMDB-style sentiment, what is the model predicting?

**English answer:** The model predicts whether a movie review expresses positive or negative sentiment. In the usual binary setup, it outputs a probability-like score for the positive class. The target label is typically 0 or 1.

**中文答案：** IMDB 情感分类中，模型预测影评是正面还是负面。常见二分类设置下，模型输出一个表示“正面”的概率式分数。真实标签通常是 0 或 1。

### 16. What is a confusion matrix, and what does a large off-diagonal entry mean?

**English answer:** A confusion matrix compares true classes with predicted classes. Diagonal entries are correct predictions. A large off-diagonal entry means the model often confuses one specific true class with another predicted class.

**中文答案：** confusion matrix 用来比较真实类别和预测类别。对角线表示预测正确。某个非对角线数值很大，说明模型经常把一个真实类别误判成另一个类别。

## Chapters 5-6: Generalisation And Machine Learning Workflow

### 17. Define underfitting vs overfitting using train vs validation behaviour.

**English answer:** Underfitting means the model performs poorly on both training and validation data, so it has not learned enough. Overfitting means training performance is strong but validation performance is much worse. Overfitting shows the model has learned patterns that do not generalise.

**中文答案：** underfitting 是训练集和验证集表现都不好，说明模型还没有学到足够的模式。overfitting 是训练集表现很好，但验证集表现明显差。overfitting 说明模型学到了不能泛化的细节或噪声。

### 18. Why do we keep a test set untouched until the end?

**English answer:** The test set estimates how the final model performs on unseen data. If we use it during tuning, we leak information from the test set into our decisions. Then the test result becomes overly optimistic and no longer fair.

**中文答案：** test set 用来估计最终模型在未见数据上的表现。如果调参时反复看 test set，就会把测试集信息泄露进模型选择过程。这样测试结果会偏乐观，不再公平。

### 19. What is the validation set for?

**English answer:** The validation set is used during development to compare models, tune hyperparameters, and detect overfitting. It guides decisions without touching the test set. It is not used as the final unbiased performance estimate.

**中文答案：** validation set 用于开发过程中比较模型、调整超参数、观察是否 overfit。它帮助我们做选择，同时保留 test set 不被污染。它不是最终无偏评估。

### 20. Give two signs of overfitting on learning curves.

**English answer:** One sign is training loss keeps decreasing while validation loss starts increasing. Another sign is training accuracy keeps improving while validation accuracy plateaus or drops. The gap between training and validation performance grows.

**中文答案：** 第一个信号是 training loss 继续下降，但 validation loss 开始上升。第二个信号是 training accuracy 继续提高，但 validation accuracy 停滞或下降。训练和验证表现之间的差距越来越大。

### 21. Give two concrete ways to reduce overfitting.

**English answer:** Add regularisation such as dropout or L2 weight regularisation. Use data augmentation when appropriate, especially for images. Other options include reducing model size, early stopping, or collecting more data.

**中文答案：** 可以加入正则化，比如 dropout 或 L2 regularisation。图像任务中可以使用 data augmentation。其他方法包括减小模型容量、early stopping，或者收集更多数据。

### 22. Give two concrete ways to address underfitting.

**English answer:** Increase model capacity by adding layers or units. Train longer or use a better learning rate if optimisation has not converged. You can also improve features or reduce excessive regularisation.

**中文答案：** 可以增加模型容量，比如加层或增加神经元。也可以训练更久，或调整 learning rate，让优化更充分。还可以改进特征，或减少过强的正则化。

### 23. Why can a model with near-perfect training accuracy still be useless?

**English answer:** Near-perfect training accuracy may mean the model memorised the training data. If it performs poorly on validation or test data, it has not learned general patterns. A useful model must generalise to new examples.

**中文答案：** 训练准确率接近完美可能只是模型记住了训练数据。如果验证集或测试集表现差，就说明它没有学到可泛化的规律。真正有用的模型必须能处理新样本。

### 24. In the universal workflow, what comes before jumping to a huge model?

**English answer:** First define the problem, choose the right evaluation metric, prepare data, and build a simple baseline. Then improve the model only after understanding baseline performance and failure modes. A huge model should not be the first step.

**中文答案：** 在直接上大模型之前，应先明确问题、选择评估指标、准备数据，并建立简单 baseline。然后根据 baseline 表现和错误类型逐步改进。巨大模型不应该是第一步。

## Chapter 7 And Assignment 3: Keras Workflow And Regularisation

### 25. What does `model.compile(...)` do?

**English answer:** `compile` configures the training process. It specifies the optimizer, loss function, and metrics. It does not train the model yet.

**中文答案：** `compile` 用来配置训练过程。它指定 optimizer、loss function 和 metrics。它本身还没有开始训练模型。

### 26. What does `model.fit(...)` do?

**English answer:** `fit` trains the model on data for one or more epochs. It repeatedly performs forward passes, computes loss, runs backpropagation, and updates weights. It can also track validation metrics.

**中文答案：** `fit` 用数据训练模型一个或多个 epoch。它会重复执行 forward pass、计算 loss、backpropagation 和权重更新。也可以同时记录 validation metrics。

### 27. What does `model.evaluate(...)` do?

**English answer:** `evaluate` measures the model's loss and metrics on a dataset without training. It is commonly used on validation or test data. It helps estimate model performance.

**中文答案：** `evaluate` 在某个数据集上计算 loss 和 metrics，但不训练模型。它常用于 validation 或 test data。它帮助我们评估模型表现。

### 28. What does `model.predict(...)` do?

**English answer:** `predict` runs the model on input data and returns output predictions. It does not need true labels. The outputs may be probabilities, class scores, or numeric predictions depending on the model.

**中文答案：** `predict` 用模型对输入数据生成预测结果。它不需要真实标签。输出可能是概率、类别分数或数值预测，取决于任务和模型结构。

### 29. What does `model.save(...)` / reloading a `.keras` file do?

**English answer:** `model.save` stores the model architecture, weights, and training configuration in a file. Reloading the `.keras` file restores the saved model. This lets you reuse the best model later without retraining.

**中文答案：** `model.save` 会把模型结构、权重和训练配置保存到文件中。重新加载 `.keras` 文件可以恢复保存时的模型。这样之后可以直接使用最佳模型，不需要重新训练。

### 30. What does `EarlyStopping(monitor="val_loss", patience=..., restore_best_weights=True)` do?

**English answer:** EarlyStopping watches validation loss and stops training when it has not improved for a set number of epochs. `patience` controls how long it waits. `restore_best_weights=True` returns the model to the weights from the best validation-loss epoch.

**中文答案：** EarlyStopping 会监控 validation loss，如果连续若干个 epoch 没有改善，就停止训练。`patience` 决定等待多少轮。`restore_best_weights=True` 会把模型恢复到 validation loss 最好那一轮的权重。

### 31. What is `ModelCheckpoint` useful for?

**English answer:** ModelCheckpoint saves the model during training, often only when validation performance improves. It protects the best model even if later epochs overfit. It is useful when training is long or unstable.

**中文答案：** ModelCheckpoint 会在训练过程中保存模型，通常是在验证表现变好时保存。这样即使后面的 epoch overfit，也能保留最佳模型。它对长时间训练或不稳定训练很有用。

### 32. What problem is `ReduceLROnPlateau` trying to solve?

**English answer:** ReduceLROnPlateau lowers the learning rate when a monitored metric stops improving. This helps when training gets stuck near a plateau. A smaller learning rate may allow finer progress toward a better minimum.

**中文答案：** ReduceLROnPlateau 在某个指标停止改善时降低 learning rate。它用于训练卡在平台期的情况。更小的学习率可能帮助模型更细致地继续下降到更好的解。

### 33. How does dropout reduce overfitting?

**English answer:** Dropout randomly sets some activations to zero during training. This prevents units from relying too much on specific other units. It encourages the network to learn more robust, distributed features.

**中文答案：** dropout 在训练时随机把一部分激活值设为 0。这样神经元不能过度依赖某些特定神经元。它会促使网络学习更稳健、更分散的特征表示。

### 34. How does L2 regularisation discourage overfitting?

**English answer:** L2 regularisation adds a penalty for large weights to the loss. This encourages smaller, smoother weights. Smaller weights can reduce overly complex decision boundaries and improve generalisation.

**中文答案：** L2 regularisation 会在 loss 中加入对大权重的惩罚。它鼓励模型使用更小、更平滑的权重。较小的权重可以减少过复杂的决策边界，帮助泛化。

### 35. When would you prefer Sequential vs the Functional API?

**English answer:** Use Sequential for simple models that are a straight stack of layers with one input and one output. Use the Functional API when the model has multiple inputs or outputs, shared layers, branches, or skip connections. The Functional API is more flexible.

**中文答案：** 如果模型只是简单的一层接一层、单输入单输出，可以用 Sequential。如果模型有多个输入或输出、共享层、分支、跳跃连接，就应该用 Functional API。Functional API 更灵活。

### 36. Why generate an architecture diagram or `model.summary()`?

**English answer:** Architecture diagrams and `model.summary()` help verify the model structure, output shapes, and parameter counts. They make it easier to catch shape mistakes. They also help explain the model clearly in assignments.

**中文答案：** 架构图和 `model.summary()` 可以帮助检查模型结构、输出形状和参数数量。它们能更容易发现 shape 错误。它们也有助于在 assignment 中清楚解释模型。

## Chapter 8: ConvNets From Scratch And Pretrained Models

### 37. Why are ConvNets usually better than flattened Dense stacks for images?

**English answer:** ConvNets preserve spatial structure and learn local patterns such as edges and textures. They reuse the same filters across the image, which is parameter-efficient. Flattened Dense stacks destroy spatial layout and require many more parameters.

**中文答案：** ConvNets 保留图像的空间结构，并学习局部模式，比如边缘和纹理。卷积核在整张图上共享参数，因此更高效。flatten 后接 Dense 会破坏空间结构，并需要大量参数。

### 38. What is a filter/kernel, and what does the number of filters control?

**English answer:** A filter or kernel is a small weight matrix that slides over the input to detect local patterns. Each filter learns a different feature. The number of filters controls how many feature maps the layer outputs.

**中文答案：** filter/kernel 是一个小的权重矩阵，会在输入上滑动，用来检测局部模式。每个 filter 学习不同特征。filter 数量决定该层输出多少个 feature maps。

### 39. What does max pooling do to spatial size and why is it used?

**English answer:** Max pooling reduces spatial dimensions by taking the maximum value in small windows. It makes representations smaller and more manageable. It also adds some tolerance to small shifts in the input.

**中文答案：** max pooling 会在小窗口中取最大值，从而减小空间尺寸。这样表示更小、更易处理。它也让模型对输入中的小位置变化更有容忍度。

### 40. Difference between `padding="valid"` and `padding="same"` with stride 1?

**English answer:** With `padding="valid"`, no padding is added, so spatial dimensions shrink after convolution. With `padding="same"` and stride 1, padding is added so the output height and width stay the same as the input. The choice affects feature-map size.

**中文答案：** `padding="valid"` 不加 padding，所以卷积后空间尺寸会变小。`padding="same"` 在 stride 1 时会加 padding，使输出高度和宽度与输入相同。两者会影响 feature map 的尺寸。

### 41. What is a receptive field, and why does stacking convolutions grow it?

**English answer:** A receptive field is the region of the original input that affects one activation in a later layer. Stacking convolutions grows the receptive field because each later unit depends on several earlier local units. This lets deeper layers capture larger patterns.

**中文答案：** receptive field 是后面某个激活值能“看到”的原始输入区域。卷积层叠加后，每一层的神经元都依赖前一层的一片局部区域，所以感受野会逐渐变大。这样深层可以捕捉更大范围的模式。

### 42. Why use data augmentation, and on which split?

**English answer:** Data augmentation creates varied versions of training examples, such as flipped or rotated images. It helps the model generalise and reduces overfitting. It should be applied to the training split, not validation or test data.

**中文答案：** data augmentation 会生成训练样本的变化版本，比如翻转、旋转图像。它帮助模型泛化并减少 overfitting。它应该只用于 training split，不应该用于 validation 或 test set。

### 43. With a small labelled image dataset, why might a pretrained model help?

**English answer:** A pretrained model has already learned general visual features from a large dataset. With limited labelled data, reusing those features can improve performance and reduce training needs. It is often better than training a large ConvNet from scratch.

**中文答案：** pretrained model 已经从大型数据集中学到了通用视觉特征。在标注数据很少时，复用这些特征可以提升表现并减少训练需求。它通常比从零训练大型 ConvNet 更可靠。

### 44. Distinguish feature extraction from fine-tuning.

**English answer:** In feature extraction, the pretrained base is frozen and only a new classifier head is trained. In fine-tuning, some upper layers of the pretrained base are unfrozen and trained with a small learning rate. Feature extraction is safer; fine-tuning can adapt the model further.

**中文答案：** feature extraction 中，预训练 base 被冻结，只训练新的分类头。fine-tuning 中，会解冻预训练 base 的部分高层，并用较小 learning rate 继续训练。feature extraction 更稳，fine-tuning 可以进一步适应新任务。

### 45. Why train a new head with the base frozen before fine-tuning?

**English answer:** A new head starts with random weights and can produce large, unstable gradients. If the pretrained base is unfrozen immediately, those gradients may damage useful pretrained features. Training the head first gives a stable starting point for fine-tuning.

**中文答案：** 新的分类头一开始是随机权重，可能产生较大且不稳定的梯度。如果一开始就解冻 base，这些梯度可能破坏预训练特征。先训练 head 可以为 fine-tuning 提供更稳定的起点。

### 46. Why is the learning rate usually lower when fine-tuning?

**English answer:** Fine-tuning changes pretrained weights that already contain useful features. A high learning rate may overwrite or damage those features. A lower learning rate makes small, careful updates.

**中文答案：** fine-tuning 会修改已经有用的预训练权重。过高的 learning rate 可能快速破坏这些特征。较低 learning rate 可以进行更小、更谨慎的更新。

## Assignments: Explain Your Own Work

### Assignment 1

### 47. What did your NumPy network learn to do?

**English answer:** It learned to map input data to target labels or outputs by adjusting weights using a training loop. The point was to understand the mechanics behind neural networks without relying only on Keras. You should connect your answer to the exact dataset and task used in your notebook.

**中文答案：** 它通过训练循环调整权重，学习把输入数据映射到目标标签或输出。这个 assignment 的重点是理解神经网络背后的基本机制，而不是只依赖 Keras。考试时要结合你 notebook 中的具体数据集和任务来回答。

### 48. What does a falling training-loss curve tell you?

**English answer:** It tells you the model is improving on the training data according to the loss function. The optimizer is finding weights that reduce training error. However, it does not prove the model generalises well; you need validation or test performance for that.

**中文答案：** training loss 下降说明模型在训练集上按 loss function 的标准变好了。优化器正在找到能降低训练误差的权重。但这不代表模型泛化好，还需要看 validation 或 test 表现。

### 49. Give one helpful and one risky aspect of using an AI coding assistant.

**English answer:** A helpful aspect is that it can explain code, suggest fixes, and speed up debugging. A risky aspect is that it may produce incorrect code that looks plausible. Students still need to understand, test, and verify all generated code.

**中文答案：** 有帮助的一面是 AI 可以解释代码、建议修复、加快 debug。风险是它可能生成看起来合理但实际错误的代码。学生必须自己理解、测试并验证生成的代码。

### Assignment 2

### 50. Why sparse categorical cross-entropy for MNIST?

**English answer:** MNIST is a 10-class single-label classification problem with integer labels from 0 to 9. Sparse categorical cross-entropy matches integer class labels. It pairs naturally with a softmax output layer.

**中文答案：** MNIST 是 10 类单标签分类，标签是 0 到 9 的整数。sparse categorical cross-entropy 适合整数类别标签。它通常和 softmax 输出层搭配。

### 51. What changed when you added dropout?

**English answer:** Dropout usually makes training accuracy or training loss improve more slowly because the model is being regularised. Validation performance may improve or become more stable if the original model was overfitting. The key comparison is the gap between train and validation curves.

**中文答案：** 加入 dropout 后，training accuracy 可能提升更慢，或 training loss 下降更慢，因为模型受到了正则化约束。如果原模型 overfit，validation 表现可能提升或更稳定。重点是比较 train 和 validation 曲线之间的差距。

### 52. How do you read the worst confused digit pair from a confusion matrix?

**English answer:** Look at the largest off-diagonal value in the confusion matrix. The row is the true digit and the column is the predicted digit, if your matrix uses the common true-row, predicted-column convention. That pair is the digit confusion the model makes most often.

**中文答案：** 找 confusion matrix 中最大的非对角线数值。通常行表示真实数字，列表示预测数字。这个位置对应的真实数字和预测数字，就是模型最常混淆的一对数字。

### Assignment 3

### 53. What was your baseline IMDB model, and what metric did you report?

**English answer:** A typical baseline IMDB model is a simple binary sentiment classifier using vectorised text, Dense layers, sigmoid output, and binary cross-entropy. The reported metric is usually accuracy, often with validation accuracy and test accuracy. Replace this with your exact architecture if your notebook used something different.

**中文答案：** 常见 IMDB baseline 是一个简单二分类情感模型：文本向量化、Dense 层、sigmoid 输出、binary cross-entropy。报告指标通常是 accuracy，包括 validation accuracy 和 test accuracy。如果你的 notebook 架构不同，要用你自己的实际模型回答。

### 54. Which two regularisation techniques did you use, and what did you observe?

**English answer:** Common answers include dropout and L2 regularisation. You should say how the training and validation curves changed: for example, training performance may reduce slightly while validation performance improves or the train-validation gap shrinks. The exact observation should match your Assignment 3 results.

**中文答案：** 常见答案包括 dropout 和 L2 regularisation。你需要说明 train 和 validation 曲线如何变化：比如训练表现可能略降，但验证表现提升，或 train-validation gap 变小。具体观察要以你 Assignment 3 的结果为准。

### 55. What did your learning-rate schedule experiment show?

**English answer:** A learning-rate schedule experiment shows how changing the learning rate during training affects convergence. Lowering the learning rate after progress stalls can sometimes improve validation loss or stabilise training. Your final answer should mention the specific trend shown in your notebook.

**中文答案：** learning-rate schedule 实验展示了训练过程中调整 learning rate 对收敛的影响。当训练停滞时降低 learning rate，有时可以改善 validation loss 或让训练更稳定。最终回答要结合你 notebook 里的具体趋势。

### 56. Why save and reload the best model rather than trusting the last epoch?

**English answer:** The last epoch may not be the best epoch because the model can start overfitting after validation performance peaks. Saving the best model keeps the weights from the best validation result. Reloading it ensures evaluation uses the strongest version, not merely the final one.

**中文答案：** 最后一轮不一定是最好的一轮，因为 validation 表现达到峰值后模型可能开始 overfit。保存最佳模型可以保留验证结果最好时的权重。重新加载它能确保评估的是最强版本，而不是单纯最后一轮。

## Mixed Practice Prompts 综合练习题

### 57. Train accuracy high, validation accuracy low: diagnosis and two fixes.

**English answer:** This is likely overfitting. The model has learned the training data well but does not generalise. Two fixes are adding dropout or L2 regularisation, and using early stopping or data augmentation where appropriate.

**中文答案：** 这通常是 overfitting。模型在训练集上学得很好，但不能泛化到验证集。两个修复方法是加入 dropout 或 L2 regularisation，以及使用 early stopping 或适合任务的数据增强。

### 58. Train and validation both stuck near chance: diagnosis and two fixes.

**English answer:** This suggests underfitting or an optimisation/data problem. The model may be too small, trained too briefly, using a poor learning rate, or receiving poorly prepared data. Two fixes are increasing model capacity and checking the data/loss/activation pairing; another is adjusting learning rate or training longer.

**中文答案：** 这说明可能 underfitting，或存在优化/数据问题。模型可能太小、训练不够、learning rate 不合适，或者数据处理有问题。两个修复方法是增加模型容量、检查数据和 loss/activation 搭配，也可以调整 learning rate 或训练更久。

### 59. You need 10 mutually exclusive classes: choose activation + loss and justify.

**English answer:** Use a 10-unit softmax output layer. If labels are integer class IDs, use sparse categorical cross-entropy; if labels are one-hot vectors, use categorical cross-entropy. Softmax is appropriate because the classes are mutually exclusive.

**中文答案：** 使用 10 个输出单元的 softmax 层。如果标签是整数类别编号，用 sparse categorical cross-entropy；如果标签是 one-hot，用 categorical cross-entropy。softmax 适合互斥类别，因为输出总和为 1。

### 60. You have few labelled photos: outline a transfer-learning plan in order.

**English answer:** Start with a pretrained ConvNet base and freeze it. Add a new task-specific classifier head and train that head on your labelled images, using augmentation on the training set. Then optionally unfreeze a few upper layers of the base and fine-tune with a low learning rate, evaluating on validation data and testing only at the end.

**中文答案：** 先使用预训练 ConvNet base，并冻结它。添加一个适合当前任务的新分类头，只训练这个 head，并在训练集上使用 augmentation。然后可以选择解冻 base 的少数高层，用较低 learning rate fine-tune，并用 validation set 监控表现，最后才用 test set。

### 61. A teammate tunes hyperparameters on the test set: explain why that is invalid.

**English answer:** The test set is supposed to simulate unseen future data. If hyperparameters are tuned using test results, the test set becomes part of the development process. The final test score is then biased and no longer measures true generalisation.

**中文答案：** test set 应该模拟未来未见数据。如果用 test result 调超参数，测试集就参与了开发过程。最终 test score 会有偏，不能再真实衡量泛化能力。

### 62. Explain compile vs fit vs evaluate without writing code.

**English answer:** Compile sets up how the model will learn: optimizer, loss, and metrics. Fit actually trains the model by updating weights on training data. Evaluate measures performance on a dataset without changing the weights.

**中文答案：** compile 是设置模型如何学习，包括 optimizer、loss 和 metrics。fit 是真正训练模型，用训练数据更新权重。evaluate 是在某个数据集上测表现，但不改变权重。

### 63. Explain early stopping as if to a classmate who has only seen training loss.

**English answer:** Training loss can keep improving even after the model starts memorising the training data. Early stopping watches validation loss, which better shows whether the model is still generalising. If validation loss stops improving for several epochs, training stops and the best weights can be restored.

**中文答案：** training loss 可能一直下降，即使模型已经开始记住训练集。early stopping 看的是 validation loss，因为它更能反映模型是否还能泛化。如果 validation loss 连续几轮不再改善，就停止训练，并可以恢复最佳权重。

### 64. For cats-vs-dogs style data, why augmentation + dropout often beat "just train longer."

**English answer:** Training longer often increases overfitting when the dataset is small. Augmentation creates useful variation in the training images, and dropout discourages the model from relying on fragile patterns. Together they improve generalisation more than simply giving the model more chances to memorise.

**中文答案：** 当数据集较小时，单纯训练更久往往会加重 overfitting。augmentation 给训练图片制造有用变化，dropout 防止模型依赖脆弱的局部模式。它们组合起来更能提升泛化，而不是让模型有更多机会记住训练集。

## Suggested Review Session 建议复习流程

1. **English:** Re-sketch the Chapter 4 pairing table: binary, multiclass, and multilabel.  
   **中文：** 重新画出第 4 章任务配对表：二分类、多分类、多标签。

2. **English:** Re-read Assignment 2 and Assignment 3 outputs, and explain every printed accuracy.  
   **中文：** 重读 Assignment 2 和 3 的输出，解释每一个 accuracy。

3. **English:** Sketch a ConvNet: layers, pooling, and shapes for a `64 x 64 x 3` input.  
   **中文：** 画一个 ConvNet 草图，说明层、pooling，以及 `64 x 64 x 3` 输入下的 shape 变化。

4. **English:** Write answers to the eight mixed practice prompts.  
   **中文：** 写出 8 道综合练习题的答案。

5. **English:** Self-mark against notes and list five weak spots.  
   **中文：** 对照笔记自我批改，并列出 5 个薄弱点。

## What Rigour Means For This Paper 本次考试中的严谨性

**English:** You are not expected to derive calculus proofs. You are expected to choose correct modelling pairings, diagnose underfitting and overfitting from behaviour, explain Keras training behaviour in words, connect ConvNets and pretrained models to small-data practice, and describe a sensible evaluation workflow.

**中文：** 这次考试不要求你推导微积分证明。你需要能够选择正确的建模搭配，能根据训练和验证表现诊断 underfitting/overfitting，能用语言解释 Keras 训练行为，能把 ConvNets 和预训练模型联系到小数据实践，并能描述合理的评估流程。

