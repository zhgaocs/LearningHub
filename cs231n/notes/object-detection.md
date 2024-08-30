# 目标检测

## 1. R-CNN(Region-based CNN)

### 1.1 R-CNN

1. 候选区域生成
    + 使用选择性搜索（Selective Search）算法在输入图像中生成一系列可能包含目标物体的候选区域
2. 特征提取
    + 将每个候选区域调整为固定大小，并输入预训练的卷积神经网络（如AlexNet）进行特征提取
3. 分类
    + 将提取的特征输入到支持向量机（SVM）中，以判断该区域是否包含目标物体及其类别
4. 边界框回归
    + 使用回归模型对候选区域的位置进行微调，以获得更精确的目标位置
5. 非极大值抑制（Non-Maximum Suppression）
    + 对分类结果进行非极大值抑制，去除重叠的候选区域，保留最优的检测结果

### 1.2 Fast R-CNN

1. 特征提取
    + AlexNet等
2. 候选区域生成
    + Selective Search
3. ROI(Region of Interest) Pooling
    + 对生成的感兴趣区域进行ROI池化，将不同大小的候选区域映射到固定大小的特征图上
4. 分类和回归
    + 将ROI池化后的特征图输入到全连接层，进行分类和边界框回归。分类器（通常是Softmax）用于判断每个候选区域的类别，回归器用于微调边界框的位置
5. 非极大值抑制

### 1.3 Faster R-CNN

1. 特征提取
2. 区域提议网络（Region Proposal Network）
    + 在特征图上滑动一个小网络，生成一系列候选区域，并预测这些区域是否包含目标以及目标的边界框
3. ROI Pooling
4. 分类和回归
5. 非极大值抑制

### 1.4 Mask R-CNN

## 2. SSD(Single Shot Detection)

## 3. YOLO(You Only Look Once)
