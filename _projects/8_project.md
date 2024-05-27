---
layout: page
title: COVID-19 Chest X-Ray Classification Project
description: 
img: assets/img/8_project/imageClassification.jpg
importance: 
category: Research
related_publications: 
pdf_link_text: 
pdf_file_path: 
---

## Dataset Overview
The dataset used for this project consists of:
- **COVID-19 Positive:** 3,616 images
- **Normal:** 10,192 images
- **Lung Opacity (Non-COVID lung infection):** 6,012 images
- **Viral Pneumonia:** 1,345 images
- **Masks:** Respective masks for each category

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/lungs.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Image samples from each class: A. Covid, B. Lung Opacity, C. Viral Pneumonia, D. Normal.
</div>

## Project Objective
The goal is to design and train an image classification network to categorize X-ray images into one of four classes: COVID-19, Normal, Lung Opacity, and Viral Pneumonia.

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/imageClassification.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Data Augmentation Techniques
Inspired by the paper "A Review of Medical Image Data Augmentation Techniques for Deep Learning Applications" (2021), various augmentation methods were applied:
- **Basic Augmentation:** Geometric transforms, cropping, noise injection, etc.
- **Deformable Augmentation:** Spline interpolation, deformable image registration, etc.
- **Deep Learning Augmentation:** GAN-based methods and others.

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/lungsFlipped.jpg" title="Example Image 1" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/shrink.jpg" title="Example Image 2" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Data augmentation examples: The left image demonstrates a basic left-right flip, while the right image illustrates a combination of shrinking, zero padding, and a left-right flip.
</div>

## Image Classification Models
Five models were evaluated for this task:

<div class="row">
    <div class="col-12">
        <img src="assets/img/8_project/vgg16.jpg" alt="Example Image" class="img-fluid rounded z-depth-1 float-end ms-3 mb-3" style="width: 300px;">
        <p>
    1. **VGG16**
   - Introduced in 2014 by the University of Oxford.
   - Achieved 92.7% top-5 accuracy on ImageNet.
   - Consists of 16 layers: 13 convolutional and 3 fully connected.
   - Total parameters: 138 million.
        </p>
        <p>
            By using the `float-end` class, the image is aligned to the right, allowing the text to wrap around it naturally. This layout is particularly effective in articles, reports, and portfolio projects where visuals play a complementary role to the written content.
        </p>
    </div>
</div>



2. **ResNet18**
   - Introduced in 2015 by Microsoft.
   - Utilizes shortcut connections to mitigate the vanishing gradient problem.
   - Comprises 72 layers with 11 million parameters.

3. **DenseNet121**
   - Introduced in 2016 by Facebook AI Research.
   - Features dense blocks with repeated convolution operations.
   - Comprises 120 convolutions with 8 million parameters.

4. **ResNet152**
   - Introduced in 2015.
   - Contains 152 layers with 60 million parameters.
   - Known for its depth and high parameter count.

5. **Vision Transformer (ViT)**
   - Introduced in 2020 by Google Research and Brain Team.
   - Utilizes transformer architecture to capture long-range dependencies.

## Methods

### Experiments and Data Augmentation
Two sets of data augmentation techniques were tested:
- **Left-right flip**
- **Shrink and pad, left-right flip**

### Models Evaluated
- ResNet18
- ResNet152
- VGG16
- DenseNet121
- Vision Transformer (ViT)

### Training Setup
- **Batch Size:** 32
- **Loss Function:** Cross Entropy Loss (weighted vs. balanced)
- **Optimizer:** SGD with momentum (0.9)
- **Learning Rate Scheduler:** Step scheduler (20 steps, gamma = 0.9)
- **Epochs:** 400 or until validation loss increases continuously

## Results

### Training and Validation Accuracy

**Left-right Flip Augmentation:**

| Model       | Training Accuracy (w/ Aug) | Training Accuracy (w/o Aug) | Validation Accuracy (w/ Aug) | Validation Accuracy (w/o Aug) |
|-------------|--------------------------------------|-----------------------------------------|----------------------------------------|-------------------------------------------|
| DenseNet121 | 1.00                                 | 0.95                                    | 0.94                                   | 0.90                                      |
| VGG16       | 1.00                                 | 0.94                                    | 0.94                                   | 0.91                                      |
| ResNet18    | 0.86                                 | 0.85                                    | 0.85                                   | 0.84                                      |
| ViT         | 0.97                                 | n/a                                     | 0.91                                   | n/a                                       |

**Shrink and Pad, Left-right Flip Augmentation:**

| Model       | Training Accuracy (w/ Aug) | Training Accuracy (w/o Aug) | Validation Accuracy (w/ Aug) | Validation Accuracy (w/o Aug) |
|-------------|--------------------------------------|-----------------------------------------|----------------------------------------|-------------------------------------------|
| DenseNet121 | 1.00                                 | 0.95                                    | 0.93                                   | 0.90                                      |
| VGG16       | 1.00                                 | 0.94                                    | 0.94                                   | 0.91                                      |
| ResNet152   | 0.84                                 | 0.88                                    | 0.85                                   | 0.87                                      |

### Test Accuracy

**Left-right Flip Augmentation:**

| Model       | Test Accuracy (With Augmentation) | Test Accuracy (Without Augmentation) |
|-------------|----------------------------------|-------------------------------------|
| DenseNet121 | 0.94                             | 0.94                                |
| VGG16       | 0.95                             | 0.94                                |
| ResNet18    | 0.86                             | 0.86                                |
| ViT         | 0.90                             | n/a                                 |

**Shrink and Pad, Left-right Flip Augmentation:**

| Model       | Test Accuracy (With Augmentation) | Test Accuracy (Without Augmentation) |
|-------------|----------------------------------|-------------------------------------|
| DenseNet121 | 0.93                             | 0.94                                |
| VGG16       | 0.93                             | 0.94                                |
| ResNet152   | 0.84                             | 0.86                                |

## <u>Conclusion</u>
This project highlights the application of various data augmentation techniques and advanced image classification models to categorize COVID-19 chest X-ray images effectively. DenseNet121 and VGG16 models demonstrated high accuracy, particularly with data augmentation. The Vision Transformer also showed promising results, indicating the potential of transformer architectures in medical image classification tasks.




