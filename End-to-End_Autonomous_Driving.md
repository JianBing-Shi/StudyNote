# 端到端自动驾驶
上一代算法架构：**Data** -> **Perception** -> **Planner**  

上一代算法架构的缺陷：
* 感知不准
* 规控不拟人，覆盖不了长尾问题
* 感知-规控的衔接有信息损耗

端到端算法架构：**Data** -> **End2End(Planner)** （既干感知算法又干规控算法）   

多模态大语言模型的兴起，让人们看到信息大融合带来的无限创造力

四大分支方向：
1. MLLM（多模态大语言模型）
2. 世界模型
3. VLA（Vision Language Action Model）
4. 扩散模型

## 端到端算法分类
### 两段式端到端
算法架构：**Data** -> **Perception** -> **Planner**  
* 传统算法框架到端到端算法的过渡阶段
* Planner模型化，解决线性planner重度依赖规则的问题（让Planner变得更强，能逐渐把Perception融入Planner）
* 仍具有现行架构的感知不准、信息损耗等问题
* 适合新手入门
### 一段式端到端
算法架构：**Data** -> **End2End(Planner)**
* 基于MLLM、世界模型已有产品级交付案例
* 基于VLA、扩散模型的技术仍在探索阶段
#### 基于MLLM的一段式端到端
基于MLLM的端到端，也叫做基于**感知**的一段式端到端模型。  
* **PV感知**（透视图）：透视图模型则提供了一个更接近人类驾驶员视角的视图，它模拟了车辆前方的视角，有助于车辆识别和理解道路上的交通标志、行人和其他车辆。
* **BEV感知**（鸟瞰图）：鸟瞰视图模型提供了一个从上方观察车辆及其周围环境的视角。这种视角有助于车辆更好地理解交通流和车辆的位置关系，常用于路径规划和避障。
* **BEV Former** VS **Transformer** ：**BEVFormer**将*BEV感知*和*Transformer*技术融合，通过提取环视相机采集到的图像特征，并将提取的环视特征通过模型学习的方式转换到 BEV 空间（模型去学习如何将特征从 图像坐标系转换到 BEV 坐标系），从而实现 3D 目标检测和地图分割任务，并取得了 SOTA 的效果。
* **一段式端到端 UniAD**  

~~MLLM + 自动驾驶 $\neq$ 问答式自动驾驶~~
#### 基于世界模型的一段式端到端
世界模型（World Model）  

Sim2Real  
动作：Agent -> Virtual Environment(世界模型)  
反馈：Virtual Environment(世界模型) -> Agent

世界模型的争议点
* 感知世界VS预测未来
* 模拟现实VS做出决策

世界模型的流派众多
* 只做仿真的世界模型
* 仿真和Planner一起做的世界模型

世界模型和planner模型怎么配合？
* 当前环境 -> Planner -> 决策 -> 世界模型 -> 导致什么/遇到什么 -> Planner

#### 基于VLA的一段式端到端
VLA借助视觉大语言模型的能力，让**自然语言控制**（开放的命令）引入**感知算法**（模型能力强大），执行**决策算法**（实现开放的决策空间，不被人定义）  
VLM + Action = VLA   
自动驾驶中的VLA：**DriveMoE**

VLA的优点
* 参数量打，涌现能力强，擅长“人-车-环境”的多方、多模态交互
* 与MLLM没有本质区别，重点在最后输出的是action，而非轨迹点
* 更类人，更一统，更加的端到端

#### 基于扩散模型的一段式端到端
主要工作在自动驾驶的轨迹生成方案。使用了Diffusion Model，将一段初始化为噪声的凌乱轨迹，通过逐层去噪，得到清晰、符合要求的轨迹。

## 端到端开源数据集
### NuScenes
#### NuPlan：Planner
* 1200小时的真实驾驶数据
* 有人工标注的地图信息（道路、车道线等）
* 用AI对目标框进行了自动标注
* 提供开环、闭环仿真工具
#### NuImages：2D感知
* 93k视频，折合150小时的驾驶
* 93k带标注的图片和1100k无标注的图片
#### NuScenes：3D多模态感知
* 几乎涵盖了所有感知任务
### Bench2Drive
* 13638个视频，2000k完整标注的图片
* 测试集包含了220段路线，每段长度约150米，场景各不相同，涵盖44中驾驶技巧
* 提供了开环、闭环测试工具
* 可以进行传感器仿真
### 其他数据集
* Kitti、BDD
* Open Occupancy
* OpenAD

## 自动驾驶仿真器
**Carla Simulator**：可用于闭环测试，模拟自动驾驶的几乎每个环节（感知、定位、规控），可自行选择环境，可以选择周围出现车辆、行人、交通标志、红绿灯等，车辆还可选择行驶风格、车速等，可选择自车安装的传感器，系统会为每种传感器都模拟采集到的数据，可导出作为数据集。

## 端到端算法的评价指标
### 开环评测
只被动播放数据，环境不响应模型规划的动作
* L2 distance：模型规划轨迹与GT轨迹
* 碰撞率：与环境障碍物碰撞的比例
### 闭环评测
需要搭建仿真系统，环境相应模型动作
* Route Complete（RC）：路线完成率
* Infraction Score（IS）：违规率
* Drive Score（DS）：上述二者乘积
### 实车评测
最真实，最重要
* 接管率
* 舒适率：速度平滑性、转弯平滑性
* 安全性：与环境障碍物的最小距离
* 效率
* S-CR：安全分数
* S-Area：可行驶区域保持分数
* S-PR：进度分数
* S-Comfort：舒适度分数

## 大语言模型LLM
**三大核心技术**
1. Transformer类架构（⭐）
2. 海量数据预训练
3. 微调精调技术（⭐）

### Transformer
### ViT：Viston Transformer
### CLIP：Contrastive Language-Image Pre-training
zero-shot
### LLaVA：Visual Instruction Tuning

## PointNet
PointNet是一个处理点云数据的神经网络架构，专门设计用于处理无序的点云数据。它通过对每个点进行独立的特征提取，并使用对称函数（如max pooling）来聚合这些特征，从而实现对整个点云的全局特征表示。PointNet在3D物体分类、分割和场景理解等任务中表现出色，成为点云处理领域的重要基石。

点云数据是无序性的，只要位置不变，任意变换顺序，数据不改变；图像数据是有序性的，图像的像素变换位置，图像信息就变了。

PointNet的核心是MLP + Max Pooling，MLP用于编码信息，Max Pooling用于忽略顺序。

## BEV感知
BEV：Bird-Eye-View，鸟瞰视角/俯视视角。当前主流自动驾驶感知、规控技术都应用在该视角下
PV：Perspective View，每个相机上单独做感知，检出结果都是对应相机上的图像坐标信息
PV->BEV：在图片上感知，得到目标坐标点，再经坐标变换，投射到BEV视角
* 2D检测结果，投射后是一篇扇形区域
* 3D检测结果，投射后仍是一个3D框
### 空间坐标系
规定使用**右手原则**
* 世界坐标系
* 车辆坐标系
* 传感器坐标系

  每个传感器在车上的安装位置是固定的，相对位置可以用一个旋转+一个平移来表达

  所有坐标变换都能写成 **\[下一个坐标\] = \[上一个坐标\] x \[变换矩阵\]** 来表达  

  变换矩阵设计的所有参数统称为标定参数
  * 内参矩阵：传感器内部的坐标变换参数，如焦距、像素高宽等
  * 外参矩阵：传感器与外部其他传感器、世界坐标之间的变换参数，如相机安装位置、角度
#### 相机坐标系（相机）  
* 像素坐标系 -> 图像坐标系 -> 相机坐标系

### BEVFormer
BEVFormer，乃至端到端算法的里程碑   
![BEVFormer](./Img/End-to-End_Autonomous_Driving/BEVFormer.png)

基于BEV的Transformer，重心在于如何处理BEV特征，可接下游任何的感知任务

后续工作开发出了轨迹规划任务，即成为端到端划时代之作UniAD

#### BEVFormer内容
* 多camera输入和特征提取：使用backbone模型（VGG、ResNet、ViT）得到图像特征
* Spatial Cross-Attention(空间交叉注意力)：从每个相机的图像特征中提取**BEV特征**，做Cross-Attention
	* BEV视角下，将自车前后左右一定长宽范围内的2D空间预设分辨率，划分成2D栅格，栅格总数记为 *M* x *N*
	* 每个栅栅格(x, y)，预设一组固定的高度 z1, z2, ……
	* 每个三位位置点（x, y, z），通过坐标变换，映射到相机上，每个点可能映射到多个相机上	*(和3dgs好像，从3D点云投影映射到2D像素)*
	* 在所有映射到的相机位置，随机提取附近的图像特征
	* 对每个相机vi，将采集到的所有图像特征，进行加权平均，作为（x, y, zi）在相机vi采集到的特征，记为f(x, y, zi, vi)
	* 由于相机的图正图像维度是D，总共有 *M* x *N* 个(x, y)栅格，故经过这种采样后，提取到的特征维度为 *M* x *N* x *D*
	* 将提取到的BEV特征，变换token实现多模态对齐，进行Cross-Attention
* Temporal Self-Attention(时间自注意力)
	* 上一刻的BEV特征 $B_{t-1}$ ，先根据自车移动向量，对齐到当前时刻 $B_{t-1}^{'}$ 
	* 当前时刻的BEV Query，自己与自己做deformable self-attn
	* 特别的，deformable attn中的偏移量，改为由 Q 与$B_{t-1}^{'}$ 的concat后，再用一个小网络预测得到

使用 BEVFormer 可以进行目标检测，实例分割，栅格地图的任务

（之间的模型：LSS（Lift-Splat-Shoot）
（之后的工作：Lane Detection：MapTR，建立BEV特征，建立Query，Cross Attention建立特征，Head输出，监督训练）

BEVFormer：BEV感知+Transformer结合的代表作
BEV感知的基本架构：传感器 -> 基础backbone提取特征 -> 构建BEV特征 -> 构造query -> 构造head和loss

### Diffusion Model扩散模型
扩散模型本质是 **加噪过程和去噪过程** 。

`Denoising Diffusion Probabilistic Models`（NIPS 2020）是Diffusion Model开山制作，一句撼动了GAN在图像生成领域的霸主地位。
训练一个模型，输入为噪声图像和step数量，预测噪声的参数  

![Denoising Diffusion Probabilistic Models](./Img/End-to-End_Autonomous_Driving/DenoisingDiffusionProbabilisticModels.png)

生成阶段
1. 给定一个纯噪声图和初始step数
2. 用模型根据输入图和step数，预测噪声的参数
3. 将噪声图减去用参数生成的噪声，得到去噪图
4. step数-1
5. 将去噪后的图像和更新后的step数再次输入模型，重复2~5步骤

假定噪声符合高斯分布，由此得到递推公式 $ x_{t} = \sqrt{a_{t}} * x_{t-1} + \sqrt{1-a_{t}} * \epsilon_{t} $ ，其中 $ x_{t} $ 为当前图像，$ x_{t-1} $ 为上一张图像，$ \epsilon_{t} $ 为对上一张图像加入的噪声。  
最终，通项公式为 $ x_{t} = \sqrt{\hat{a_{t}}} * x_{0} + \sqrt{1-\hat{a_{t}}} * \epsilon_{t} $ , 其中 $ \hat{a_{t}} = a_{1} * a_{2} * …… * a_{t}$

引入时间编码嵌入，让扩散模型知道自己在生成过程的哪个阶段，从而做出正确的决策。为模型提供了上下文信息，明白了当前处于去噪过程的哪个阶段。

扩散模型最终预测噪声，相比于预测去噪后的图像效果更好。这是因为在加噪过程中，图像逐渐被噪声淹没，图像几乎全是噪声，预测干净图像是一件非常困难的事情。预测噪声，由于噪声符合高斯分布，模型在每一个时间步的任务都是一致的，预测一个来自标准高斯分布的噪声，这降低了模型的学习难度。

### Reinforcement Learning强化学习
|  | 监督学习 | 强化学习 |
| ------- | ------- | ------- |
| 训练真值 | 标签 | 环境 | 
| 优化目标 | 最小化损失 | 最大化reward |
| 优化策略 | 梯度下降 | 梯度上升 | 

如果你能准确描述正确答案，推荐使用监督学习进行训练；  
如果你不能准确描述正确答案，但直到正确答案有哪些特点，错误答案有哪些特点，推荐使用强化学习进行训练

强化学习中，以Agent智能体为强化学习的训练目标，由`环境`、`动作`、`状态`、`奖励`组成

策略函数，根据状态 $s$ ，描述动作$a$的概率分布的函数  
$ \pi(a|s) = P(A=a|S=s) $

状态转移函数，根据状态 $ s_{t} $ ，当采取了动作 $ a_{t} $ 后，描述下一个状态 $ s_{t+1} $ 的概率分布的函数  
$ p(s_{t+1}|s_{t}, a_{t}) = P(S_{t+1}=s_{t+1}|S=s_{t}, A=a_{t}) $

动作价值函数，已知策略函数$ \pi $、状态$ s_{t} $、采取的动作$ a_{t} $，描述该动作所获的**奖励**期望  
$ Q_{\pi}(s_{t}, a_{t}) = r(s_{t}, a_{t}) + \gamma \sum_{s_{t+1} \in S} p(s_{t+1}|s_{t}, a_{t}) \sum_{a_{t+1} \in A} \pi(a_{t+1}|s_{t+1})Q_{\pi}(s_{t+1}, a_{t+1}) $

## 两段式端到端
算法架构：**Data** -> **Perception** -> **Planner**   
* 传统算法框架到端到端算法的过渡阶段
* Planner模型化，解决线性planner重度依赖规则的问题（让Planner变得更强，能逐渐把Perception融入Planner）
* 仍具有现行架构的感知不准、信息损耗等问题
* 适合新手入门

### 港科大PLUTO:Pushing the Limits of Imitation Learning-based Planning  for Autonomous Driving
AI模型在nuPlan首次击败基于规则的planner PDM（PDM：2023 nuPlan planning challenge冠军）

此前AI方案的弱项
* 不擅长做横向行为建模（横向行为是方向盘的方向控制，纵向行为是车辆加速度）
* 开环评测与闭环评测结论差异大
* 总学到short-cut，混稀因果（例如跟车情况下，对红绿灯判断弱，导致前方车辆卡绿灯通过，车辆依旧跟车通过了红灯）

Query-based网络结构
* 对横向、纵向规划单独建模
* 在构建query的过程中将横向、纵向融合，增强模型同时规划横纵向行为的能力

设计辅助loss
* 提出了一种差分插值辅助loss
* 引入多项辅助训练任务，且对batch-wise运算友好，便于大规模应用于当代AI模型

数据增强
* 扰动，dropout，insertion
* 用以缩小开环评测与闭环评测的差异

对比学习
* 与数据增强配合使用，引入对比损失
* 用以抑制模型学习short-cut

#### 网络模型结构
![PLUTO 网络模型结构](./Img/End-to-End_Autonomous_Driving/PLUTO.png)

* $E_{A}$：动态物体编码器
* $E_{O}$：静态物体编码器
* $E_{P}$：地图元素编码器
* $E_{AV}$: 自车状态编码器
* $Logitudinal Query$：自车轨迹预测

* $Polyline Encoder$：自车轨迹编码器
* $Perdictions$：预测动态物体轨迹 
* $Lateral$：横向规划
* $Logitudinal$：纵向规划
* $Trajectory$ & $Score$：轨迹规划 & 得分

#### **Perception** 感知模块
$E_{A}$： 动态物体编码器（FPN编码器）
1. 当前动态物体状态 

	$s_{i}^{t} = (p_{i}^{t}, \theta_{i}^{t}, v_{i}^{t}, b_{i}^{t}, \mathbb{I}_{i}^{t})$

	* $p_{i}^{t}$：第i个动态物体在t时刻的位置坐标
	* $\theta_{i}^{t}$：第i个动态物体在t时刻的朝向
	* $v_{i}^{t}$：第i个动态物体在t时刻的速度
	* $b_{i}^{t}$：第i个动态物体在t时刻的bbox信息
	* $\mathbb{I}_{i}^{t}$：第i个动态物体在t时刻的可见状态

2. 历史动态物体状态

	$\hat{s}_{i}^{t} = (p_{i}^{t} - p_{i}^{t-1}, \theta_{i}^{t} - \theta_{i}^{t-1}, v_{i}^{t} - v_{i}^{t-1}, b_{i}^{t}, \mathbb{I}_{i}^{t})$

$E_{O}$： 静态物体编码器（MLP编码器）
1. 当前动态物体状态

	$o_{i} = (p_{i}, \theta_{i}, b_{i})$

	* $p_{i}$：第i个静态物体的位置坐标
	* $\theta_{i}$：第i个静态物体的朝向
	* $b_{i}$：第i个静态物体的bbox信息

$E_{P}$：地图元素编码器（PointNet编码器）
1. 当前地图元素状态

	若干组polylines

	$(p_{i} - p_{0}, p_{i} - p_{i-1}, p_{i} - p_{i}^{left}, p_{i} - p_{i}^{right})$

	* $p_{0}$：自车起始点
	* $p_{i}^{right}$：最左边的点

$E_{AV}$: 自车状态编码器
	
当前时刻的位置、朝向、速度、加速度、转角

不使用历史信息，防止ShortCut

State dropout encoder，仅取消自车历史信息，模型还是会被自车运动状态误导，SDE可消除这种误导

总特征**embedding** —— $E_{0} = concat(E_{A}, E_{O}, E_{P}, E_{AV}) + PE（全局位置编码） + E_{attr}（属性编码，agent类型、限速、交通灯状态）$

#### **Planner** 规划模块
轨迹规划
  * 纵向规划：车速
  * 横向规划：线型（直行/左变道/右变道/左转/右转）

纵向：初始化一组可学习的Query

横向：用传统路径规划方法，获得一组参考线，用PointNet编码

在横向和纵向Query上做自注意力，希望跟输入编码的KV交互时，Query能综合分析所有横纵向情况，做出更合理的决策。

如果将$N_{1}$个横向Query和$N_{2}$个纵向Query进行融合，所有的注意力计算需要$N_{1}^{2}*N_{2}^{2}$次。

先对每个纵向Query，做横向注意力，$N_{2}$个纵向Query， 每个纵向Query需要$N_{1}^{2}$次注意力计算；
再对每个横向Query，做纵向注意力，$N_{1}$个横向Query，每个横向Query需要$N_{2}^{2}$次注意力计算$；
总计算量为$N_{1}^{2}*N_{2} + N_{1}*N_{2}^{2}$，相比于直接融合的$N_{1}^{2}*N_{2}^{2}$，大大降低了计算量。

轨迹预测使用了两个MLP，一个为每个轨迹模态预测具体轨迹点，另一个预测每个模态的打分，又添加一个MLP在不借助参考线的情况下，直接预测轨迹。

#### Loss设计
1. 主任务Loss
	* 轨迹损失：$\mathcal{L}_{reg} = L1_{smooth}(\hat{\pi} - \pi^{gt}) +  L1_{smooth}({\pi}^{free} - \pi^{gt})$
	* 分类损失：$\mathcal{L}_{cls} = CE({\pi}_{0}, \pi_{0}^{*})$
2. 模仿学习Loss
	* $\mathcal{L}_{i} = \mathcal{L}_{reg} + \mathcal{L}_{cls}$
3. Agent轨迹预测Loss（辅助任务）
	* $P_{1:N_{A}} = MLP(E_{A}^{'})$
	* $\mathcal{L}_{pred} = L1_{smooth}(P_{1:N_{A}} - P_{1:N_{A}}^{gt})$

加入辅助任务的Loss，能让模型更好地理解环境，提升规划能力。纯模仿学习难以让模型学习到真实场景的驾驶约束。

在训练数据集样本中，99%的数据样本是没有碰撞的，模型难以学习到碰撞的特征，数据集样本中的负样本是非常稀少的。

因此，**PLUTO** 提出了ESDF（Euclidean Signed Distance Field），在ESDF描述了鸟瞰图下可行驶区域，每个点距离其最近不可行驶区域的欧式距离。

4. 可行使区域辅助Loss
	* $\mathcal{L}_{aux} = \frac{1}{T_{f}} \sum\limits_{i=1}^{T_f} \sum\limits_{i=1}^{N_c} \max(0, R_{c} + \epsilon - d_{i}^{t})$

用n个圆覆盖住车身，获取每个圆的圆心的ESDF值，如果ESDF值 < 圆半径，即可能驶离了可行驶区域，给予惩罚

5. 对比学习Loss
	* $\mathcal{L}_{contrastive} = - \log \frac{\exp(sim(z, z^{+})/ \sigma)}{\exp(sim(z, z^{+})/ \sigma) + \exp(sim(z, z^{-})/ \sigma)}$

* 最终Loss：
	* $\mathcal{L} = \omega_{1}\mathcal{L}_{i} + \omega_{2}\mathcal{L}_{pred} + \omega_{3}\mathcal{L}_{aux}$ + $\omega_{4}\mathcal{L}_{contrastive}$

后处理：模型推理出的轨迹有两个打分，第一个是模型自己给每个模态的打分；第二个是用LQR+自行车模型，对轨迹进行处理，处理后的轨迹根据完成度、舒服度、碰撞、交规等因素进行打分。两个打分做加权平均，最高者为最终轨迹
#### 引用内容
* IDM：Intelligent Driver Model，一种经典的基于规则的微观交通流模型
* PDM-Closed：基于规则的planner，nuPlan planning challenge冠军，用IDM生成多个proposal，再用规则筛选
* Urban Driver：PMLR2022，比较早期的模仿学习工作
* PlanTF：ICRA2024，PLUTO作者自己的早期工作

### CarPlanner:Consistent Auto-regressive Trajectory Planning for Large-scale Reinforcement Learning in Autonomous Driving（CVPR 2025）
CarPlanner是一个基于强化学习的端到端自动驾驶规划模型，自回归的推理方式规划轨迹，朴素世界模型 + planner模型结合的方案。首篇在nuPlan超越模仿学习、规则方案的RL-based planner，且在闭环评测中表现优于模仿学习方案。

* 自回归推理
	* 没有自回归：忽略时序因果关系
	* Mode可变自回归：轨迹稳定性差
	* Mode不变自回归：本文方法

#### 网络模型结构
![CarPlanner 网络模型结构](./Img/End-to-End_Autonomous_Driving/CarPlanner.png)

输入Agent（含自车）
* 位置、朝向、速度、BBox、时间戳、类型
* $N（个数） * H（时间窗） * D_{a}$
* PointNet编码成 $N * D$ 

地图
* Polyline
	* Polyline表示，每个polyline有$N_{p}$个边，每个边有左边界、中点、右边界三个点，共$3N_{p}$个点
	* 每个点有$D_{m}$个特征：位置、朝向、类型、限速等
	* $N_{m1}$个polyline组成的初始特征维度为 $N_{m1} * 3N_{p} * D_{m}$，经过PointNet编码成 $N_{m1} * D$
* Polygon
	* 路口、停止线、横道线等
	* 类似的方式，经过PointNet编码成 $N_{m2 } * D$

Query
* 每个mode一条query
* 一个mode：一种横向选择+一种速度选择
* 横向、纵向兼顾
	* 纵向query
		* 一共$N_{lon}$种速度，$i / N_{lon}$ 为第 i 种的速度值
		* 速度值重复D次构成初始queryt向量，得到 $N_{lon} * D$ 的初始query特征维度
	* 横向query
		* 用图搜索（传统方法）在map上搜索出 $N_{lat} * D$ 条可行路径
	* 横纵query结合
		* 先将两组query两两concat，得到 $N_{lon} * N_{lat} * 2D$ 条mode的初始query特征维度
		* 再用MLP处理成 $N_{lon} * N_{lat} * D$ 的mode query特征维度

Decoder
* Agent、地图的特征作为KV，与Query进行注意力融合
#### **(Planner)**预测模块
Non-reactivate Transition Model（朴素世界模型）
* 一次性预测其他agent所有未来时刻的轨迹，文中表示观察到其他agent的预测，自回归推理和一次性推理效果差异不大

Mode Selector
* 在decoder后接一个MLP + Softmax，预测每种mode的打分
* Cross-Entropy Loss作为loss
* 添加一个回归任务预测自车轨迹，L1 Loss作为side-task loss

Trajectory Generator
* IVM(Invariant-view module)：预测自车轨迹时，先对其为轴的agent和map信息做KNN采样，采样得到的信息转换到自车在当前时刻下的坐标系，输入前文所述的网络
* 不同mode，IVM内容是不一同的，但维度一样，不影响并行计算
* Policy head
	* 预测自车动作
  	* 用高斯分布对自车动作建模，预测的时高斯分布的参数
  	* 训练时，动作用高斯采样得到
  	* 推理时，动作为高斯分布的均值
* Value head
	* 预测价值 ~ PPO
* 自回归推理
	* 当前时刻 -> 得到IVM -> 推理下个时刻 -> 得到下个时刻的IVM -> 推理下下个时刻 -> …… -> 得到整个轨迹
* Mode不变自回归
	* 训练过程中，在选择mode时，winner takes all，用gt匹配上的mode作为固定condition

其他设计
* Rule-augmented Selector：用模型给mode的打分以及一些主观评估标准（抵达、舒适、安全……）的加权筛选最终轨迹
* Reward function：碰撞or驶出可行驶区域为-1；否则为0
* Mode dropout：随机mask路径，应为有的场景可能搜不到或不存在可行路径，要破除模型的路径依赖
* 强化学习训练：PPO算法

### Plan-R1
## 一段式端到端
算法架构：**Data** -> **End2End(Planner)**

实际：仍显式地带有感知模块，只是感知和Planner模块是一同训练/端到端训练，因此都视为端到端
* 感知和Planner一同训练，有助于增强解释性，方便迭代调优

Planner的输出有两种形式
* 轨迹
* 驾驶动作

### 基于感知的端到端模型
对感知模型做改动、缝合