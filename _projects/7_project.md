---
layout: page
title: Chest X-RAY Segmentation
description: 
img: assets/img/7_project/segmentation_model.jpg
importance: 4
category: Research
related_publications: 
pdf_link_text: 
pdf_file_path: 
---

## <u>Project Objective</u>
Our objective was to design and train an image segmentation network to predict masks of chest X-ray images, aiding in the diagnosis and analysis of lung diseases.

<div class="container text-center">
        {% include figure.html path="assets/img/7_project/segmentation_model.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>


## <u>Dataset Overview</u>
The COVID-19 Chest X-Ray dataset comprises 21,165 X-ray images categorized into four classes: COVID-19 positive (3,616), Normal (10,192), Lung Opacity (6,012), and Viral Pneumonia (1,345), along with respective masks for segmentation tasks.


## <u>Image Segmentation Models</u>

Our selection of cutting-edge image segmentation models, including TransUNet, Swin UNet, and InternImage-T, excels in capturing fine details and structures within medical images. Leveraging advanced techniques such as transformers and hierarchical feature learning, these models deliver precise segmentation results, making them ideal candidates for our segmentation tasks.

<div class="container text-center">
        {% include figure.html path="assets/img/7_project/transunet.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>

I. TransUNet
  -	Combines the strengths of transformers and U-Net architecture for enhanced medical image segmentation.
  -	Leverages self-attention mechanisms to capture long-range dependencies in the input image.
  -	Achieves state-of-the-art performance on multiple medical imaging benchmarks, particularly in organ and tumor segmentation tasks.

<div class="container text-center">
        {% include figure.html path="assets/img/7_project/swinunet.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>

II. Swin UNet
  -	Utilizes Swin Transformer blocks within a U-Net framework, allowing for hierarchical feature learning and improved context aggregation.
  -	Employs a shifting window approach to efficiently handle high-resolution images and maintain computational efficiency.
  -	Demonstrates superior performance in various medical image segmentation challenges, including brain tumor and lung lesion segmentation.

<div class="container text-center">
        {% include figure.html path="assets/img/7_project/internimage.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>

III. InternImage-T (59M parameters)
  -	Integrates internal attention mechanisms with convolutional layers for refined feature extraction and segmentation precision.
  -	Focuses on capturing intricate patterns and textures in medical images, enhancing segmentation accuracy for complex structures.
  -	Proven effectiveness in segmenting diverse medical imaging modalities, such as MRI and CT scans, with notable improvements over traditional methods.


## <u>Methods</u>
### Data Augmentation Techniques
In this project, we conducted extensive experiments to evaluate the performance of various image segmentation models on the COVID-19 Chest X-Ray dataset. We tested different data augmentation techniques to improve model generalization and robustness. The experiments were designed to compare the performance with and without data augmentation, specifically focusing on:
-	i. No data augmentation
-	ii. Data augmentation - Zoom in and center crop + left-right flip. 

### Training Setup
- **Batch Size**: 32
- **Criterion**: Binary Cross Entropy (BCE) loss
- **Optimizer**: Adam with lr=1e-4
- **Learning Rate Scheduler**: ReduceLROnPlateau with patience of 25 epochs
- **Training Duration**: Up to 500 epochs or until validation loss increases continuously.


## <u>Results</u>
The training results indicate that both the TransUNet (TU) and Swin UNet models achieved comparable mean training Binary Cross Entropy (BCE) values with and without augmentation, with values around 0.0223. Similarly, the mean validation BCE values were consistent between the augmented and non-augmented scenarios, hovering around 0.0393. These results suggest that augmentation did not significantly impact the model's performance in terms of BCE. (*Note: at the time of writing this article, the training results for the InternImage models was lost).

**Training Results:**

| Models     | Mean Training BCE | Mean Validation BCE |
|------------|-------------------|---------------------|
| TransUNet  | 0.0223            | 0.0393              |
| Swin Unet  | 0.0223            | 0.0393              |

The test set results show that both the TransUNet and Swin UNet models achieved high mean dice scores, with values of approximately 0.951 and 0.954, respectively, when augmentation was applied. Without augmentation, the dice scores remained high, with values around 0.963 for TransUNet and 0.960 for Swin UNet. Additionally, the mean Hausdorff distance (HD95) values were relatively low, ranging from approximately 2.076 to 4.091 millimeters across different models and augmentation scenarios, indicating accurate segmentation performance. Notably, the InternImage model demonstrated exceptionally high dice scores of 0.9801 and 0.9824 for 6k and 60k iterations, respectively, showcasing superior segmentation accuracy compared to the other models.

**Test Results:**

| Models                      | Mean dice | Mean hd95 (mm) |
|-----------------------------|-----------|-----------------|
| TransUNet (With Augmentation) | 0.9509    | 3.0122          |
| Swin Unet (With Augmentation)  | 0.9544    | 4.091           |
| InternImage (6k iterations) | N/A       | 2.283           |
| InternImage (60k iterations) | N/A       | 2.076           |


Below, we showcase qualitative results obtained from the InternImage-T and SwinUNet models.


<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/7_project/swinunetResults.jpg" title="Example Image 1" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/7_project/swinunetResults2.jpg" title="Example Image 2" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Segmentation results for SwinUNet are displayed. On the left, input images, original masks, and predicted segmentation results from both data augmentation and no augmentation experiments are shown. On the right, the predicted mask is overlaid on the input image for visualization.
</div>

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/7_project/6kInternimage.jpg" title="Example Image 1" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/7_project/60kInternimage.jpg" title="Example Image 2" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Segmentation results for InternImage-T models are showcased. Each image displays, from left to right, the ground truth, the predicted mask, and the non-overlapping image. The left and right images correspond to the 6k and 60k iteration InternImage-T models, respectively.
</div>


## <u>Conclusion</u>
The project successfully developed and evaluated image segmentation models for chest X-ray images, showcasing competitive performance and highlighting the effectiveness of transformer-based architectures in medical image analysis. Further research could explore ensemble methods and domain-specific pretraining to enhance segmentation accuracy further.
