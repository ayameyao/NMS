# 3.4. 代码分析

源代码：<https://github.com/yihui-he/KL-Loss/>

在detectron/core/test.py 有 Softer-NMS (配置cfg.STD_NMS), Soft-NMS (配置cfg.TEST.SOFT_NMS.ENABLED) 以及NMS的实现。

798-812行：

```python
if cfg.STD_NMS:
    confs_j = confs[inds, j * 4:(j + 1) * 4]
    nms_dets, _ = py_nms_utils.soft(dets_j,
            confidence=confs_j )
elif cfg.TEST.SOFT_NMS.ENABLED:
    nms_dets, _ = box_utils.soft_nms(
        dets_j,
        sigma=cfg.TEST.SOFT_NMS.SIGMA,
        overlap_thresh=cfg.TEST.NMS,
        score_thresh=0.0001,
        method=cfg.TEST.SOFT_NMS.METHOD
    )
else:
    keep = box_utils.nms(dets_j, cfg.TEST.NMS)
    nms_dets = dets_j[keep, :]
```

在detectron/utils/py_cpu_nms.py文件有Softer-NMS的具体实现，加权求平均在50-56行代码实现：

```python
ovr_bbox = np.where((ious[i, i:N] > thresh))[0] + i
assert cfg.STD_NMS
if cfg.STD.METHOD == 'stdiou':
    p = np.exp(-(1-ious[i, ovr_bbox])**2/cfg.STD.IOU_SIGMA)
    dets[i,:4] = p.dot(dets[ovr_bbox, :4] / confidence[ovr_bbox]**2) / p.dot(1./confidence[ovr_bbox]**2)
else:
    assert cfg.STD.METHOD == 'soft'
```
