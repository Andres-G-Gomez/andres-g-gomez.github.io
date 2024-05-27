---
layout: page
title: Chest X-Ray Classification 
description: 
img: assets/img/8_project/imageClassification.jpg
importance: 
category: Research
related_publications: 
pdf_link_text: 
pdf_file_path: 
---

## <u>Project Objective</u>
The goal is to design and train an image classification network to categorize X-ray images into one of four classes: COVID-19, Normal, Lung Opacity, and Viral Pneumonia.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/imageClassification.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


## <u>Dataset Overview</u>
The COVID-19 Chest X-Ray dataset is a comprehensive collection of X-ray images categorized into four distinct classes. This dataset is designed to aid in the development and evaluation of machine learning models for image classification and segmentation tasks related to lung diseases. Below is a detailed breakdown of the dataset:
- **COVID-19 Positive:** 3,616 images
- **Normal:** 10,192 images
- **Lung Opacity (Non-COVID lung infection):** 6,012 images
- **Viral Pneumonia:** 1,345 images
- **Masks:** Respective masks for each category

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/lungs.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Image samples from each class: A. Covid, B. Lung Opacity, C. Viral Pneumonia, D. Normal.
</div>


## <u>Image Classification Models</u>
Various state-of-the-art image classification models were evaluated in this project, each offering unique architectural features and performance characteristics. These models include VGG16, ResNet18, DenseNet121, ResNet152, and Vision Transformer (ViT). VGG16, known for its simplicity and effectiveness, comprises 16 layers and achieved remarkable accuracy on the ImageNet dataset. ResNet18 and ResNet152 utilize residual connections to address the vanishing gradient problem, offering deeper architectures with improved performance. DenseNet121 employs dense blocks for efficient feature reuse, while ViT represents a breakthrough in computer vision with its transformer architecture, allowing for the capture of long-range dependencies in images. By exploring these diverse models, we aimed to identify the most suitable architecture for accurate and robust classification of COVID-19 Chest X-Ray images. 

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/vgg16.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
I. **VGG16**
    - Introduced in 2014 by the University of Oxford.
    - Achieved 92.7% top-5 accuracy on ImageNet.
    - Consists of 16 layers: 13 convolutional and 3 fully connected.
    - Total parameters: 138 million.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/resnet18.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
II. **ResNet18**
   - Introduced in 2015 by Microsoft.
   - Utilizes shortcut connections to mitigate the vanishing gradient problem.
   - Comprises 72 layers with 11 million parameters.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/densenet121.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
III. **DenseNet121**
   - Introduced in 2016 by Facebook AI Research.
   - Features dense blocks with repeated convolution operations.
   - Comprises 120 convolutions with 8 million parameters.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/resnet152.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
IV. **ResNet152**
   - Introduced in 2015.
   - Contains 152 layers with 60 million parameters.
   - Known for its depth and high parameter count.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/8_project/vit.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
V. **Vision Transformer (ViT)**
   - Introduced in 2020 by Google Research and Brain Team.
   - Utilizes transformer architecture to capture long-range dependencies.


## <u>Methods</u>

### Data Augmentation Techniques
The paper "A Review of Medical Image Data Augmentation Techniques for Deep Learning Applications" (2021) discussed various augmentation methods, including
- **Basic Augmentation:** Geometric transforms, cropping, noise injection, etc.
- **Deformable Augmentation:** Spline interpolation, deformable image registration, etc.
- **Deep Learning Augmentation:** GAN-based methods and others.

In this project, we conducted extensive experiments to evaluate the performance of various image classification models on the COVID-19 Chest X-Ray dataset. We tested different data augmentation techniques to improve model generalization and robustness. The experiments were designed to compare the performance with and without data augmentation, specifically focusing on:
- i. No Augmentation: Baseline for comparison
- ii. Left-right flip
- iii. Shrink and pad, left-right flip

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

### Training Setup
During the training phase, careful attention was paid to various hyperparameters and optimization techniques to ensure effective model training and convergence. The following configuration details were employed:

- **Batch Size**: The batch size refers to the number of training examples utilized in one iteration. A batch size of 32 was chosen to balance computational efficiency and model stability.

- **Criterion**: The criterion, or loss function, is a measure of the model's performance during training. In this project, both Cross Entropy Loss and its variants, Weighted Cross Entropy Loss and Balanced Cross Entropy Loss, were explored to address class imbalances in the dataset. These loss functions were chosen for their effectiveness in multi-class classification tasks.

- **Optimizer**: Stochastic Gradient Descent (SGD) with momentum 0.9 was employed as the optimizer. SGD is a widely-used optimization algorithm for training deep neural networks. The momentum term helps accelerate SGD in relevant directions and dampens oscillations.

- **Learning Rate Scheduler**: A step learning rate scheduler with 20 steps and a decay factor (gamma) of 0.9 was utilized to adjust the learning rate during training. This scheduler reduces the learning rate by a factor of gamma after a certain number of epochs, allowing for finer adjustments as training progresses.

- **Training Duration**: The models were trained for a maximum of 400 epochs or until the validation loss showed signs of continuous increase. This early stopping mechanism helps prevent overfitting and ensures that the model is trained for an optimal number of epochs.

By carefully tuning these hyperparameters and employing optimization techniques, the training process aimed to strike a balance between model convergence, generalization, and computational efficiency.


## <u>Results</u>

### Training and Validation Accuracy

The experimental results demonstrate the efficacy of different data augmentation techniques and model architectures in classifying COVID-19 Chest X-Ray images. With no augmentation, DenseNet achieved a training accuracy of 95% and a validation accuracy of 90%. Left-right flipping augmentation further improved performance across all models, with VGG16 achieving a validation accuracy of 94% and ResNet18 achieving 86%. 

**Left-right Flip Augmentation:**

| Model       | Train Acc (w/ Aug) | Train Acc (w/o Aug) | Val Acc (w/ Aug) | Val Acc (w/o Aug) |
|-------------|--------------------------------------|-----------------------------------------|----------------------------------------|-------------------------------------------|
| DenseNet121 | 1.00                                 | 0.95                                    | 0.94                                   | 0.90                                      |
| VGG16       | 1.00                                 | 0.94                                    | 0.94                                   | 0.91                                      |
| ResNet18    | 0.86                                 | 0.85                                    | 0.85                                   | 0.84                                      |
| ViT         | 0.97                                 | n/a                                     | 0.91                                   | n/a                                       |

Additionally, the combination of shrinking, zero padding, and left-right flipping demonstrated promising results, with DenseNet achieving a validation accuracy of 93%. These findings underscore the importance of data augmentation in enhancing model robustness and generalization. Further exploration of model architectures, such as Vision Transformer, holds potential for improving classification accuracy and addressing complex medical image analysis tasks.

**Shrink and Pad, Left-right Flip Augmentation:**

| Model       | Train Acc (w/ Aug) | Train Acc (w/o Aug) | Val Acc (w/ Aug) | Val Acc (w/o Aug) |
|-------------|--------------------------------------|-----------------------------------------|----------------------------------------|-------------------------------------------|
| DenseNet121 | 1.00                                 | 0.95                                    | 0.93                                   | 0.90                                      |
| VGG16       | 1.00                                 | 0.94                                    | 0.94                                   | 0.91                                      |
| ResNet152   | 0.84                                 | 0.88                                    | 0.85                                   | 0.87                                      |


### Test Accuracy

The test accuracy results validate the models' robustness on unseen data. With left-right flipping augmentation, DenseNet and VGG16 achieved test accuracies of 94% and 95%, respectively. The combination of shrinking, zero padding, and left-right flipping resulted in competitive performance, with DenseNet reaching a test accuracy of 93%. These findings affirm the models' effectiveness in accurately classifying COVID-19 Chest X-Ray images, indicating their potential for practical use in clinical and research settings.

**Left-right Flip Augmentation:**

| Model       | Test Acc (w/ Aug) | Test Acc (w/o Aug) |
|-------------|----------------------------------|-------------------------------------|
| DenseNet121 | 0.94                             | 0.94                                |
| VGG16       | 0.95                             | 0.94                                |
| ResNet18    | 0.86                             | 0.86                                |
| ViT         | 0.90                             | n/a                                 |


**Shrink and Pad, Left-right Flip Augmentation:**

| Model       | Test Acc (w/ Aug) | Test Acc (w/o Aug) |
|-------------|----------------------------------|-------------------------------------|
| DenseNet121 | 0.93                             | 0.94                                |
| VGG16       | 0.93                             | 0.94                                |
| ResNet152   | 0.84                             | 0.86                                |


## <u>Conclusion</u>
This project highlights the application of various data augmentation techniques and advanced image classification models to categorize COVID-19 chest X-ray images effectively. DenseNet121 and VGG16 models demonstrated high accuracy, particularly with data augmentation. The Vision Transformer also showed promising results, indicating the potential of transformer architectures in medical image classification tasks.




