# 目标检测



## 参考资料

- 1 [目标检测入门，看这篇就够了](https://zhuanlan.zhihu.com/p/34142321)







## RCNN

### 核心贡献

- CNN 可以用于基于区域定位和分割。（这里定位是指的）影响2-stage的方法
- fine-tuning



IoU阈值的选择对结果影响显著，这里要谈两个threshold，一个用来识别正样本（如跟ground truth的IoU大于0.5），另一个用来标记负样本（即背景类，如IoU小于0.1），而介于两者之间的则为难例（Hard Negatives），若标为正类，则包含了过多的背景信息，反之又包含了要检测物体的特征，因而这些Proposal便被忽略掉。

位置坐标的回归（Bounding-Box Regression），这一过程是Region Proposal向Ground Truth调整，实现时加入了log/exp变换来使损失保持在合理的量级上，可以看做一种标准化（Normalization)操作