# 1. NMS

在执行目标检测任务时，算法可能对同一目标有多次检测。NMS 是一种让你确保算法只对每个对象得到一个检测的方法，即“清理检测”。如下图所示：

![img](assets/20190309142710836-1557977439396.png)

这个选出来的框框叫做Bounding Box / Boundary Box (BBox)，每个BBox除了框的中心(x,y)和长宽 (h,w ) 外，几乎都会有一个Confidence score。这个score代表的就是这个框框是background还是foreground的置信度，基本上介于0~1之间。即当score=1时，代表这个框可以肯定是一个目标，但这个物件是什么类别还要看相对应的分类准确率。

所以，一般每个BBox带有分类信息（假设我们只检测2个物体），一般算法至少包含：BBox中心位置 (x,y)、BBox长宽 (h,w)、confidence score、BBox相对应的目标概率，共5+2=7。如果要做100类的预测，那么BBox就会有5+100=105个输出。

------

理解 BBox 输入或输出格式，通常会见到两种格式

- 第一种，BBox 中心位置(x, y) + BBox 長寬(h, w) + Confidence Score；
- 第二种，BBox 左上角点(x1,y1) + BBox 右下角点(x2,y2) + Confidence Score；

两种表达的本质是一样的，均为五个变量。与 BBox 相关的四个变量用于计算 IOU，Confidence Score 用于排序。