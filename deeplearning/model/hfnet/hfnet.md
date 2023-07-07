#  HFnet

[论文链接](https://openaccess.thecvf.com/content_CVPR_2019/html/Sarlin_From_Coarse_to_Fine_Robust_Hierarchical_Localization_at_Large_Scale_CVPR_2019_paper.html)

## 背景

​		HF-NET主要解决视觉定位中的问题，视觉定位是指，在大尺度场景下，已知环境地图，给定任意一张图片，计算出该图片对应的位姿。

​		目前比较流行的方法是，由粗到细两步定位。先通过图像检索的方式找到最接近的关键帧，再与该关键帧匹配局部特征。由于关键帧位姿已知，所以通过PnP等方法可以估计出当前帧位姿。两步定位可以避免从所有关键帧中直接匹配带来的时间复杂度，同时避免了将整个环境地图加载进内存带来的空间复杂度。

