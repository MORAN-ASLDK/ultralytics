# CSMS-YOLO

CSMS-YOLO 是我们提出的一种基于 YOLOv11n 基线的目标检测模型。它专门用于应对水下目标检测的挑战，包括高噪声水平、低可见度和边缘模糊。该架构融合了四项关键创新：用于保留多尺度信息的 CSP-PMSFA 模块、用于频域滤波的 C2PSA-EDFFN 模块、用于深度监督的 DetectAux 模块，以及用于精确边界框回归的 D-InterpIoU 损失函数。该模型已在 URPC 和 UTDAC 数据集上进行了广泛验证。在参数量和计算复杂度仅略有增加的情况下，其检测精度显著高于基线 YOLOv11n 模型，同时保持了实时的处理速度。

在 URPC 和 UTDAC 数据集上，我们均未使用预训练权重，模型从头开始训练了 300 个 epoch。训练是在配备 RTX 3090 Ti GPU 的 Ubuntu 20.04 系统上进行的。

其性能指标如下：

| Datasets | size(pixels) | mAPval50:95 | mAPval50 | Precision | Recall | GFLOPs | params(M) | FPS |
| :------- | :----------- | :---------- | :------- | :-------- | :----- | :----- | :-------- | :-- |
| URPC     | 640          | 50.9        | 86.0     | 81.9      | 79.8   | 7.7    | 2.67      | 301 |
| UTDAC    | 640          | 50.2        | 84.3     | 82.9      | 76.9   | 7.7    | 2.67      | 307 |



## <div align="center"></div>

有关训练、测试和部署的完整文档，请参考以下快速入门示例。

克隆仓库，并确保您的环境中安装了 [**Python>=3.8.0**](https://www.python.org/)。此外，安装 Ultralytics 框架所需的依赖项，并确保您已安装 [**PyTorch>=1.8**](https://pytorch.org/get-started/locally/)。

```bash
git clone [https://github.com/MORAN-ASLDK/ultralytics.git](https://github.com/MORAN-ASLDK/ultralytics.git)
cd ultralytics

# 安装 YOLOv11 所需的标准依赖项
pip install -r requirements.txt
```
请注意，URPC 和 UTDAC 数据集应放置在与您工作目录同级的 datasets 目录中。
```bash
# 导航到项目目录并运行以下代码以开始训练。
# 按照我们的实验设置，batch size 设置为 64。如果您的显存不足，可以相应地减小 batch size。
python train.py --img 640 --batch 64 --epochs 300
```

验证模型
```bash
# 训练完成后，会在 runs/train 目录下生成一个包含所有训练结果文件的 exp 文件夹。可以使用 weights 文件夹中的 best.pt 文件来调用验证脚本：
python val.py --weights runs/train/exp/weights/best.pt --img 640
```
图像检测
```bash
# 利用训练好的权重对水下图像进行推理:
python detect.py --weights runs/train/exp/weights/best.pt --source your_image_folder/  # Generate images with detection results in the corresponding runs/detect/ folder.
```
