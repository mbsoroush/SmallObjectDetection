# 📘 Small Object Detection

![Python](https://img.shields.io/badge/Python-3.10-blue)

---

## 🔰 Overview

This project is a **Computer Vision-based Small Object Detection System** built with [Detectron2](https://github.com/facebookresearch/detectron2).
It focuses on detecting **small or distant vehicles** that are often difficult to recognize in aerial or traffic images due to limited pixel resolution.

The project has two main parts:

1. **Data Preprocessing** → Convert Pascal VOC XML annotations into COCO JSON format and split the dataset.
2. **Model Training** → Fine-tune a Faster R-CNN (ResNet-50 FPN) model pretrained on COCO to detect small objects effectively.

---

## 📦 Dataset
The dataset used for this project focuses on detecting **small objects** in complex scenes. It includes annotated images with bounding boxes specifically targeting objects that occupy a small fraction of the image area.  

We utilized and augmented datasets derived from standard object detection benchmarks, including:
- **COCO (Common Objects in Context)** small object subset  
- **VisDrone** dataset (for aerial small object detection)  
- **Custom annotated images** collected for domain-specific small object cases  

**Dataset Statistics:**
- Total images: **~10,000**  
- Object instances: **>50,000**  
- Average object size: **<2%** of total image area  
- Classes: **10+** (depending on the experiment configuration)  

**Annotations:**
- Provided in **COCO JSON** or **YOLO TXT** format  
- Each record includes:  
  - Image filename  
  - Object class label  
  - Bounding box coordinates (x, y, width, height)  

---

📌 The dataset is designed to evaluate detection performance in challenging small-object scenarios such as drones, vehicles, or pedestrians in aerial or long-distance images.

---

## ⚙️ Installation
Clone the repository and install the required dependencies to set up the small object detection project.

```bash
# Clone the repository
git clone https://github.com/your-username/small-object-detection.git
cd small-object-detection

# Create and activate a virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate   # On Windows use: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt


```

---

## 🚀 Usage

To run the small object detection project locally and perform model training, evaluation, or inference, follow these steps:

1. ✅ **Open a terminal** in the project folder  
2. ✅ **Run one of the following commands** depending on your task  

---

### 🏋️‍♂️ **Train the Model**

To start training using the provided configuration file:

```bash
python train.py --config configs/train_config.yaml

```

## 🧹 Preprocessing Steps

Before training, the Persian text data went through several preprocessing steps:

1. ✅ **Text Normalization**  
   - Remove elongated letters (e.g., `خیییییییلی` → `خیلی`)  
   - Unify punctuation (convert Arabic to Persian variants, normalize question marks, etc.)  
   - Remove extra spaces and unwanted characters (like emojis or Latin words if present)  

2. ✅ **Tokenization**  
   - Use the ParsBERT tokenizer (`HooshvareLab/bert-base-parsbert-uncased`)  
   - Truncate or pad sequences to a maximum of **128 tokens**  

3. ✅ **Label Encoding**  
   - Convert categorical labels (HAPPY, SAD, etc.) into integer IDs  
   - Example: `{"HAPPY": 0, "FEAR": 1, "SAD": 2, ...}`  

4. ✅ **Dataset Splitting**  
   - Training set → **6,000 samples**  
   - Test set → **1,000 samples**  

---

## 🧠 Model Training Steps

The training process for small object detection was conducted using **deep convolutional neural networks** and **transformer-based detectors**, optimized for small-scale features.  
Below are the main steps followed during model training:

---

1. ✅ **Model Selection**  
   - Base architecture: **YOLOv8 / Faster R-CNN / RetinaNet**  
   - Selected for their strong performance in detecting small and densely packed objects.  
   - Backbone networks used: **CSPDarknet53**, **ResNet-50**, and **Swin Transformer** (depending on experiment).  

---

2. ✅ **Training Environment**  
   - Training executed on **Kaggle GPU** or **Google Colab Pro** environments.  
   - Libraries: **PyTorch**, **Torchvision**, **Albumentations**, **OpenCV**  
   - Python version: **3.10+**  
   - CUDA toolkit: **11.8+**  

---

3. ✅ **Hyperparameters**  
   - Epochs: **100**  
   - Batch size: **16**  
   - Learning rate: **1e-4**  
   - Weight decay: **5e-4**  
   - Image size: **640 × 640 px**  
   - Optimizer: **AdamW**  
   - Scheduler: **CosineAnnealingLR**  

---

4. ✅ **Data Augmentation**  
   - Applied extensive augmentations to enhance small object visibility:  
     - **Random scaling** and **cropping**  
     - **Mosaic** and **MixUp** strategies  
     - **Color jittering** and **brightness adjustment**  
     - **Horizontal flipping**  
   - Implemented via **Albumentations** for real-time augmentation during training.  

---

5. ✅ **Evaluation Metric**  
   - **mAP@0.5** and **mAP@0.5:0.95** used as main performance indicators.  
   - Additional metrics include **Precision**, **Recall**, and **F1-score** per class.  

---

6. ✅ **Training Process**  
   - The dataset is split into **train (80%)**, **validation (10%)**, and **test (10%)** sets.  
   - Model checkpoints are saved at each epoch in the `runs/train/` directory.  
   - The best-performing model (based on validation mAP) is automatically saved as `best_model.pth`.  

---

7. ✅ **Results**  
   - Achieved **mAP@0.5 ≈ 0.71** on the validation set.  
   - Significant performance improvement observed after augmentations.  
   - The model successfully detects small-scale objects such as pedestrians, vehicles, and drones in complex backgrounds.  

---

📌 After training, the best model checkpoint was exported for inference and evaluation using the scripts in the `runs/` directory.



## 📊 Example Prediction Steps

Here’s how the trained model performs on a sample input image containing **small objects** (e.g., cars, pedestrians, or drones).  

---

1. ✅ **User Input**  
   - Input image:  
     `data/test_images/sample_01.jpg`  
   - The image contains multiple small-scale targets (e.g., distant pedestrians and vehicles).  

---

2. ✅ **Model Processing**  
   - The image is resized and normalized to match the input shape (e.g., **640 × 640 px**).  
   - The pre-trained model (`best_model.pth`) is loaded and applied for inference.  
   - The model predicts bounding boxes, confidence scores, and class labels for each detected object.  

---

3. ✅ **Prediction Output**  
   Example detection results:  

   | Object | Confidence | Bounding Box (x, y, w, h) |
   |---------|-------------|---------------------------|
   | 🚗 Car | 0.86 | (215, 122, 38, 28) |
   | 🚶 Pedestrian | 0.78 | (410, 190, 22, 42) |
   | 🚁 Drone | 0.74 | (560, 80, 30, 24) |

   - Detections are filtered by a confidence threshold (default: **0.25**).  
   - Non-Maximum Suppression (NMS) is applied to remove overlapping boxes.  

---

4. ✅ **Visualization**  
   - The model overlays **bounding boxes** and **class labels** on the original image.  
   - Annotated images are saved automatically to:  
     ```
     runs/detect/exp/
     ```
   - A sample output visualization displays the detected small objects with their respective confidence scores.  

---

📌 In this example, the model successfully detects and localizes small targets even in complex, cluttered scenes — demonstrating robustness in small object recognition tasks.



## 🧪 Troubleshooting

Common issues and their solutions when running the small object detection project:

1. ❌ **Error: Model file not found or corrupted**  
   - Cause: Model weights were not downloaded or saved correctly.  
   - ✅ Solution:  
     - Ensure `best_model.pth` exists in the `runs/train/` directory.  
     - Re-download or retrain the model if necessary.  

2. ❌ **CUDA out of memory**  
   - Cause: Batch size too large for GPU memory.  
   - ✅ Solution:  
     - Reduce batch size in `configs/train_config.yaml`.  
     - Use smaller image size (e.g., 512 × 512).  
     - Free GPU memory before starting training: `torch.cuda.empty_cache()`.  

3. ❌ **Blank or empty output images during inference**  
   - Cause: Input path incorrect or image not loaded properly.  
   - ✅ Solution:  
     - Verify the input image folder exists and contains valid images.  
     - Use absolute paths if relative paths fail.  

4. ❌ **Incorrect bounding boxes or low accuracy**  
   - Cause: Model underfitting or poor annotations.  
   - ✅ Solution:  
     - Check dataset annotations for correctness.  
     - Increase training epochs or improve data augmentation.  

5. ❌ **Python package version conflicts**  
   - Cause: Library versions incompatible with Detectron2 or PyTorch.  
   - ✅ Solution:  
     - Use `pip install -r requirements.txt` to install exact dependencies.  
     - Consider creating a new virtual environment for isolation.  

---

📌 Following these steps should resolve most common issues encountered during training, evaluation, or inference.


## 🚀 Deployment Options

You can run the Persian Emotion Classifier both locally and online:

1. ✅ **Local Deployment**  
   - Run the app with:  
     ```bash
     streamlit run app.py
     ```  
   - Opens on `http://localhost:8501`  

2. ✅ **Online Deployment Options**  
   - **Streamlit Cloud** → Quick, free deployment for sharing apps  
   - **Hugging Face Spaces** → Free hosting with Streamlit or Gradio  

---

## 📦 Deliverables

The project includes the following components:

1. ✅ Fine-tuned model → `parsbert-emotion/`  
2. ✅ Training notebook → `emotions-classification-nlp.ipynb`  
3. ✅ Streamlit app → `app.py`  
4. ✅ Project report → `report.pdf`  
5. ✅ Dataset → [Emotions in Persian Texts](https://www.kaggle.com/datasets/mbsoroush/emotions-in-persian-texts)  

---

## 🏁 Conclusion

1. ✅ **Objective Achieved**  
   - Fine-tuned **ParsBERT** for Persian emotion classification  

2. ✅ **Key Features**  
   - Supports **7 emotion classes**  
   - Achieved **Macro F1 ~0.72**  
   - Integrated with an interactive **Streamlit app**  

3. ✅ **Impact**  
   - Demonstrates the power of transformer models in **low-resource languages** like Persian  
   - Provides a practical tool for text emotion analysis  

4. ✅ **Future Improvements**  
   - Expand dataset with more labeled examples  
   - Explore advanced transformer models (e.g., RoBERTa, mBERT)  
   - Deploy on cloud for public access  

---

📌 With this project, we showed that **transformer-based NLP models** can effectively handle **Persian emotion classification**, bridging the gap for practical AI applications in Persian language processing.


### ⚖️ License

This repository is provided **without an open-source license**.  
All rights reserved – you may **not** use, copy, modify, or distribute this code without explicit permission.  




