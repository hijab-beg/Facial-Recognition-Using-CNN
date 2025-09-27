Here’s your content formatted in clean GitHub-ready Markdown:

---

## 1. Introduction

The objective of this assignment is to explore the application of deep learning for affective computing, specifically for facial expression recognition. The task involves developing a system capable of analyzing facial images to perform two distinct but related tasks:

**Categorical Classification:** Classifying the facial expression into one of eight discrete emotion categories (Neutral, Happy, Sad, Surprise, Fear, Disgust, Anger, Contempt).
**Continuous Domain Prediction:** Predicting two continuous-valued metrics—Valence (how positive or negative the emotion is) and Arousal (how exciting or calming the emotion is).

To achieve this, a multi-task learning approach was adopted. Three different pre-trained Convolutional Neural Network (CNN) architectures—ResNet50, DenseNet121, and VGG16—were fine-tuned on the provided dataset. This report details the dataset used, the preprocessing and augmentation techniques applied, the architectural modifications made, and provides a comprehensive comparison and analysis of the performance of each model based on the results generated in the accompanying Kaggle notebook.

---

## 2. Dataset Description

The project utilizes a dataset of facial images, each annotated with a rich set of labels. The dataset is split into three parts for training, validation, and final evaluation.

* **Training Set:** 11,196 images (including augmented versions to improve generalization)
* **Validation Set:** 600 unique images used to monitor training and detect overfitting
* **Test Set:** 600 unique unseen images for final evaluation

Each image is annotated for expression, valence, and arousal.

---

## 3. Data Preprocessing and Augmentation

All images were resized to **224×224** pixels to match the input size of the pre-trained models. Separate transformations were used for training versus validation/test.

**Training Transforms:**

* Random Horizontal Flip (50% probability)
* Random Rotation (up to 15°)
* Color Jitter (brightness, contrast, saturation)
* Normalization (ImageNet mean and std)

**Validation & Test Transforms:**

* Resizing
* Normalization
  (No augmentations applied to preserve evaluation integrity)

---

## 4. Model Architectures

A multi-head setup was used: each model’s final classification layer was replaced with three separate heads to output expression, valence, and arousal.

* **ResNet50:** 50-layer residual network with skip connections to address vanishing gradients
* **DenseNet121:** Dense connections enabling feature reuse and parameter efficiency
* **VGG16:** Classic 16-layer CNN using stacked 3×3 convolutions, but with a high parameter count

All were pre-trained on ImageNet.

---

## 5. Training Setup

| Parameter      | Value                                             |
| -------------- | ------------------------------------------------- |
| Optimizer      | Adam                                              |
| Learning Rate  | 1e-4                                              |
| Loss Functions | Expression: Cross-Entropy<br>Valence/Arousal: MSE |
| Epochs         | 15                                                |
| Batch Size     | 32                                                |
| Environment    | Kaggle Notebook (NVIDIA Tesla T4 GPU)             |

The total loss was the sum of the three individual task losses.

---

## 6. Results and Comparison

### Categorical Classification (Expression)

| Metric        | ResNet50 | DenseNet121 | VGG16  |
| ------------- | -------- | ----------- | ------ |
| Accuracy      | 0.4417   | 0.4650      | 0.4417 |
| F1-Score      | 0.4424   | 0.4554      | 0.4444 |
| Cohen's Kappa | 0.3620   | 0.3878      | 0.3624 |

### Continuous Domain (Valence & Arousal)

**Valence**

| Metric | ResNet50 | DenseNet121 | VGG16  |
| ------ | -------- | ----------- | ------ |
| RMSE   | 0.4227   | 0.3861      | 0.3859 |
| CORR   | 0.5150   | 0.5933      | 0.5492 |
| CCC    | 0.5075   | 0.5727      | 0.4811 |

**Arousal**

| Metric | ResNet50 | DenseNet121 | VGG16  |
| ------ | -------- | ----------- | ------ |
| RMSE   | 0.3696   | 0.3554      | 0.3693 |
| CORR   | 0.4078   | 0.4654      | 0.3981 |
| CCC    | 0.3836   | 0.4434      | 0.3697 |

---

## 7. Discussion and Analysis

Severe overfitting was consistently observed across all models. Training loss dropped to very low values (e.g., ~0.05 for ResNet50), but validation loss rose sharply after a few epochs (e.g., ~1.8 to ~3.3 for ResNet50, ~1.6 to ~3.2 for DenseNet121). This indicates the models were memorizing the training data rather than learning generalizable features. Data augmentation alone was insufficient to prevent overfitting.

**Model Comparison:**

* **DenseNet121** delivered the best overall performance across nearly all metrics.
* **ResNet50** and **VGG16** showed similar but weaker results, especially in regression tasks.
* DenseNet121 was also the fastest to train, averaging ~1m 51s per epoch vs. ~2m 38s for VGG16 and ~2m 40s for ResNet50.

**Metric Rationale:**

* Accuracy, F1-score, and Cohen’s Kappa were appropriate for classification, especially with potential class imbalance.
* RMSE quantified prediction error, while CORR and CCC better captured the relationship between predictions and ground truth for valence/arousal.

---

## 8. Conclusion

This assignment demonstrated multi-task deep learning for facial affect analysis using three pre-trained CNN architectures. **DenseNet121** emerged as the most effective and efficient model.

The major limitation was **severe overfitting** across all architectures. Future improvements should prioritize regularization strategies such as early stopping, increased dropout, or learning rate scheduling to improve generalization and real-world applicability.

---

Let me know if you want headings numbered, capitalized differently, or tables styled another way.
