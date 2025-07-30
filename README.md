# 🧠 X2CT-GAN: Reconstructing 3D CT from 2D X-ray Views  
📘 Re-implementation of the X2CT-GAN paper  
📝 Paper: "Reconstructing 3D CT Volumes from Orthogonal 2D X-ray Views using GAN"

---

## 📌 Overview  
This project implements **X2CT-GAN**, a generative adversarial network designed to reconstruct **3D CT volumes** using only **two orthogonal 2D X-ray images** (frontal and lateral).  
The model reduces radiation exposure and cost by replacing hundreds of X-ray projections traditionally required for CT reconstruction with just two views.

---

## 🧩 Motivation  
- Traditional CT: requires 180~1000 X-ray projections from multiple angles → high radiation + cost  
- X2CT-GAN: generates 3D volumes from **just 2 X-rays** using GAN, solving **2D → 3D dimensional expansion**  
- Introduces feature fusion, multiple loss functions, and 3D-aware upsampling to achieve high-quality reconstructions  

---

## 🧠 Key Features

- 🧾 Uses **only two orthogonal X-rays** (frontal + lateral)  
- 🔄 Learns to infer missing depth (Z-axis) using a **2D-to-3D generator**  
- 🧬 Combines **MSE Loss**, **Projection Loss**, and **Adversarial Loss**  
- 🧠 Introduces **feature fusion** across views using multi-connection modules (A, B, C)

---

## 🏗 Network Architecture

### 🎯 Generator (G)

- Two parallel 2D encoders for frontal and lateral X-rays
- Feature fusion modules:
  - **Connection A**: Global structure (Fully Connected → 3D reshape)  
  - **Connection B**: Local feature expansion (2D → pseudo-3D → true 3D)  
  - **Connection C**: Spatial alignment of orthogonal views (Permute & Average)
- 3D decoder to generate final CT volume using 3D upsampling and convolutions

### 🧪 Discriminator (D)

- 3D Patch-based discriminator  
- Takes either reconstructed CT or projected 2D X-ray  
- Learns to distinguish real vs. fake CT volumes or projections

---

## 🧮 Loss Functions

| Loss Type           | Purpose                                                        |
|---------------------|----------------------------------------------------------------|
| 🧠 MSE Loss          | Aligns reconstructed CT with ground truth voxel-by-voxel       |
| 🎯 Projection Loss   | Projects reconstructed CT → 2D and compares with input X-rays  |
| 🔍 Adversarial Loss  | Ensures realism of generated CT using GAN                     |
| 📦 Total Objective   | Weighted sum of above three losses                             |

---

## 🧪 Dataset

- **CT Volume**: LIDC-IDRI Dataset (~1018 chest CT scans)  
- **Synthetic X-rays**: Generated using **DRR** (Digitally Reconstructed Radiographs)  
- **Real X-rays**: 200 collected samples used to train **CycleGAN** to convert synthetic X-rays → realistic appearance

---

## 🔄 Data Generation Pipeline

1. CT volumes → DRR → Frontal + Lateral synthetic X-rays  
2. CycleGAN: Makes synthetic X-rays look realistic using unpaired real X-ray dataset  
3. X2CT-GAN: Takes 2 X-rays → generates 3D CT volume  
4. Projected CT used to match input X-rays (Projection Loss)  
5. Final volume evaluated using Discriminator (Adversarial Loss)

---

## ⚙️ Training Details

- Optimizer: `Adam`  
  - Learning rate: `2e-4`  
  - β₁: `0.5`, β₂: `0.99`  
- Epochs: `100`  
  - After 50 epochs → Linear Learning Rate Decay  
- Normalization: `InstanceNorm`  
- Discriminator: 3D PatchGAN with Conv3D + ReLU  

---

## 🧠 Technical Stack

- `PyTorch`, `NumPy`, `OpenCV`  
- `DRR generation`, `CycleGAN`, `3D CNN`, `Transposed Conv3D`  
- Visualizations: `Matplotlib`, `Seaborn`, `Tensorboard`

---

## 🎯 Highlights

- ✅ Dimensional expansion: 2D → 3D mapping successfully achieved  
- ✅ Projection consistency ensures alignment between CT and original X-rays  
- ✅ Dual-view X-rays significantly outperform single-view X-ray reconstruction  
- ✅ CT volumes are quantitatively and qualitatively similar to ground truth  

---

## 🩻 Qualitative Results

![Generator Output](https://user-images.githubusercontent.com/your_image_here.png)

---

## 📁 Folder Structure

