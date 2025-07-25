**🧠🔬 Semantic Segmentation: Brain Tumor Detection 🩻**

---

### 🌐 Dataset

* **Source
  **: [KaggleHub Dataset - Brain Tumor Segmentation](https://www.kaggle.com/datasets/nikhilroxtomar/brain-tumor-segmentation)
* **Structure**:

    * Images: 2D grayscale images of brain scans.
    * Masks: Binary masks corresponding to tumor locations.

![](assets/img_0.png)
![](assets/img_1.png)
![](assets/img_2.png)

---

### ⚖️ Preprocessing Pipeline

1. **Image Loading**: Read grayscale images and masks using OpenCV.
2. **Resizing**: All images and masks resized to `224x224`.
3. **Filtering Channels**:

    * Canny edge detectors with multiple thresholds.
    * Directional gradients using Sobel + angle computation.
4. **Normalization**:

    * Input range scaled to `[-1, 1]`.
    * Masks binarized to `-1` (background) and `1` (tumor).
5. **Channel Stacking**:

    * Input: Original + 3 edge-based channels (total 4).
    * Output: Single binary mask.

![](assets/img_3.png)
![](assets/img_4.png)
![](assets/img_5.png)

---

### 🔧 Augmentation

* Applied on-the-fly using NumPy-based custom logic:

    * Horizontal flip
    * Random rotation (± 10 degrees)
    * Random crop and resize
    * Brightness/contrast shift
    * Gaussian noise

---

### 📊 Data Split

* Train / Validation / Test split using `train_test_split`
* Controlled with seed: `42`

---

### ⚖️ Data Generator

* `MyDataGenerator`:

    * Efficient I/O using `ThreadPoolExecutor`
    * Caching mechanism for repeated loading
    * On-the-fly augmentation

---

### 📊 Loss & Metrics

* **Loss**: Dice Loss (custom)
* **Metrics**:

    * Dice Coefficient
    * Intersection over Union (IoU)

---

### 🧱 Model Architectures

* **Shared components**:

    * Encoder-decoder structure (U-Net based)
    * Squeeze-and-Excitation blocks
    * Skip connections

#### 📈 Model 1: `unet_v1`

* UpSampling-based decoder
* Uses `SE blocks` in bottleneck only

![](assets/img_6.png)

#### 📈 Model 2: `unet_v2`

* Conv2DTranspose-based decoder
* `SE blocks` used in decoder as well

![](assets/img_7.png)

---

### 🌟 Training

* Optimizer: Adam
* Epochs: 70
* Callbacks:

    * EarlyStopping (patience=5)
    * ModelCheckpoint (best weights)
    * ReduceLROnPlateau
    * HistoryCheckpoint (custom logger)

---

### 🎯 Evaluation

* Quantitative:

    * Trained models evaluated on test set
    * Metrics printed: Dice, IoU, Loss
* Qualitative:

    * Plots showing:

        * Input
        * Ground truth mask
        * Predicted mask
        * Differences
        * Overlays (image + mask / prediction)

![](assets/img_8.png)
![](assets/img_9.png)
![](assets/img_10.png)
![](assets/img_11.png)
![](assets/img_12.png)
![](assets/img_13.png)
![](assets/img_14.png)
![](assets/img_15.png)

---

### 📷 Visuals

* Random raw samples (image + mask)
* Augmented sample comparisons
* Preprocessed stacked channels
* Training/validation curves

    * Loss
    * Dice
    * IoU
* Model predictions: train / val / test

![](assets/img_16.png)
![](assets/img_17.png)
![](assets/img_18.png)
![](assets/img_19.png)
![](assets/img_20.png)
![](assets/img_21.png)


---

### 🎓 Notes

* Dataset normalization plays key role in training stability.
* Custom augmentations designed specifically for medical imagery.
* SE blocks improve learning of semantic context.
* Canny filters and direction-based channels boost edge-based understanding.

---

### 📦 Folder Structure Suggestion

```
.
├── models_cache/
│   ├── *.weights.h5
│   ├── *.history.json
│   └── *_arch.png
├── drive/MyDrive/brain_tumor_outputs/
├── plots/
├── results/
├── notebook.ipynb
└── README.md
```

---
