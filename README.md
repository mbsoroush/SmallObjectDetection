# 📘 Small Object Detection with Detectron2

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
│ ├── images/
│ └── annotations.json
├── val/
│ ├── images/
│ └── annotations.json

Each annotation file follows the **COCO format**, including bounding boxes, segmentation masks, and class labels.

---

## ⚙️ Installation
Clone the repository and install the required dependencies:

```bash
git clone https://github.com/your-username/small-object-detection.git
cd small-object-detection
pip install -r requirements.txt
```

Or install manually:

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
pip install detectron2 opencv-python matplotlib tqdm
```

---

## 🚀 Usage

To train and evaluate the model, simply open and run the notebook:

```bash
jupyter notebook small-object-detection.ipynb
```

If you’ve converted the notebook into a Python script, you can also run it directly:

```bash
python train_detectron2.py
```

---

























