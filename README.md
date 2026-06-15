# 🧠 Advanced Handwritten Digit Recognition Pipeline

   ![Python](https://shields.io)
   ![TensorFlow](https://shields.io)
   ![Keras](https://shields.io)
   ![License](https://shields.io)
   ![Maintenance](https://shields.io)

An advanced deep learning training pipeline implementing a custom Convolutional Neural Network (CNN) in TensorFlow/Keras to classify the MNIST Handwritten Digits Dataset (0–9). 

## 🚀 Key Engineering Highlights
* **Modern Activation:** Replaced legacy ReLU with **GELU (Gaussian Error Linear Unit)** for smoother gradient flow and superior validation convergence.
* **Robust Callbacks:** Integrated dynamic learning rate adjustment (`ReduceLROnPlateau`) and training termination safeguards (`EarlyStopping`) to prevent overfitting.
* **Production-Ready Serialization:** Automatically serializes complete weights and compiler configurations into the secure, native `.keras` format.

---

## 🏗️ Model Topology Architecture

The network processes `28x28x1` input tensors through an optimized feature extraction and classification backend:

```text
Input (28x28x1) 
   │
   ├──► Conv2D (20 filters, 5x5, Valid Padding, GELU) ──► MaxPooling2D (2x2, Stride 2)
   │
   ├──► Conv2D (60 filters, 5x5, Valid Padding, GELU) ──► MaxPooling2D (2x2, Stride 2)
   │
   ├──► Flatten ──► Dense (100, GELU) ──► Dense (50, GELU)
   │
   └──► Dropout (0.2) ──► Dense (10, Softmax Output)
```

---

## ⚡ Quick Start & Installation

### 1. Clone the Project
```bash
git clone https://github.com
cd mnist-cnn-gelu
```

### 2. Setup Virtual Environment & Dependencies
```bash
python -m venv venv
# On Windows use: venv\Scripts\activate
source venv/bin/block/activate  

pip install --upgrade pip
pip install tensorflow keras numpy
```

### 3. Run Training Execution Pipeline
```bash
python train.py
```

---

## ⚙️ Hyperparameter Configuration

| Parameter | Operational Target Value | Description |
| :--- | :--- | :--- |
| **Optimizer** | Adam | Adaptive Moment Estimation Engine |
| **Learning Rate** | 0.001 | Initial optimization step size |
| **Batch Size** | 64 | Minibatch dimensions for GPU parallelism |
| **Total Epochs**| 15 | Upper termination ceiling threshold |
| **Loss Function**| Sparse Categorical Crossentropy | Optimized loss criteria for integer targets |

---

## 📄 License
Distributed under the **MIT License**. See `LICENSE` inside the repository layout folder for more details.
