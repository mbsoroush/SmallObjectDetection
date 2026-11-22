# 📘 Small Object Detection 

![Python](https://img.shields.io/badge/Python-3.10-blue)
![Detectron2](https://img.shields.io/badge/Detectron2-0.6+-orange)
![DeepLearning](https://img.shields.io/badge/Deep_Learning-Computer_Vision-green)

---

## 🔰 Overview
This project focuses on **detecting small objects in images** using **deep learning** techniques with [Detectron2](https://github.com/facebookresearch/detectron2).  
Small object detection remains a challenging task due to scale variation, occlusion, and limited pixel information — yet it’s essential for:

- 🚗 Traffic and surveillance analysis  
- 🛰️ Aerial and satellite imaging  
- 🩺 Medical image interpretation  
- 🤖 Autonomous vehicles and robotics  

The notebook walks through all key stages: **dataset preparation**, **model configuration**, **training**, and **evaluation**.

---

## 📦 Dataset
This project is dataset-agnostic — any **COCO-style annotated dataset** with small object instances can be used.

Example dataset structure:

    dataset/
    ├── train/
    │   ├── images/
    │   └── annotations.json
    ├── val/
    │   ├── images/
    │   └── annotations.json

Each annotation file follows the **COCO format**, including bounding boxes, segmentation masks, and class labels.

---

## ⚙️ Installation
Clone the repository and install the required dependencies:

    git clone https://github.com/your-username/small-object-detection.git
    cd small-object-detection
    pip install -r requirements.txt

Or install manually:

    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
    pip install detectron2 opencv-python matplotlib tqdm

---

## 🚀 Usage
To train and evaluate the model, open the notebook:

    jupyter notebook small-object-detection.ipynb

Or, if converted to a Python script:

    python train_detectron2.py

---

## 🧹 Data Preprocessing
Before training, the input data is preprocessed to ensure quality and consistency:

### ✅ Dataset Validation
- Verify image–annotation alignment  
- Remove empty or corrupted samples  

### ✅ Resizing and Normalization
- Scale images while maintaining aspect ratio  
- Normalize pixel intensity for Detectron2’s input format  

### ✅ Annotation Conversion
- Convert to COCO JSON format  
- Verify bounding box and category consistency  

---

## 🧠 Model Configuration & Training
Training is performed using **Detectron2**, a Facebook AI Research (FAIR) framework.

### 🧩 Model Selection
- Base architecture: `faster_rcnn_R_50_FPN_3x` (ResNet-50 backbone with FPN)  
- Pretrained weights from COCO used for transfer learning  

### ⚙️ Training Parameters
- Learning rate: `0.00025`  
- Batch size: `4`  
- Iterations: `~5000`  
- Augmentation: random flips and resizing  

### 📏 Evaluation Metrics
- mAP (mean Average Precision) across IoU thresholds  
- Separate evaluation for small, medium, and large objects  

### 💾 Output Checkpoints
- Model checkpoints saved in `/output` after every epoch  
- Final model stored as `model_final.pth`  

---

## 📊 Results & Visualization
After training, performance is analyzed both quantitatively and visually.

### 📈 Evaluation Metrics
- Example mAP (small objects): ~0.35  
- Example mAP (medium/large): ~0.60  

### 🖼️ Detection Visualization
Bounding boxes and labels plotted over sample images.  
Model predictions compared to ground truth for validation.

Example output visualization:

    from detectron2.utils.visualizer import Visualizer
    visualizer = Visualizer(image[:, :, ::-1], metadata=metadata)
    out = visualizer.draw_instance_predictions(outputs["instances"].to("cpu"))
    plt.imshow(out.get_image()[:, :, ::-1])

---

## 🧪 Troubleshooting
Common issues and fixes:

### ❌ CUDA Out of Memory
- Reduce batch size or image resolution  

### ❌ Dataset Registration Error
- Ensure correct paths and annotation file format in `DatasetCatalog.register()`  

### ❌ Low Detection on Small Objects
- Use Feature Pyramid Network (FPN)  
- Apply higher input resolution  
- Consider multi-scale training  

---

## 🚀 Deployment Options
You can deploy the trained model using:

### ✅ Local Inference Script
Run inference on an image locally:

    python inference.py --image path/to/image.jpg --model output/model_final.pth

### ✅ Web or API Deployment
- Use **Flask** or **FastAPI** for REST inference  
- Convert to **ONNX** or **TorchScript** for optimized runtime  

---

## 📦 Deliverables
This repository includes:

- ✅ Jupyter notebook → `small-object-detection.ipynb`  
- ✅ Model checkpoints → `/output/model_final.pth`  
- ✅ Visualization samples → `/visuals/`  
- ✅ Config and training logs → `/configs/`  
- ✅ Requirements file → `requirements.txt`  

---

## 🏁 Conclusion

### ✅ Objective Achieved
- Successfully trained a small-object detector using **Detectron2**  

### ✅ Key Features
- Adaptable to any **COCO-style** dataset  
- Visual evaluation and **mAP** metrics integrated  

### ✅ Impact
- Enhances object detection for low-visibility and small-scale targets  
- Useful for applications in **surveillance**, **remote sensing**, and **medical imaging**  

### ✅ Future Work
- Experiment with custom backbones (e.g., **Swin Transformer**, **ConvNeXt**)  
- Explore attention-based architectures for improved small object recall  
- Optimize inference speed with **quantization** or **pruning**  

---

## ⚖️ License

This work is licensed under a [Creative Commons Attribution–NonCommercial–NoDerivatives 4.0 International License](https://creativecommons.org/licenses/by-nc-nd/4.0/).  
Use in CVs, portfolios, or derivative works is **not permitted without explicit permission** from the author.





















