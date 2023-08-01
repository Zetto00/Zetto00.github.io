# Tapered Amplifer 搭建

<!--more-->

- [Tapered Amplifier - Toptica Eagleyard Products](https://www.toptica-eagleyard.com/?L=1&_product-family=tapered-amplifier&_wavelength_p76485559=650.00%2C980.00&id=161)
- 使用 TA 芯片为 GaAs Semiconductor Laser Diode
	- [TA 808nm 2W - Toptica Eagleyard](https://www.toptica-eagleyard.com/ey-product/eyp-tpa-0808-02000-4006-cmt04-0000/?_application%5B0%5D=quantum-technology&_application[]=spectroscopy)
## 光路计算
### 前提
由于 TA 的锥形放大过程，出射高斯光束的竖直方向比水平方向更受约束，从而发散角度更大，分别为 28° 和 14°。同时，存在 astigmatism 现象，即两个方向焦点位置不同，水平方向比竖直方向提前 700um。

要使光束很好地耦合到耦合头，一般的办法是使用透镜和柱面镜使光斑成为平行光，再用透镜组对光斑整形。这种方法可以随意选择柱透镜和透镜组的初始位置，再通过调透镜组提高耦合效率。

若想省去透镜组，需要对柱面镜的位置选择进行计算，并且使用单模高斯光耦合到单模光纤的模型。
### 提高单模光纤耦合效率
[单模光纤耦合 | thorlabs](https://www.thorlabs.com/newgrouppage9.cfm?objectgroup_id=12211&tabname=%E5%85%89%E7%BA%A4)

![|600](https://www.thorlabs.com/images/tabimages/MFD_Single_Mode_Fiber_A4-780.gif)
  
用模场直径（mode field diameter, MFD）来描述光束在光纤中传播的光斑大小。MFD 一般稍大于纤芯直径，多出的部分可以用光的波动性和倏逝波来理解。
  
假设光纤端面为平面并和光纤轴垂直，达到最高耦合效率需要满足
	- 光为高斯强度轮廓
	- 从光纤端面正入射
	- 束腰位于光纤端面
	- 束腰中心对准纤芯中心
	- 束腰直径等于光纤模场直径
### 计算模场直径（MFD）
查得所用光纤 [PMC780-APC](https://www.lbtek.com/product/471.html) 模场直径为 5.3 um±1.0 um@850 nm，但我们的工作波长为 811 nm。

对于 step-index single-mode fibers，存在近似公式
$$\text{MFD} = 2a(0.65+\frac{1.619}{V^{3/2}}+\frac{2.879}{V^6})$$
其中 a 为纤芯半径，V 为光纤所能容纳模式数
$$V = \frac{2 \pi a}{\lambda} \text{NA}$$

查得所用光纤纤芯直径为 4.5 um，NA 为 0.12，算得 V = 2.10，再代入 [Calculator for the Mode Radius of a Step-index Fiber - RP photonics](https://www.rp-photonics.com/mode_radius.html) 提供的计算器，可以算得模场半径为 2.74 um。
### 计算柱面镜位置
使用软件 [GaussianBeamVisualSimulation](https://gaussianbeam.sourceforge.net/)。
#### 竖直方向
![](/images/科研/LensVertical.png)
使用小焦距透镜（聚焦能力强）聚焦发散程度大的竖直方向高斯光束，使其近似成为平行光。再在耦合头处放置透镜使其耦合到光纤。

软件上，初始光束发散角为 28°（244 mrad）输入到 Divergence 栏（发散全角）。选择 f=4.51 mm 透镜，令透镜与光斑束腰的距离为焦距，实现很好的平行光。此时光斑半径为 1.13 mm。耦合头处，选择 f=12 mm 的透镜能很好地满足束腰半径等于模场半径的要求，且束腰位于光纤端面。
#### 水平方向
![](/images/科研/LensHorizontal.png)
选择强聚焦透镜使得发散相对不大的水平方向过度聚焦，需要放置只对一个方向有聚焦效果的柱面镜使其成为平行光。

软件上，注意存在 astigmatism，代入竖直方向已经得到的参数，选择 f=50.38 mm 的柱面镜，精心改变柱面镜位置，可以得到半径 0.94 mm的光斑，此时柱面镜距离上一个透镜 82.84 mm。

### 在模块里放置柱面镜

{{< image src="/images/科研/TA.png" width=450px caption="模块构型示意图">}}

柱面镜的 z 向移动范围为前后 3 mm，常态为套筒拉出底座 3mm。

已算得 x~82.84 mm，而小铜壳长度 a=15 mm，小铜壳外表面与壳底承距离 b=3.1 mm，柱面镜离其底座螺纹 c=14 mm（柱面镜拉出底座3mm）。假设工作状态下透镜紧贴小铜壳底面，有 
$$
y = x - a - b + c \approx 78 mm
$$

软件调整发现，$x \in (78, 90)$  均为较好的平行光，故 $y \in (74,  86)$。柱面镜的可调节范围为 6 mm，最终以 78 mm 为中心间隔 6 mm 阵列多排螺纹孔。
## 设计放置芯片的铜壳
### 材料选择
以下为各种材料的性能。

| 材料 | 黄铜       | 无氧铜         | 不锈钢 |
| ---- | ---------- | -------------- | ------ |
| 优点 | 易加工     | 导热性能好     | 硬     |
| 缺点 | 导热性能差 | 易氧化，导致地线不连接；难加工 | 高精度加工难     |

TA 芯片发热严重，需要温控，因此包裹它的铜壳材料为无氧铜。

在以前的设计中，由于透镜外壳为不锈钢，承载它的螺柱选用不锈钢。改变透镜距离需要在不锈钢上做出精细的螺纹，一般的加工厂做不好。不锈钢旋进无氧铜的过程中由于螺纹不匹配，会产生金属屑。此时若再用力旋入，就会卡死；需要旋出不锈钢，清理干净金属屑再旋入。

为了避免上述问题，新设计采用黄铜螺柱，螺柱旋入一个黄铜底座后再固定在无氧铜铜壳。黄铜是易于精密加工的材料，例如手表中的核心机械部件就是黄铜材料。
### 设计细节
黄铜底座留有上下左右移动距离，弥补透镜芯片光心位置差异。

黄铜螺柱需要保证透镜能在其最底部，因为透镜焦距小，芯片需要在透镜焦距范围内。因为非球面镜的实际焦距并非其参数标识，其实际焦距称为工作距离，焦距范围要按工作距离来确定。黄铜螺柱的外螺纹为光学螺纹，保证能精密调节透镜。
