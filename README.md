# 3D捏脸

网易《Face-to-Parameter Translation for Game Character Auto-Creation》论文复现

## 1、网络结构

G：Imitator网络

​	输入：连续参数208d（归一化到0~1），离散参数102d；

​	输出：游戏角色正脸图像（512x512x3）；

​	SGD，batch_size=16，momentum=0.9，learning_rate=0.01 decay 10% per 50epochs，max_epochs=500；

F1：lightcnn，做人脸识别，确保生成前后为同一个人，算余弦损失；

​	输入：128x128x1灰度图；

​	输出：256d向量；

F2：faceparse，BiSeNet网络，做面部语义分割，算带权重的L1损失；（不算L2的原因：L1的稀疏性，能突出五官的特征；而L2是平滑性）

​	输入：256x256x3 RGB图像；

​	输出：语义分割结果；

​	语义概率图增强人脸特征，采用不同权重突出五官的特征；

## 2、数据预处理

​	Face alignment：dlib

## 3、缺点

（1）对人脸鲁棒性低：人脸姿态、遮挡敏感；

（2）离散参数求解未能有效解决；

（3）需要多次迭代优化：基于梯度下降的算法运算效率低；

（4）只适合真人向的游戏角色；
