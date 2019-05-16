# 3.1. 算法思想

论文的motivation来自于NMS时用到的score仅仅是分类置信度得分，不能反映Bounding box的定位精准度，即分类置信度和定位置信非正相关的。NMS只能解决分类置信度和定位置信度都很高的，但是对其它三种类型：“分类置信度低-定位置信度低”，“分类置信度高-定位置信度低”，“分类置信度低-定位置信度高“都无法解决。

| 定位置信度 \| 分类置信度 |    高    |    低    |
| :----------------------: | :------: | :------: |
|            高            | $\surd$  | $\times$ |
|            低            | $\times$ | $\times$ |

论文首先假设Bounding box的是高斯分布，round truth bounding box是狄拉克delta分布（即标准方差为0的高斯分布极限）。

KL 散度用来衡量两个概率分布的非对称性度量，KL散度越接近0代表两个概率分布越相似。

论文提出的KL loss，即为最小化Bounding box regression loss，即Bounding box的高斯分布和ground truth的狄拉克delta分布的KL散度。直观上解释，KL Loss使得Bounding box预测呈高斯分布，且与ground truth相近。而将包围框预测的标准差看作置信度。

论文提出的Softer-NMS，基于soft-NMS，对预测标准方差范围内的候选框加权平均，使得高定位置信度的bounding box具有较高的分类置信度。