

# Top 10 ways to make Maya go FASTER

## 1关闭一些显示设置：

首先是阴影，然后OCC，然后运动模糊然后抗锯齿

## 2关闭材质球实时预览

可以改用右键单个refresh swatch

## 3选择暂停viewport
<!-- more -->
## 4确保不实用的插件不加载

还可以导入的时候不自动载入ref，选择手动载入

## 5关闭模型的Adaptive open Subdiv

## 6设置Viewport2.0

1浮点渲染目标，此设置与色彩管理有关系，代价是CPU的RAM

推荐，R32G32B32A32_Float > R16G16B16A16_Float

也可以完全禁用

2性能下把透明贴图算法改成 Alpha Cut

代价是牺牲了半透明的效果，但是提高了速度

## 7使用贴图时，最轻量的贴图是JPEG