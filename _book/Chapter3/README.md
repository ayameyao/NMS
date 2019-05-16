# 3. Softer-NMS

这篇文章是卡内基梅隆大学与旷视科技的研究人员提出了一种新的非极大抑制算法Softer-NMS，其方法是在Soft-NMS和NMS基础上改进，也算是一脉相承。论文不是简单的更改CNN结构或调整参数，引入高斯分布，狄拉克delta分布，KL 散度等数学知识，建议首先阅读维基百科/百度百科对相应的概念做基本了解。

论文地址：[Bounding Box Regression with Uncertainty for Accurate Object Detection](https://arxiv.org/abs/1809.08545)

