---
layout: page
title: Speech Denoising
description: 
img: assets/img/9_project/contour.jpg
importance: 2
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
        {% include figure.html path="assets/img/9_project/diagram.jpg" title="example image" class="img-fluid d-block mx-auto w-40 w-md-60 w-lg-80" %}
</div>

Below, we outline various methods employed in signal processing tasks, including Normalized Least Mean Squares, Newton's method, and the Affine Projection Algorithm (APA), each offering distinct approaches to optimize model parameters and enhance the efficiency and accuracy of signal processing applications.

-	**Normalized Least Mean Squares**: A variant of the Least Mean Squares algorithm, it adjusts filter coefficients based on the correlation between input signals and error signals, offering a computationally efficient approach for adaptive filtering tasks.
-	**Newton's Method (APA2)**: Utilizes second-order derivatives to iteratively refine parameter estimates, often employed for optimizing non-linear functions, including in signal processing tasks to improve convergence speed.
-	**Affine Projection Algorithm (APA1)**: A method for adaptive filtering that incorporates a linear predictor to estimate future signal values, enabling effective noise reduction and channel equalization in signal processing applications.

The update equations for each of the algorithms, with lambda set to 0.00001 for all experiments, are as follows:

<div class="container text-center">
        {% include figure.html path="assets/img/9_project/formulas.jpg" title="example image" class="img-fluid d-block mx-auto w-20 w-md-25 w-lg-30" %}
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
        {% include figure.html path="assets/img/9_project/contour.jpg" title="example image" class="img-fluid d-block mx-auto w-10 w-md-25 w-lg-50" %}
</div>

### Weights Tracks
Below, the weight values as a function of iterations are plotted for each algorithm. To improve interpretability, only every 100th weight value is displayed. The leftmost weight track corresponds to NLMS and exhibits considerable noise, with notable fluctuations observed in the first 50,000 iterations. In contrast, the APA2 method is expected to exhibit greater stability in weight updates compared to NLMS, as it lacks the scaling factor of 2 in the gradient term. As seen in the center plot of Figure 3, while the weights still fluctuate dramatically in the first 50,000 iterations, the jumps are less pronounced than those seen with the NLMS method.

The APA1 method, however, is anticipated to yield noisier weight tracks than the other algorithms because it does not include the autocorrelation function in its weight update. The autocorrelation function, which describes how data points in a time series relate to preceding points, is crucial for understanding the performance surface. The rightmost plot of Figure 3 confirms that the weight track obtained from the APA1 method is indeed noisier than those from both the NLMS and Newton's methods.

<div class="container text-center">
        {% include figure.html path="assets/img/9_project/wt.jpg" title="example image" class="img-fluid d-block mx-auto w-100 w-md-100 w-lg-250" %}
</div>
<div class="caption">
    Figure 3: From left to right, weight tracks obtained from using the 2-tap NLMS, Newton's method/APA2, and APA 1-family algorithms.
</div>

### Frequency Response
The below figure displays the frequency response of 2-tap filters obtained using different algorithms: NLMS, Newton's method, and APA1. In the leftmost plot, the frequency response obtained by NLMS is displayed, revealing a high-pass filter characteristic. We anticipate identical frequency responses across all 2-tap systems. In the center plot, the frequency response obtained by Newton’s method closely resembles that of the NLMS, aligning with expectations as both algorithms converge to similar solutions. Similarly, in the rightmost plot, the frequency response from the APA1 method mirrors that of the previous 2-tap systems, as anticipated.

<div class="container text-center">
        {% include figure.html path="assets/img/9_project/fr.jpg" title="example image" class="img-fluid d-block mx-auto w-100 w-md-100 w-lg-250" %}
</div>
<div class="caption">
    Figure 4: From left to right, frequency responses obtained from using the 2-tap NLMS, Newton's method/APA2, and APA 1-family algorithms.
</div>

### Learning Curves
Figure 5 illustrates the learning curves generated by our 2-tap NLMS, Newton's method, and APA1 algorithms. In the leftmost plot, the mean squared error (MSE) smoothly converges after fifty thousand iterations, reaching an optimal solution by seventy thousand iterations. Due to the absence of scaling in the gradient term of the weight update equation, Newton’s method should yield a smoother learning curve than NLMS, as seen in the center plot. However, in the rightmost plot, the APA1 method's learning curve exhibits more noise in the first ten thousand iterations due to the absence of an autocorrelation function in the weight update, resulting in more jumps. Compared to the other learning curves, this one is notably noisier.
<div class="container text-center">
        {% include figure.html path="assets/img/9_project/lr.jpg" title="example image" class="img-fluid d-block mx-auto w-100 w-md-100 w-lg-250" %}
</div>
<div class="caption">
    Figure 5: From left to right, learning curves obtained from using the 2-tap NLMS, Newton's method/APA2, and APA 1-family algorithms.
</div>

## <u>Optimal Filter</u>
The next part of the assignment involves searching for optimal hyperparameters, namely the filter order and learning rate. This section involves displaying the frequency response, SNR, filter performance as a function of learning rate, the misadjustment, and discussing the convergence of the algorithms in non-stationary environments.

### Frequency Response
The figure below displays the frequency response of optimal filters obtained using three different algorithms (from left to right): NLMS, Newton's method, and APA1. As expected, the leftmost plot of Figure 6 shows more peaks and troughs than the two-tap filters, with a notable trough at the human speech frequencies, which are highlighted in red. The center plot also exhibits several peaks and troughs, similar to NLMS, due to comparable filter orders, with a slight peak at the human speech frequencies. In the rightmost plot, we observe fewer peaks and troughs than in the NLMS and Newton’s method, due to a smaller filter order, but still see a trough at the human speech frequencies, indicated in red.
<div class="container text-center">
        {% include figure.html path="assets/img/9_project/fropt.jpg" title="example image" class="img-fluid d-block mx-auto w-100 w-md-100 w-lg-250" %}
</div>
<div class="caption">
    Figure 6: From left to right, frequency responses obtained from using the NLMS, Newton's method/APA2, and APA 1-family algorithms with optimal filter orders. Human speech frequencies are outlined in red.
</div>

### SNR Improvement Estimation
Echo Return Loss Enhancement (ERLE) quantifies the reduction in noise level achieved by an adaptive signal-processing algorithm. It does not make assumptions about the speech signal, making it a reliable performance metric for this task. During hyperparameter tuning, the model that maximizes ERLE also minimizes the mean squared error (MSE), indicating the best possible model. In this case, the best model is the Newton’s method approach with a filter order of 34 and a learning rate of 0.3. The Signal-to-Noise Ratio (SNR) improvement obtained by the optimal-tap Newton’s method is approximately 18.06 dB, slightly lower than that achieved by the NLMS and APA1 algorithms.

| Algorithm               | SNR Improvement (dB) |
|-------------------------|----------------------|
| NLMS                    | 19.97                |
| Newton’s method/APA2    | 18.06                |
| APA1                    | 19.77                |

### Effect of learning rate

Figure 7 displays the learning curves for various hyperparameters. The leftmost plots show the learning curves for the NLMS algorithm, where the MSE increases after a certain learning rate, particularly evident in the hyperparameter search (upper leftmost plot). The center plots illustrate the learning curves for Newton’s method, exhibiting a similar MSE increase after a specific learning rate. The rightmost plots present the learning curves for the APA1 algorithm, showing an MSE increase after a learning rate of 0.03, consistent with the patterns observed in the other algorithms.
<div class="container text-center">
        {% include figure.html path="assets/img/9_project/lropt.jpg" title="example image" class="img-fluid d-block mx-auto w-100 w-md-100 w-lg-250" %}
</div>
<div class="caption">
    Figure 7: From left to right, learning curves obtained from using the NLMS, Newton's method/APA2, and APA 1-family algorithms. The top plots show the learning curves as a function of learning rate and the bottom plots show the learning curves with an optimal filter order.
</div>

### Misadjustment
According to Agrawal et al., misadjustment quantitatively measures how much the final mean square error deviates from the minimum mean square error achieved by the Wiener filter. It is a useful metric for assessing the cost of adaptability in an algorithm. Among the algorithms tested, the optimal-tap APA1 algorithm exhibited the lowest misadjustment.


| Algorithm               | Misadjustment       |
|-------------------------|---------------------|
| NLMS                    | 9.51 × 10<sup>-17</sup> |
| Newton’s method/APA2    | 2.63 × 10<sup>-6</sup>  |
| APA1                    | 9.52 × 10<sup>-18</sup> |

### Algorithm convergence
These adaptive signal-processing algorithms analyze incoming speech signals to learn their characteristics. To accomplish this, they maintain a history (filter order) of the speech, using these stored samples to converge towards an optimal solution. Convergence typically happens faster with stationary signals compared to non-stationary ones. In non-stationary scenarios, the algorithm requires time to adapt to the changing signal characteristics and converge to a solution aligned with the updated signal statistics.

In a non-stationary environment, I expect NLMS to converge more quickly than the APA1 algorithm but slightly slower than Newton’s method. The solutions should be similar to those obtained by Newton’s method and better than APA1, due to the presence of the autocorrelation function in the update equation. APA2 is expected to converge the quickest in non-stationary conditions, outperforming both NLMS and APA1. Conversely, APA1 converges the slowest in a non-stationary environment because the lack of an autocorrelation function provides less information about the performance surface.

## <u>Conclusion</u>
NLMS, Newton's method, and an APA 1-family algorithm were used to denoise a noisy signal. Optimal hyperparameters were identified, with NLMS (filter order 34, learning rate 0.3) producing the clearest audio. The final output revealed a recording of Queen Amidala from Star Wars Episode I: The Phantom Menace saying, "I will not condone a course of action that will lead us to war."

<div class="container text-center">
        {% include figure.html path="assets/img/9_project/fin.jpg" title="example image" class="img-fluid d-block mx-auto w-50 w-md-75 w-lg-100" %}
</div>
