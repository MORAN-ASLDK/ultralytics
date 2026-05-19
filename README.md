# CSMS-YOLO

# Real-Time Lightweight Underwater Object Detector via Cross-Stage Multi-Scale Feature Aggregation

CSMS-YOLO is a target detection model we proposed based on the YOLOv11n baseline. It is explicitly designed to tackle underwater object detection challenges, including high noise levels, low visibility, and blurred edges. The architecture integrates four key innovations: the CSP-PMSFA module to preserve multi-scale information, the C2PSA-EDFFN module for frequency-domain filtering, the DetectAux module for deep supervision, and the D-InterpIoU loss for accurate bounding box regression. It has been extensively validated on the URPC and UTDAC datasets. With only slight increases in parameters and computational complexity, its detection accuracy is significantly higher than that of the baseline YOLOv11n model while maintaining real-time processing speeds.

On both the URPC and UTDAC datasets, no pre-trained weights were used, and the model was trained from scratch for 300 epochs. The training was conducted on an Ubuntu 20.04 system equipped with an RTX 3090 Ti GPU.

Its performance metrics are as follows:

| Datasets | size(pixels) | mAPval50:95 | mAPval50 | Precision | Recall | GFLOPs | params(M) | FPS |
| :------- | :----------- | :---------- | :------- | :-------- | :----- | :----- | :-------- | :-- |
| URPC     | 640          | 50.9        | 86.0     | 81.9      | 79.8   | 7.7    | 2.67      | 301 |
| UTDAC    | 640          | 50.2        | 84.3     | 82.9      | 76.9   | 7.7    | 2.67      | 307 |



## <div align="center"></div>

For complete documentation on training, testing, and deployment, please refer to the quickstart examples below.

Clone repo, and ensure that [**Python>=3.8.0**](https://www.python.org/) is installed in your environment. Additionally, install the dependencies required by the Ultralytics framework, and ensure that you have [**PyTorch>=1.8**](https://pytorch.org/get-started/locally/) installed.

```bash
git clone [https://github.com/MORAN-ASLDK/ultralytics.git](https://github.com/MORAN-ASLDK/ultralytics.git)
cd ultralytics

# Install standard dependencies required by YOLOv11
pip install -r requirements.txt
```
Note that the URPC and UTDAC datasets should be placed in the datasets directory at the same level as your working directory.
```bash
# Navigate to the project directory and run the following code to start training.
# Following our experimental setup, the batch size is set to 64. If you have insufficient GPU memory, you can adjust the batch size accordingly.
# Note: Before training, please ensure that you update the data parameter in train.py to point to the correct relative or absolute path of your dataset's data.yaml file.
python train.py --img 640 --batch 64 --epochs 300
```

Validate the module
```bash
# After training, an exp folder containing all the training result files will be created in the runs/train directory. The best.pt file in the weights folder can be used to call the validation script:
python val.py --weights runs/train/exp/weights/best.pt --img 640
```
Detect the pictures
```bash
# Utilizing the trained weights to perform inference on underwater images:
python detect.py --weights runs/train/exp/weights/best.pt --source your_image_folder/  # Generate images with detection results in the corresponding runs/detect/ folder.
```
