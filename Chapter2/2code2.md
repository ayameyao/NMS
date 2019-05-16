# 2.2. 代码实现

论文的作者提供R-FCN和Faster-RCNN集成soft-NMS的代码。

代码传送门：[soft-NMS](<https://github.com/bharatsingh430/soft-nms>)

其实最重要的改变在soft-nms/lib/nms/cpu_nms.pyx 79行到92行

```python
ov = iw * ih / ua #iou between max box and detection box
if method == 1: # linear
    if ov > Nt: 
        weight = 1 - ov
    else:
        weight = 1
elif method == 2: # gaussian
    weight = np.exp(-(ov * ov)/sigma)
else: # original NMS
    if ov > Nt: 
        weight = 0
    else:
        weight = 1
```

可以明显看到soft-NMS最重要是更新weight变量的值。采用线性加权时，更新为1-ov,高斯加权时引入sigma参数，而原始NMS算法时，直接取0或1。

