---
layout: page
title: Speech Denoising
description: 
img: assets/img/9_project/contour.jpg
importance: 3
category: Academic
related_publications: 
pdf_link_text: 
pdf_file_path: 
---

## <u>Project Overview</u>
We were tasked with reducing the noise of a noisy audio file containing human speech using three algorithms: Normalized Least Mean Squares, Newton's method, and Affine Projection Algorithm (APA). First, we use two-tap filters. This helps us examine the performance surface contour, and compare the weight tracks, learning curves, and frequency responses. Next, we find the optimal filter order and examine similar metrics. 

Finally, we determine the signal-to-noise ratio improvement and misadjustment. The signal-to-noise ratio (SNR) measures the clarity of a signal by comparing the level of the desired signal to the level of background noise, indicating how well a filtering or processing technique has enhanced the signal quality. The misadjustment evaluates how well an adaptive filter is working by measuring how far the filter's settings are from their ideal values. This helps determine how effectively and efficiently the filter is adapting to changes.


## <u>Algorithmic Overview</u>
The basic implementation of each of these algorithms is illustrated in Figure 1. Noise serves as the input to our system. The filter is characterized by two main hyperparameters: the filter length and the individual weights of each delay. Predictions are generated and compared against the desired output (the noisy signal) to calculate an error. A learning algorithm is then applied to update these hyperparameters. Finally, to obtain a clean signal (just the speech itself), we subtract our predictions from the desired noisy signal.
<div class="container text-center">
        {% include figure.html path="assets/img/9_project/diagram.jpg" title="example image" class="img-fluid d-block mx-auto w-100 w-md-100 w-lg-150" %}
</div>

Below, we outline various methods employed in signal processing tasks, including Normalized Least Mean Squares, Newton's method, and the Affine Projection Algorithm (APA), each offering distinct approaches to optimize model parameters and enhance the efficiency and accuracy of signal processing applications.

-	**Normalized Least Mean Squares**: A variant of the Least Mean Squares algorithm, it adjusts filter coefficients based on the correlation between input signals and error signals, offering a computationally efficient approach for adaptive filtering tasks.
-	**Newton's Method (APA2)**: Utilizes second-order derivatives to iteratively refine parameter estimates, often employed for optimizing non-linear functions, including in signal processing tasks to improve convergence speed.
-	**Affine Projection Algorithm (APA1)**: A method for adaptive filtering that incorporates a linear predictor to estimate future signal values, enabling effective noise reduction and channel equalization in signal processing applications.

The update equations for each of the algorithms, with lambda set to 0.00001 for all experiments, are as follows:

<div class="container text-center">
        {% include figure.html path="assets/img/9_project/formulas.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>

## <u>Key Metrics</u>
The following bullet points describe key metrics and visualizations used to evaluate the performance and behavior of adaptive filtering algorithms:

- **Weight Tracks**: Monitor the evolution of filter coefficients over time, providing insights into the stability and convergence behavior of the algorithm.
- **Frequency Response**: Shows how a filter attenuates or amplifies different frequency components of the input signal, useful for analyzing the filter's effectiveness in tasks like noise reduction or signal enhancement.
- **Learning Curves**: Plot the performance of an algorithm, typically the error or loss, against the number of iterations or training time, offering information about the convergence rate, stability, and overall efficiency of the learning process.


## <u>2-Tap Filter</u>
The first part of the assignment involves trying various algorithms with two weights. This section involves displaying the performance surface, the weight tracks, learning curves, frequency responses, and the SNR improvement.

### Performance Surface Contours
The performance surface visualizes the mean squared error (MSE) across a range of possible weight values. This surface is consistent for all 2-tap systems. As shown in Figure 2, the minimum MSE is achieved with weight values of approximately 0.5 and 1.4.

<div class="container text-center">
        {% include figure.html path="assets/img/9_project/contour.jpg" title="example image" class="img-fluid d-block mx-auto w-100 w-md-100 w-lg-300" %}
</div>

### Weights Tracks
Below, the weight values as a function of iterations are plotted for each algorithm. To improve interpretability, only every 100th weight value is displayed. The leftmost weight track corresponds to NLMS and exhibits considerable noise, with notable fluctuations observed in the first 50,000 iterations. In contrast, the APA2 method is expected to exhibit greater stability in weight updates compared to NLMS, as it lacks the scaling factor of 2 in the gradient term. As seen in the center plot of Figure 3, while the weights still fluctuate dramatically in the first 50,000 iterations, the jumps are less pronounced than those seen with the NLMS method.

The APA1 method, however, is anticipated to yield noisier weight tracks than the other algorithms because it does not include the autocorrelation function in its weight update. The autocorrelation function, which describes how data points in a time series relate to preceding points, is crucial for understanding the performance surface. The rightmost plot of Figure 3 confirms that the weight track obtained from the APA1 method is indeed noisier than those from both the NLMS and Newton's methods.

<div class="container text-center">
        {% include figure.html path="assets/img/9_project/wt.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>

### Frequency Response
The below figure displays the frequency response of 2-tap filters obtained using different algorithms: NLMS, Newton's method, and APA1. In the leftmost plot, the frequency response obtained by NLMS is displayed, revealing a high-pass filter characteristic. We anticipate identical frequency responses across all 2-tap systems. In the center plot, the frequency response obtained by Newton’s method closely resembles that of the NLMS, aligning with expectations as both algorithms converge to similar solutions. Similarly, in the rightmost plot, the frequency response from the APA1 method mirrors that of the previous 2-tap systems, as anticipated.

<div class="container text-center">
        {% include figure.html path="assets/img/9_project/fr.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>

### Learning Curves
Figure 5 illustrates the learning curves generated by our 2-tap NLMS, Newton's method, and APA1 algorithms. In the leftmost plot, the mean squared error (MSE) smoothly converges after fifty thousand iterations, reaching an optimal solution by seventy thousand iterations. Due to the absence of scaling in the gradient term of the weight update equation, Newton’s method should yield a smoother learning curve than NLMS, as seen in the center plot. However, in the rightmost plot, the APA1 method's learning curve exhibits more noise in the first ten thousand iterations due to the absence of an autocorrelation function in the weight update, resulting in more jumps. Compared to the other learning curves, this one is notably noisier.
<div class="container text-center">
        {% include figure.html path="assets/img/9_project/lr.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>

## <u>Optimal Filter</u>
The next part of the assignment involves searching for optimal hyperparameters, namely the filter order and learning rate. This section involves displaying the frequency response, SNR, filter performance as a function of learning rate, the misadjustment, and discussing the convergence of the algorithms in non-stationary environments.

### Frequency Response
...To be completed soon
<div class="container text-center">
        {% include figure.html path="assets/img/9_project/fropt.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>

### Learning Curves
<div class="container text-center">
        {% include figure.html path="assets/img/9_project/lropt.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>
