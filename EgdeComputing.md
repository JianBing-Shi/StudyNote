# 边缘计算 EgdeComputing
## TensorRT
*TensorRT*本质上是一个**推理优化与运行时系统**。它通常不直接读取服务器训练得到的`.pt`权重，而是将训练模型转换为推理图，再针对目标 GPU/边缘设备生成 TensorRT Engine。

TensorRT主要通过以下方式加速：
1. 算子融合。

		将多个算子合并成一个*GPU Kernel*，例如：`Conv + BatchNorm + ReLU`被融合成一个更高效的执行单元，减少显存读写和 Kernel 启动次数。

2. 低精度推理

		在边缘设备上，使用FP16、INT8、INT4，减少漩村占用，并利用Tensor Core提高吞吐量。

3. Kernel自动选择

		TensorRT会根据 *GPU架构、输入尺寸、BatchSize、精度、算子布局*，选择更高效的实现。

4. 内存优化

		TensorRT 会静态规划中间张量的显存，减少不必要的内存分配和复制。

5. 图优化

		包括常量折叠、删除无效节点、层融合和数据布局优化等。

6. 专用插件

		对 TensorRT 不原生支持的算子，可以通过 Plugin 自定义 CUDA 实现。
