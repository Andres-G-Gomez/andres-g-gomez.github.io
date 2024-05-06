---
layout: page
title: project 4
description: Tight Risk Bounds for Distracted Driver Detection
img:
importance: 
category: Academic
---

**I. <u>Introduction</u>**
The leading cause of fatal accidents in the U.S. is due to distracted driving. Although self-driving technologies are becoming commercially prevalent in the U.S., drivers are expected to be fully alert, prepared to take over at any moment. Many newer vehicles are equipped with driver alert systems, which monitor a driver’s behavior either through data collected from external sensors or through a camera system and warn or alert the driver when unsafe behavior is detected. This work will apply and compare various deep learning models to the publicly available State Farm Distracted Driver Detection dataset as well as estimate tighter risk bounds of false positives for distracted drivers than would generally be obtained through confidence intervals. Risk bound characterizations, like binomial proportion confidence intervals, will be determined.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/images_drivers.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 1: Sample images from the State Farm Distracted Driver Dataset containing alert drivers (A, B) and distracted drivers (C, D). 
</div>

**II. <u>Methods</u>**

**A. <u>Data Sampling and Augmentation</u>**

Due to RAM limitations and computational time, a sample of the original dataset set was selected. A sample of about 1130 images was selected for training/validation. This dataset was then split into a training and validation set by partitioning the data with an 80/20 split. To evaluate the accuracy of the estimated BPCI for the precision, 5 different tests sets, each containing approximately 225 images, were sampled from the original dataset. These test sets will be labeled A-E in later sections. All images were sampled from the original dataset without replacement. To save additional memory and computational time, each RGB image was down sampled to a 150x150 image size. To account for noise in the data and enhance in generalizability, data augmentation was applied. The main transformation that was added were random rotations within a 0-to-45-degree range. This angle was decided upon after looking through the dataset and seeing that images were captured at varying angles.

**B. <u>Convolution Neural Network Frameworks</u>**

Two architectures, shown layer-by-layer in Table 1, were trained, and tested. The first network that was evaluated was one similar to the SCNNB, developed by Lei, Dai, and Ling (2020) [8]. The simplicity of the frameworks allows for faster training. The size of the input data is 150x150x3. As described in [8], SCNNB first extracts data features by a 3x3 convolution with 32 filters. Next, a 2x2 max-pooling layer is applied to reduce the dimensionality as well as the training
time.

Afterwards, two sequential 3x3 convolution layers with 64 filters are applied to extract additional features. The output is fed to a 2x2 max-pooling layer. Then, a fully connected layer of 1280 neurons is applied, followed by another fully connected layer with 512 neurons. In the original SCNNB, the last layer is a softmax output layer and is used to achieve multi-classification. Since this problem is a binary classification problem, the last layer was changed to a single neuron with a sigmoid activation function. 

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/table1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 1: Architectures of the two CNNs that were trained and evaluated. Framework 1 corresponds to the CNN trained from     scratch. Framework 2 utilizes transfer learning techniques. 
</div>

**C. <u>Transfer Learning with Xception</u>**

Framework 2 had the Xception model as the base model. The Xception model, developed by Francois Chollet in 2016, is a leading image classification CNN. In a competition involving over 350 million photos with 17,000 classes, it dramatically outperformed the previous competition leader. The main difference between the Xception model and other leading CNN architectures is it assumes cross-channel and spatial patterns can be analyzed separately. So, the architecture first separates the channels, identifies spatial features from each channel, then applies depth-wise 1x1 convolution to look for cross-channel patterns amongst the extracted features [9]. These depthwise convolutions cannot caprute spatial patterns, since they are of size 1x1. Hence, their role is to primarily extract deppthwise information from the spatial information extracted across each channel. 

After including the imagenet weights and discarding the top layer, an architecture very similar to framework 1 was constructed on top. The layer that follows the Xception base model is a 32 filter, 5x5 convolution layer, followed by a
2x2 max-pooling layer. Two sequential 5x5 convolution layers with 64 filters are applied, followed by a 2x2 maxpooling layer. The last layer is a fully connected layer of 256 neurons followed by a 0.25 dropout. The last layer is a single neuron with a sigmoid activation function.

The loss function for both frameworks was binary cross entropy, with an Adam optimizer with a learning rate of 0.0005. As previously mentioned, the performance metric was precision. Both frameworks were tested with 100 epochs, a patience of 20, and a batch size of 64. 

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/xception.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2: Diagrammatic representation of the Xception architecture. 
</div>

**II. <u>Training Evaluation</u>**

Initially, the two frameworks presented in Table 1, were evaluated on the validation set. Table 2 shows the F1-score and BPCI at 95% confidence for the precision obtained by each framework on the validation set. We see that Framework 2 obtained a higher F1-score as well as tighter risk bounds. Hence, the remaining results will be carried out for Framework 2 only. 

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/table2.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 2: F1-score and BPCI of the precision at 95% confidence obtained by each Framework on the validation set. 
</div>

**III. <u>Results</u>**

We evaluated the accuracy of the estimated 95% BPCI for the precision on each of the test sets, labeled A-E in later sections. Additionally, we tracked the precision, recall and F1-score for each of the test sets. We can see from Table 3, that the 95% BPCI was fairly consistent on all of the test sets. The lower bound of the 95% BPCI for precision ranged between 0.31 and 0.79, and the upper bound ranged between 4.26 and 5.57. The values for precision, recall, and the F1-score were all consistent as well. 

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/table3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Table 3: The 95% BPCI for precision, the precision, recall and F1-score obtained by Framework 2 on each of the 5 tests sets. 
</div>

**IV. <u>Conclusion</u>**

Two CNN architectures were trained on a sample of the State Farm Distracted Driver Detection dataset to identify distracted drivers. The first framework was inspired by the shallow convolutional neural network, presented by Lei et al. The second framework utilized Xception as the base model, followed by convolution and fully connected layers. Additionally, binomial proportion confidence intervals were obtained to determine the true proportion of false positives, defined as distracted drivers falsely labeled as not distracted.

As outlined in Table 2, framework 2 outperformed framework one in terms of F1-score and tighter binomial proportion confidence intervals were also obtained for framework 2. On 5 separate test sets, the F1-score ranged between 0.96 and 0.98, the recall ranged between 0.961 and 0.985, and the Precision ranged between 0.976 and 0.985. Additionally, the 95% BPCI for precision remained consistent amongst all of the individual test sets. The lower bound ranged between 0.31 and 0.79, and the upper bound ranged between 4.26 and 5.57. 

**V. <u>References</u>**

[1] M. Shah and S. Shanian, “Hold-out Risk Bounds for Classifier Performance Evaluation,” 2009.

[2] N. Highway Traffic Safety Administration and U. Department of Transportation, “Distracted Driving 2020,” 2020.

[3] A. Pal, S. Kar, and M. Bharti, “Algorithm for Distracted Driver Detection and Alert Using Deep Learning,” Optical Memory and Neural Networks (Information Optics), vol. 30, no. 3, pp. 257–265, Jul. 2021, doi: 10.3103/S1060992X21030103.

[4] T. Abbas et al., “Deep Learning Approach Based on Residual Neural Network and SVM Classifier for Driver’s Distraction Detection,” Applied Sciences (Switzerland), vol. 12, no. 13, Jul. 2022, doi: 10.3390/app12136626.

[5] M. Wöllmer et al., “On-line Driver Distraction Detection using Long Short-Term Memory.”

[6] S. Masood, A. Rai, A. Aggarwal, M. N. Doja, and M. Ahmad, “Detecting distraction of drivers using Convolutional Neural Network,” Pattern Recognit Lett, vol. 139, pp. 79–85, Nov. 2020, doi: 10.1016/J.PATREC.2017.12.023.

[7] Y. Maximov and D. Reshetova, “Tight risk bounds for multi-class margin classifiers,” Pattern Recognition and Image Analysis, vol. 26, no. 4, pp. 673–680, Oct. 2016, doi: 10.1134/S105466181604009X.

[8] Lei, F., Liu, X., Dai, Q. Shallow convolutional neural network for image classification. SN Appl. Sci. 2, 97 (2020).

[9] F Chollet. “Xception: Deep Learning with Depthwise Separable Convolutions” 

{% endraw %}
